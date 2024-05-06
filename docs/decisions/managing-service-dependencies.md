# Managing Service Dependencies

## Context and Problem Statement

As we split Open Food Facts up into smaller, more manageable, services, it becomes increasingly difficult for developers to know what additional services need to be deployed in order for the service they are working on to function correctly. This becomes increasingly difficult if the referenced service itself has other dependencies.

Note that for development and testing purposes it is recommended that any dependencies are mocked or managed using tools like testcontainers, rather than relying on a separate service being up. This document concerns itself more with creating a workable end-to-end deployment that can be used for ad-hoc, interactive testing and discovery.

This document is specifically concerned with "internal" dependencies where shared state is needed, e.g. each service can have its own independent database rather than relying on a shared database service, but the messaging system (Redis) needs to be shared in order for messages to pass between services. Dependencies on services outside the scope of Open Food Facts are referred to as "external" dependencies.

## Decision Drivers

* the definition of dependencies should be as close as possible to the particular service's source code
* existing tooling should be used wherever possible
* the mechanism to pull in any dependencies should be as simple and as fast as possible
* any mechanism needs to be compatible with all supported development environments, e.g. local code, dev containers, GitPod
* it should be relatively straightforward for a developer to switch from a simple "latest" deployment of a referenced service to a version that is also undergoing local development

## Considered Options

* Use the docker compose include command to reference dependencies in sibling folders
* Develop a custom tool that works from a list of dependencies defined in a file

## Decision Outcome

Custom tooling will be used so that we can cope with circular dependencies and to allow loading of dependencies to be performed using a single command.

This will be achieved as follows:

Each project will have a makefile in its root folder with a target named "run". This target will fetch any missing dependencies from GitHub into peer directories, change into each of the directories and execute `make -e run`. Note it is important to make each directory the working directory when running to ensure that local `.env` files are imported.

The `make run` target must function when only the root folder of the project is cloned, e.g. when using `git clone --filter=blob:none --sparse ...` and should use the latest images from GitHub (i.e. not require a local build) by default. Note that the initiating project could override the image tag used from "latest" to "dev" to use a local container.

If a project anticipates that it could be part of a circular reference then it should defend for this by setting a temporary environment variable, e.g.

```make
run:
ifndef PROJECT1_RUNNING
# Fetch and start dependencies
	@export PROJECT1_RUNNING=true; \
	for dep in "project2" "project3" ; do \
		if [ ! -d ../$$dep ]; then \
			git clone --filter=blob:none --sparse \
				https://github.com/openfoodfacts/$$dep.git ../$$dep; \
		fi; \
		cd ../$$dep && make -e run; \
	done
# Start myself
	docker compose -p project1 up -d
endif
```

Note that the make `ifndef` command and comments mustn't be indented.

Other projects may set the COMPOSE_PROJECT_NAME environment variable so it is important to use the `-p` parameter in `docker compose` to ensure that each dependency is loaded into its own a consistent project name.

### Consequences

* Good, because a single command can be run to bring up a service with all of its dependencies
* Good, because each repository can use its own docker project
* Neutral, because cloning referenced projects may take time. It is suggested that a filtered clone is performed, e.g. `git clone --filter=blob:none --sparse https://github.com/openfoodfacts/service.git`
* Neutral, environment variables need to be unique across projects to avoid conflicts (unless they refer to the same thing), e.g. database username and password variables would need to be prefixed with the owning service name, whereas the REDIS_URL could be shared
* Bad, because this will be something else that developers need to learn about when joining the team

### Confirmation

This approach allows dependencies to be defined in each specific project (albeit using an imposed convention).

Use of a filtered clone should allow dependencies to be loaded relatively quickly (a `blob:none` filter on openfoodfacts-server completes in about 10 seconds).

Tools like GitPod [multi-repository working](https://www.gitpod.io/docs/configure/workspaces/multi-repo) support this approach.

If a developer also wants to work on a referenced service they can easily convert it into a full clone using `git sparse-checkout disable`. Note that the `blob:none` filter is still applied but this should not present too many complications.

## Pros and Cons of the other Options

### Use docker compose include

This could use a docker compose syntax like this:

```
services:
  my-service:
    image: ghcr.io/openfoodfacts/service1:${SERVICE1_TAG}
    build:
      context: .
      dockerfile: Dockerfile

include:
  - ../service2/docker-compose.yml
```

* Good, because it uses a standard docker compose feature
* Good, because it also handles using .env files from the target project
* Neutral, because some kind of custom tooling or manual process is still needed to download the referenced repository
* Bad, because all dependencies get loaded into the same docker project
* Bad, because it can't handle circular dependencies

## More Information

TBA
