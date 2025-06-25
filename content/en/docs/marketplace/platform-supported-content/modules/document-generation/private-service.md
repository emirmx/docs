---
title: "PDF Document Generation"
url: /appstore/modules/document-generation/private-service/
description: "Describes the configuration and usage of the private PDF Document Generation service, which is used in combination with the PDF Document Generation module in the Marketplace."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

For generating PDF documents with the [PDF Document Generation module](/appstore/modules/document-generation/), Mendix currently offers a public, Mendix hosted, PDF document generation service. In some cases, access to this public service is not available, for example for apps running in air-gapped environments.

For these scenarios, Mendix offers a Docker image that can be used to run a (set of) private PDF Document Generation service(s) for Mendix apps. 

### Features {#features}

* Can run as a centralized service or as a container per app. 
* Can run on any container infrastructure that supports Docker as the container runtime, like Kubernetes, AWS ECS, etc.
* No (enforced) rate limits. The actual rate limit depends on the resource configuration and usage of the container.
* Configurable limits for maximum file size and maximum page rendering time.

### Limitations {#limitations}

* Setup, management and monitoring of the service is the responsibility of the customer.
* The service is open to all applications that can access the service. If additional access restrictions are required, these need be setup on network-level, to be configured by the customer.
* Currently, it is not possible to import custom certificate authorities. Apps that use a self-signed or internal certificate are only supported when disabling certificate validation in the service.
* The customer is responsible for setting up a retry mechanism in the application to handle failures or time-outs of the service.

### Prerequisites {#prerequisites}

* You have a good understanding on how to run and manage Docker containers. 
* (Optional) You are familiar with using Helm charts for Kubernetes deployments.
* You are familiar with PDF Document Generation module. For more information see [PDF Document Generation](/appstore/modules/document-generation/).
* Your deployment environment needs to allow bi-directional communication between the Mendix app(s) and the Docker container(s) running the private PDF Document Generation service.

## Installation {#installation}

### Considerations {#considerations}

The service can be setup in several ways, depending on the specific customer needs such as the required isolation level and scalability requirements. In order to install the service, make sure to review the following considerations and adapt your setup accordingly.

#### Resources

The required resources depends on demand, in order to be able to generate 5 documents in parallel, we recommend the following as a minimum:
* CPU: > 2
* Memory: > 4096M

#### Isolation

Requests share the same container resources, which has the following implications:

* Requests in the same container could affect each other, in terms of performance.
* Container crashes could affect all requests being processed at the time of the crash.

Requests in the same container are isolated on browser level, using an incognito browser context per request.

#### Scalability

You can scale the service in two ways:

1. Using **vertical** scaling, with a single container setup

    * One container can serve multiple requests at a time; where requests are processed in parallel using an isolated browser context per request.
    * The browser keeps running after processing a request.
    * No load balancing needed in case of a single container instance.

1. Using **horizontal** scaling, where multiple containers run in parallel.
    * Each container can serve multiple requests at a time.

Running multiple container replicas requires additional load balancing, this is not provided by Mendix. Customers need to configure and use their own preferred load balancing tools, for example [Nginx](https://nginx.org/).

### Installing the service

In order to install the service, the following artifacts are available:

* The Docker image for the PDF Document Generation service. 
* An (optional) Helm chart that can be used to setup the service in case you are using Helm charts to manage your deployments.

#### Install using Docker

Run the docker container using `docker run -p 8085:8085 --name document-generation mendix/document-generation-service:<tag>`, where `<tag>` should be replaced with the version of the service, for example `1.0.0`.

This will create a docker container and expose it on port `8085`.

#### Install using Helm

Install the service using `helm install document-generation <path-to-helm-chart-directory>`.

This will create a Kubernetes deployment and service exposed on port `8085`.

## Configuration {#configuration}

The service has several configuration options to adapt the service to your specific needs. See the table in [section 3.2](#configuration-options) for more details.

### Setting configuration values {#setting-configuration-values}

The approach for setting the configuration values depends on the installation type. Follow the applicable instructions below.

#### Configure using Docker

When using Docker to run the image, add the configuration using the provided environment variable(s), for example: `docker run -p 8085:8085 -e MAX_DOCUMENT_SIZE=<value> --name document-generation mendix/document-generation-service:<tag>`.

#### Configure using Helm

When using Helm, you can configure the options using the `values.yaml` file.

### Available configuration options {#configuration-options}

| Environment variable | Helm chart variable | Default value | Description |
|----------------------|---------------------|---------------|-------------|
| `MAX_DOCUMENT_SIZE` | `maxDocumentSize` | `25000000` (25 MB) | The maximum size for PDF documents generated using the service. When a PDF exceeds this file size, the request gets aborted. |
| `MAX_PAGE_RENDERING_TIME` | `maxPageRenderingTime` | `30000` (30 seconds) | The maximum time to wait for the page to finish loading and rendering. If loading the page exceeds this time, a [Wait for Content](/appstore/modules/document-generation/#wait-for-content-exception) exception will be sent to the module. |
| `ACCEPT_INSECURE_CERTIFICATES` | `acceptInsecureCertificates` | `false` | Allows the use of untrusted certificates, for example when using self-signed certificates. **Warning:** this will disable certificate validation, and will also allow the use of invalid certificates. Be aware of the resulting security risks. |

## Usage {#usage}

### Configure your Mendix app(s)

When you have the Document Generation container running in your environment, you should configure your Mendix application(s) to use the private service.

* Make sure that you are using version 2.1.0 or higher of the PDF Document Generation module.
* Configure the `DocumentGeneration.OverrideServiceType` constant to Private.
* Configure the `DocumentGeneration.ServiceEndpoint` constant to point to the container address and port, for example: `http://document-generation:8085`.
* To generate your first document, follow the instructions in the module documentation [PDF Document Generation](/appstore/modules/document-generation/).

Note: you do not need to register your application when using a private service. In this case, it is therefore also not required to include the Snip_AppRegistration snippet in your app.

#### Logging

All application level errors are sent back to the module, see the [module documentation](/appstore/modules/document-generation/) for more details. 

Technical logs of the service are available on container level. In case you run multiple containers, the logs will be spread across them. We recommend to setup a centralized monitoring solution yourself.