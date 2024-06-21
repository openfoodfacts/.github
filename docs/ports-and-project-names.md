# Service Port Allocations

During local development it is often necessary to have multiple services running at the same time. To make this easier to manage we encourage each service to stick to a specific range of port numbers when exposing services outside of docker, to avoid clashes.

Please raise a PR to update this document if you are introducing a new service into the project.

# Docker Compose Project Names

Each service should load all if its internal dependencies into a unique project.

# Current Port Ranges and Project Names

Port ranges are allocated in blocks of 10.

You do not need to track the individual ports that you use here, please do that in the documentation for the specific project repo.

| Port Range | Project Name | Repo |
|---|---|---|
| 27017 | off_shared | https://github.com/openfoodfacts/openfoodfacts-shared-services |
| 80 | po_off | https://github.com/openfoodfacts/openfoodfacts-server |
| 5500-5509 | robotoff | https://github.com/openfoodfacts/robotoff |
| 5510-5519 | off-query | https://github.com/openfoodfacts/openfoodfacts-query |
| 5600-5609 | openfoodfacts-auth | https://github.com/openfoodfacts/openfoodfacts-auth |


