# Managing Service Dependencies

## Context and Problem Statement

As we split OpenFoodFacts up into smaller, more manageable, services, it becomes increasingly difficult for developers to know what additional services need to be deployed in order for the service they are working on to function correctly. This becomes increasingly difficult if the referenced service itself has other dependencies.

Note that for development and testing purposes it is recommended that any dependencies are mocked or managed using tools like testcontainers, rather than relying on a separate service being up. This document concerns itself more with creating a workable end-to-end deployment that can be used for ad-hoc, interactive testing and discovery.

This document is specifically concerned with "internal" dependencies where shared state is needed, e.g. each service can have its own independent database rather than relying on a shared database service, but the messaging system (Redis) needs to be shared in order for messages to pass between services. Dependencies on services outside the scope of OpenFoodFacts are referred to as "external" dependencies.

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

Each project must provide the following:

* A file with the list of other services it depends on
* a docker compose file which loads the service and any of its external dependencies from the latest available images
* associated list of environment variables with default values that will allow the service to start without intervention
* an optional file (not in source control) that allows the developer to override environment defaults
* all of the files referenced by the above must be in the root folder, to facilitate a sparse checkout

The custom tool will do the following:

* trawl through the dependencies and, if necessary, recursively clone the needed repositories into sibling directories
* ensure that any environment variable overrides defined on the starting project take precedence over defaults in the referenced projects
* run `docker compose up` on each of the referenced dependencies

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
* Bad, because it can't handle circular dependenties

## More Information

TBA
