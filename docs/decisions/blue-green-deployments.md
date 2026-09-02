# Managing Blue-Green Deployments

## Context and Problem Statement

Sometimes it is necessary to have multiple versions of a service running in parallel so that the new version can be initialized and / or tested while the old version is still available. This is commonly referred to as [Blue-green deployment](https://en.wikipedia.org/wiki/Blue%E2%80%93green_deployment).

The typical steps involved in a deployment would be as follows:

1. Deploy new green version (old blue version is still running)
2. Perform any tests and initializations on the green version
3. Update other services to reference the green version where they were previously referencing the blue version
4. Take down the blue version

In the subsequent release blue and green would be swapped above.

This decision record considers how blue-green deployments should be managed.

## Decision Drivers

* it should be easy to determine which is the current deployment (blue or green)
* it must be possible to test the deployment process locally
* it may be desirable to avoid parallel deployments in non-production environments / locally when resources are constrained and a period of down time is acceptable
* it should be easy to switch other services to reference the new version once testing / initialization has been completed

## Considered Options

There are two aspects to this decision:

* How to deploy the alternative service versions
* Switching between the two deployments

### Deployment Mechanism

* Deploy different versions on separate hosts
* Deploy the two versions as separate docker compose projects
* Manage the different versions within a single docker compose project

### Switching Between Deployments

* Use a shared reverse proxy
* Use a reverse proxy on each project
* Target version is configured independently for each referencing service
* Re-deploy the target service with the normal ports re-mapped

## Decision Outcome

Chosen option is to "Manage the different versions within a single docker compose project" and "Re-deploy the target service with the normal ports re-mapped" as this will work equally well for local and production deployments and does not require other services to be re-configured during the transition.

The short period of downtime during the switch from one version to the next would be no different than for normal deployments.

### Consequences

The chosen option is not strictly a "blue-green" approach but more of a "current-next" option with versioned storage. An example of the steps needed to make a transition is provided below:

An existing `example` service with one volume that maps its internal port 80 to 8080 on the host, e.g.:

```
services:
  example:
    image: ghcr.io/openfoodfacts/example:${TAG}
    build: .
    ports:
      - 8080:80
    volumes:
      - dbdata:/var/data
```

The first step in deploying a new version would be to add a new service and volume and update the old service to use the last build image, e.g.

```
services:
  example:
    image: ghcr.io/openfoodfacts/example:sha_of_last_build_of_old_version
    ports:
      - 8080:80
    volumes:
      - dbdata:/var/data

  example_next:
    image: ghcr.io/openfoodfacts/example:${TAG}
    build: .
    ports:
      - 8081:80
    volumes:
      - dbdata_v2:/var/data
```
This will create a new container with the new image version exposed on a different port with a new volume. The old version will be re-created from the previous build image.

When it is time to switch to the new version the docker compose file would be updated as follows:

```
services:
  example:
    image: ghcr.io/openfoodfacts/example:${TAG}
    build: .
    ports:
      - 8080:80
    volumes:
      - dbdata_v2:/var/data
```
Note that the dbdata_v2 volume name is retained. Running `docker compose up --remove-orphans` in this case would remove the `example_next` container and replace the old `example` container with the new version.

Subsequent cleanup would be needed to remove old `dbdata` volume.

### Confirmation

At any point in time the project may be deployed locally which will reflect the current staging / production state.

It is recommended that integration tests are implemented to verify that the old services continue to run during the transition while new services are initializing.

## Pros and Cons of the Options

### Deployment Mechanism

#### Deploy different versions on separate hosts

With this option we would keep the old version running on its current host and deploy the new version on a separate host.

* Good: Requires no changes to the docker configuration
* Good: No need to change exposed port numbers
* Good: Switching versions could be done with a simple DNS change, no need to change port mappings
* Bad: Every container in the project needs to be duplicated
* Bad: May be difficult to balance load between hosts
* Bad: Difficult to test locally

#### Deploy the two versions as separate docker compose projects

For this option the docker compose project name would include the deployment name (blue / green).

* Good: Can test locally
* Good: Containers and volumes will be automatically renamed
* Neutral: Can't use the "reverse proxy per project" option for switching versions
* Bad: Every container in the project needs to be duplicated
* Bad: Exposed port numbers for different versions need to be different

#### Manage the different versions within a single docker compose project

In this option only the specific services that require parallel deployment would be renamed. During the transition, older services would reference the last appropriate build image.

* Good: Can test locally
* Good: Only the containers that need to run in parallel need to be duplicated
* Bad: Need to ensure that all resources that can't be shared by multiple versions (like volumes) are also renamed
* Bad: Exposed port numbers for different versions need to be different during the transition

### Switching Between Deployments

#### Use a shared reverse proxy

This is potentially the most similar to how production environments are managed, using nginx. A reverse proxy service could be added to the shared-services project which would manage routing to the correct version of each service.

* Good: Active version of each service is configured in one place
* Bad: Need to update shared services every time a blue-green deployment is needed
* Bad: All requests have to go through the proxy (this can be challenging in environments where the proxy is on the same host as the calling service)
* Bad: All services have to be exposed externally

#### Use a reverse proxy on each project

Using this approach each project would have its own reverse proxy which would be configured to direct traffic to the appropriate deployment.

* Good: All configuration is within the target project
* Good: Could also be used for scaling within the service, e.g. adding multiple instances of the same service with load balancing
* Bad: Additional latency as all API calls will be going through the proxy
* Bad: All services have to be exposed externally

#### Target version is configured independently for each referencing service

With this approach both versions of the target service would be available on different containers and any applications using these services would need to be updated to reference the new container / port when it is time to switch.

* Good: Services do not need to be exposed outside of the host
* Good: Dependant services can switch to new version when they are ready (if it is, say, a breaking change to an API)
* Bad: Switching services requires updates to every dependant service

#### Re-deploy the target service with the normal ports re-mapped

This option is only applicable to the "Manage the different versions within a single docker compose project" deployment mechanism.

The new service would be deployed in the project under a new name with no ports mapped (or mapped to temporary port numbers). The old service entry would be retained during the transition, referencing the last container image for that version.

When it is time to switch, the new service entry name would be changed to match the original service name with the normal port mappings, and the old service entry removed. Note that the `--remove-orphans` option would need to used to remove the new version under its temporary name.

Subsequent cleanup tasks will be needed to remove old volumes.

* Good: Versions are completely managed within the target service
* Good: Service name and port number for the "active" service does not need to change
* Good: Simple to roll back - just revert last commit which will re-create the container with the old image
* Good: Local checkout will reflect the current deployed state
* Neutral: Old volumes require additional cleanup
* Bad: Developers may not realize that a project is in a "transitioning" state, so the newly built containers will not be exposed on the normal port numbers
* Bad: Project will deploy two versions during the transition
* Bad: There will be a short period of downtime when the old version of the container is replaced with a new one (same as for normal deployments)
