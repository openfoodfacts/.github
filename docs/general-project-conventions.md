# General Project Conventions

Multiple languages and technologies are used throughout Open Food Facts (OFF) but the following general patterns should be used wherever possible.

## Environment Variables

Projects should obtain all of their runtime setting from environment variables.

Every OFF repo has a `.env` file that contains the settings needed by the application to run properly. The `.env` file is loaded by the `docker compose` commands.

The default `.env` file in the repo is ready for local development and should rarely be modified.

In pre-production and production, the `.env` file is populated by the GitHub action (using GitHub environment secrets) before deploying to the target environment.

**Warnings:**

The default `.env` file should rarely change. If you need a different environment locally, create a `.envrc` file to override any specific environment variables.

Do not commit your local .envrc files to the repos!

The project `Makefile` should be configured to load both the `.env` and `.envrc` files and you can use `direnv` to ensure your overrides are recognized in other commands. See [how-to for openfoodfacts-server](https://github.com/openfoodfacts/openfoodfacts-server/blob/main/docs/dev/how-to-use-direnv.md).

Note that `direnv` is capable of loading a `.envrc` file from the parent directory, but the `Makefile` include will not support this so it is recommended that any variables that need to be the same for all projects are duplicated in each project's `.envrc` file.

## Makefile

A `Makefile` proves very useful for wrapping up and centralizing all the commands we run (locally or on remote environments) and have a lighter development and deployment process using simpler aliases.

OFF **contributors** should know that the `Makefile` is the simplest entrypoint for collaborating to OFF repos, although they are not mandatory if the user has a good knowledge of the application at hand.

`Makefile`s should stay away from complexities when possible and be streamlined enough that we can easily understand what the commands stand for.

It is important to be able to switch between the different OFF repositories but keep the same interface to set up our local developer workflow. 

Most of the existing OFF repos try to have the commands below in their `Makefile`:

* `make dev` is the only command needed to set up a complete developer environment after cloning the repo. It should hopefully never fail, but if it does anyway please open an issue to track it.

* `make up`, `make down`, `make hdown`, `make restart`, `make status` map exactly to `docker-compose` commands, respectively `docker-compose up`, `docker-compose down`, `docker-compose down -v`, `docker-compose restart` and `docker-compose ps`.

* `make build` will build the project's images.

* `make run` will ensure that other projects that this project depends on are running (see [Managing Service Dependencies](decisions/managing-service-dependencies.md)). This involves making a shallow clone into the `DEPS_DIR` folder and calling `make run` on each project it needs. It will then start its own services in docker using the latest built images from GitHub (projects should allow the image tag to be overridden if required). `make run` should not require any local software to be available (other than `bash` and `docker`) and should not depend on any files outside of the root directory of the repo (to allow for shallow cloning).

All `Makefile`s will need to explicitly `include` the `.env` and `.envrc` file (if it exists) and then export these variables as otherwise the `Makefile` will not pass environment settings on to target commands. For example:

```Makefile
include .env
-include .envrc

export
```

## Docker Compose

Projects should layer `docker compose` files to support the different ways these might be consumed. The following layers are suggested:

### Raw Configuration

This docker compose file might be included by other projects in their integration tests. In this mode the services will appear in the other project's "test" docker compose project. Any environment settings will come from the parent project.

This docker layer should start the minimum number of services needed to support the interface that the project offers to other projects, fetching pre-built images from GitHub.

It is recommended that no external ports are mapped from these containers and no external volumes should be defined.

### Local Runtime

This layer builds on the above to create a full local deployment of the service that would mirror a production deployment as much as possible. This layer would typically be used by the `make run` target.

Environment settings will come from the `.env` and `.envrc` files. Note that `make run` will not typically use the default `COMPOSE_FILE` and image tag values from the `.env` file as the latter would normally include the Local Development layer (see below).

Services that need to communicate with services in other projects should join the `COMMON_NET_NAME` network. External ports may also be mapped if needed.

This layer should not depend on any files outside of the root directory of the repo (to allow for shallow cloning).

### Local Development

This layer builds on the above by adding the necessary build commands to the docker compose services. This layer would typically be used by the `make build` target.

### Production Deployment

The layer would extend the Local Runtime layer with any production-specific configurations.

