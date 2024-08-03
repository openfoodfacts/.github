# Service Dependencies

This document uses [Mermaid](https://mermaid.js.org/) to visualize the dependencies between services.

In the future we may try to derive this from the `DEPS` declared in each repo. Only services that are exposed on the `COMMON_NET_NAME` network are show, with their respective port numbers.

```mermaid
graph
    subgraph openfoodfacts-server
        frontend:80
    end
    openfoodfacts-server-->openfoodfaces-shared
    openfoodfacts-server-->openfoodfacts-query
    openfoodfacts-server-->openfoodfacts-auth

    subgraph openfoodfacts-query
        query:5511
    end
    openfoodfacts-query-->openfoodfaces-shared

    subgraph openfoodfacts-auth
        keycloak:5600
    end
    openfoodfacts-auth-->openfoodfaces-shared

    subgraph openfoodfaces-shared
        redis:6379
        mongodb:27017
    end
```