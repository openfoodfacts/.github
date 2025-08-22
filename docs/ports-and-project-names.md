# Service Port Allocations

During local development it is often necessary to have multiple services running at the same time. To make this easier to manage we encourage each service to stick to a specific range of port numbers when exposing services outside of docker, to avoid clashes.

Please raise a PR to update this document if you are introducing a new service into the project.

# Docker Compose Project Names

Each service should load all if its internal dependencies into a unique project.

# Events

Each service should describe the events it generates and consumes to using an [AsyncAPI](https://www.asyncapi.com/docs) schema document.

There should be one main AsyncAPI document per service, but each message type that the service generates should be defined in a separate file so that these can be referenced from other services that consume this message. External files can be referenced using the GitHub raw content URL, e.g. https://raw.githubusercontent.com/openfoodfacts/openfoodfacts-server/refs/heads/main/docs/events/messages/product_updated.yaml

The events documentation should also be rendered in HTML. This can be done using the [AsyncAPI HTML Template Generator](https://github.com/asyncapi/html-template). The following can be added to a project's `Makefile` to generate this:

```Makefile
build_asyncapi:
	npm list -g @asyncapi/cli || npm install -g @asyncapi/cli
	cd docs/events && asyncapi generate fromTemplate {repo name}.yaml @asyncapi/html-template@3.0.0 --use-new-generator --param singleFile=true outFilename={repo name}.html --force-write --output=.
```
Substitute repo name for the name of the service's repository.

The generated HTML should be available online and referenced in the table below. Most projects make this available using GitHub pages.

# Current Port Ranges, Project Names and Event descriptions

Port ranges are allocated in blocks of 10.

You do not need to track the individual ports that you use here, please do that in the documentation for the specific project repo.

| Port Range | Project Name | Repo | Events |
|---|---|---|---|
| 6379, 27017 | off_shared | https://github.com/openfoodfacts/openfoodfacts-shared-services |  |
| 80 | po_off | https://github.com/openfoodfacts/openfoodfacts-server | https://openfoodfacts.github.io/openfoodfacts-server/events/openfoodfacts-server.html |
| 5500-5509 | robotoff | https://github.com/openfoodfacts/robotoff |  |
| 5510-5519 | off-query | https://github.com/openfoodfacts/openfoodfacts-query | https://openfoodfacts.github.io/openfoodfacts-query/docs/events/openfoodfacts-query.html |
| 5520-5529 | recipe-estimator | https://github.com/openfoodfacts/recipe-estimator |  |
| 5600-5609 | openfoodfacts-auth | https://github.com/openfoodfacts/openfoodfacts-auth | https://openfoodfacts.github.io/openfoodfacts-auth/docs/events/openfoodfacts-auth.html |


