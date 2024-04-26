# Service Port Allocations

During local development it is often necessary to have multiple services running at the same time. To make this easier to manage we encourage each service to stick to a specific range of port numbers to avoid clashes.

Please raise a PR to update this document if you are introducing a new service into the project.

# Environment Variable Prefixes

When running multiple services together it is sometimes necessary to override a parameter for a specific service. To avoid clashing environment variable names each project is encouraged to use a consistent prefix for their environment variable names.

# Current Port Ranges and Prefixes

Port ranges are allocated in blocks of 10.

Prefixes should be followed by and underscore and then the specific variable name.

You do not need to track the individual ports and environment variables that you use here, please do that in the documentation for the specific project repo.

| Port Range | Prefix | Repo |
|---|---|---|
| 5500 | ROBOTOFF | https://github.com/openfoodfacts/robotoff |
| 5510 | QUERY | https://github.com/openfoodfacts/openfoodfacts-query |
| 5600 | KEYCLOAK | https://github.com/openfoodfacts/openfoodfacts-auth |


