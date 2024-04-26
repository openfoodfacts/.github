# Service Port Allocations

During local development it is often necessary to have multiple services running at the same time. To make this easier to manage we encourage each service to stick to a specific range of port numbers to avoid clashes.

Please raise a PR to update this document if you are introducing a new service into the project.

## Current Port Ranges

Port ranges are allocated in blocks of 10.

You do not need to track the individual ports you use here, please do that in the documentation for the specific project repo.

| Port Range | Repo |
|---|---|
| 5500 | https://github.com/openfoodfacts/robotoff |
| 5510 | https://github.com/openfoodfacts/openfoodfacts-query |
| 5600 | https://github.com/openfoodfacts/openfoodfacts-auth |


