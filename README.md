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
