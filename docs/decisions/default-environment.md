# Default Environment for Cloned Repos

## Context and Problem Statement

If a developer wants to start work on a repo then they should be able to clone the repo and immediately start work after running `make dev`. In this case the `COMPOSE_FILE` environment variable would typically include overrides that perform the build process and images would be given a `dev` tag.

However, we also want to be able to shallow clone projects and run then as dependencies using `make run`. In this case we want the project to start using pre-built images, i.e. .we don't want to use the development `COMPOSE_FILE` overrides, and we want to load the current `main` tagged image from GitHub.

Whichever default we choose will then determine the `COMPOSE_FILE` settings for `make up` or just `docker compose up`.

## Decision Drivers

* it should be simple for a developer to start working on a project
* dependencies should be loaded as quickly and simply as possible
* it should not be complicated for a developer to simultaneously work on one project and another project that depends on it

## Considered Options

* default environment variables should assume development (build) mode of working
* default environment variables should assume run (fetch pre-built images) mode of working

## Decision Outcome

Chosen option: "Default environment variables should support Development Mode", because it reflects the current model and is a more natural fit for most scenarios.

Note that core docker compose files should be as close as possible to production with run, development and production overrides layered on top of these. This is discussed in more detail in the [General Project Conventions](../general-project-conventions.md).

### Consequences

The default settings in a project's `.env` file should assume the developer wants to build the project locally.

The `make run` command will need to override the `COMPOSE_FILE` and image tag to use pre-built images.

It is suggested that `COMPOSE_FILE_RUN` and `IMAGE TAG` environment variables are used by `make run` to allow developers to override these values if they are working on two projects that depend on each other.

### Confirmation

Developers should raise a bug in the repo if `make up` and `docker compose up` do not us the same docker settings as `make dev`.

## Pros and Cons of the Options

### Default to Development Mode

In this case the `COMPOSE_FILE` environment variable would typically include overrides that perform the build process and images would be given a `dev` tag.

The `make run` command would override the `COMPOSE_FILE` value from the environment to exclude an build commands and use a `main` tag for the image.

* Good: Reflects existing model for development
* Good: Standard docker commands will work with development images
* Bad: If a developer wants `make run` to use the `dev` image they will need to add overrides to `.envrc`.

### Default to Run Mode

In this case we want the `COMPOSE_FILE` and image tags to default to using pre-built images.

The `make dev` command would need to explicitly override the `COMPOSE_FILE` to add build steps.

* Bad: `make up` and raw docker commands will not use local images or development overrides
* Bad: If a developer wants `make run` to use the `dev` image they will need to add overrides to `.envrc`.
