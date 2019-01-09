# OpenAPI CodeGen Example with RESTEasy

This project serves as an example of how you can generate server stubs from an OpenAPI (formerly Swagger) document.

## Build Details

This project utilizes [Maven](https://maven.apache.org/) for build management.

The generation of server stubs from the OpenAPI specification document is achieved using the [OpenAPI CodeGen Maven Plugin](https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen-maven-plugin).
This plugin is definited within the `<plugins>` section in the `<build>` section of `pom.xml` and detailed descriptions of the individual parameters for the plugin can be found there.
If additional information is needed for the plugin, you can refer to the [plugin MOJO implementation](https://github.com/swagger-api/swagger-codegen/blob/master/modules/swagger-codegen-maven-plugin/src/main/java/io/swagger/codegen/plugin/CodeGenMojo.java) directly.

## Server Stub Generation

The server stubs are generated based upon the OpenAPI specification document which is found at `src/main/resources/openapi.yaml`.
For this project, we are using RESTEasy for the code generation
The generated code is placed in `target/generated-sources/openapi/src/generated/java/main`.
Note that the specification document location, the technology used, and the generated code target destination are all configurable via the codegen `<plugin>` in `pom.xml`.

#### Specifying API Method Names

The OpenAPI code generation relies on the `operationId` of the specification document to determine the API method name.
It is recommended that you provide an `operationId` as failing to provide one leaves the method name decision in the hands of the code generation (you will get a warning in your build if you don't provide one).
Examples of specifying the `operationId` are given in `src/main/resources/openapi.yaml`.
You can see the resulting methods in the `GreetingApiService` in the code generation folder.

#### Known Issues

There is a [known issue](https://github.com/swagger-api/swagger-codegen/issues/5410) with the OpenAPI codegen maven plugin.
The plugin generates the code as if it were an independent or new project and creates a separate POM with the generated code (in our case, at `target/generated-sources/openapi/pom.xml`).
As a result, upon building the project, the dependencies in the separate POM are not discovered and various compile errors occur.
Until this issue is resolved, the workaround is to copy the `<dependencies>` and `<properties>` from the generated POM into the main project POM.
This should only be necessary when the version of the plugin is updated.
Comments in the main project POM indicate where dependencies and properties have been copied over (and therefore where they need to be updated should the plugin version ever be updated).

In addition, the code generation does not seem to correctly configure jetty for the generated project (trying to follow the directions in `target/generated-soruces/openapi/README.md` errors).
The workaround to this is to add the jetty plugin directly to the main project POM and copy over the `web.xml` from the generated project (`target/generated-sources/openapi/src/main/webapp/WEB-INF/web.xml`) to the main project.
Like above, this copying will need to be repeated should the plugin version be updated and inline comments have been added to `web.xml` to indicate the copied block.
