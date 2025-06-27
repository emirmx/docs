---
title: "Private PDF Document Generation Service"
url: /appstore/modules/private-document-generation-service/
description: "Describes the configuration and usage of the private PDF Document Generation service, which is used in combination with the PDF Document Generation module in the Marketplace."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Mendix offers a public, Mendix-hosted PDF document generation service included with the [PDF Document Generation module](/appstore/modules/document-generation/). 

In some cases, however, access to this public service is not available, such as for apps running in air-gapped environments. For these scenarios, Mendix offers a private PDF Document Generation service for Mendix apps. 

### Features {#features}

These are the main features of the private PDF Document Generation service:

* It can run as a centralized service or as a container per app. 
* It can run on any container infrastructure that supports Docker as the container runtime, such as Kubernetes, AWS ECS, etc.
* It has no enforced rate limits. The actual rate limit depends on the resource configuration and usage of the container.
* It has configurable limits for maximum file size and maximum page rendering time.

### Limitations {#limitations}

These are the limitations of using the private PDF Document Generation service:

* Setup, management, and monitoring of the service is your responsibility.
* The service is open to all applications that can access it. If additional access restrictions are required, you need to set these up at the network level and configure them.
* You cannot import custom certificate authorities. Apps that use a self-signed or internal certificate are only supported when you disable certificate validation in the service.
* You are responsible for setting up a retry mechanism in the application to handle failures or timeouts of the service.

### Prerequisites {#prerequisites}

Before you start using the private PDF Document Generation service, make sure you meet these prerequisites:

* You have a good understanding of how to run and manage Docker containers. 
* You are familiar with the PDF Document Generation module. For more information, refer to [PDF Document Generation](/appstore/modules/document-generation/).
* Your deployment environment needs to allow bidirectional communication between Mendix apps and the Docker containers running the private PDF Document Generation service.

## Installation {#installation}

The service can be set up in several ways, depending on the specific customer needs, such as the required isolation level and the scalability requirements. To install the service, make sure you review the following considerations and adapt your setup accordingly.

### Required Resources

The required resources depend on demand. For instance, to be able to generate 5 documents in parallel, we recommend the following as a minimum:

* CPU: more than 2 CPU cores
* Memory: more than 4096 MB of RAM

### Isolation

Requests share the same container resources, which has the following implications:

* Requests in the same container could affect each other in terms of performance.
* Container crashes could affect all requests being processed at the time of the crash.

Requests in the same container are isolated at the browser level, using an incognito browser context per request.

### Scalability

You can scale the service in two ways:

* Using vertical scaling, with a single container setup

    * One container can serve multiple requests at a time, where requests are processed in parallel using an isolated browser context per request.
    * The browser keeps running after processing a request.
    * No load balancing is needed in case of a single container instance.

* Using horizontal scaling, where multiple containers run in parallel
    
    * Each container can serve multiple requests at a time.

Running multiple container replicas requires additional load balancing, which is not provided by Mendix. You need to configure and use your own preferred load balancing tools, such as [Nginx](https://nginx.org/).

### Installing the Service

In order to install the service, the following artifact is available:

* The Docker image for the PDF Document Generation service. 

#### Installing through Docker

* Pull the Docker image through `docker pull private-cloud.registry.mendix.com/mendix/document-generation-service:<tag>`.
* Run the Docker container through the `docker run -p 8085:8085 --name document-generation private-cloud.registry.mendix.com/mendix/document-generation-service:<tag>`, command, where `<tag>` should be replaced with the version of the service, such as `1.0.0`. This creates a Docker container, which is exposed on port `8085`.

## Configuration {#configuration}

The service has several [configuration options](#configuration-options) for adapting to your specific needs.

### Setting Configuration Values {#setting-configuration-values}

The approach for setting configuration values depends on the installation type. Follow the applicable instructions described in the [Available Configuration Options](#configuration-options) table.

#### Configuring through Docker

When using Docker to run the image, add the configuration using the provided environment variables. An example of this is `docker run -p 8085:8085 -e MAX_DOCUMENT_SIZE=<value> --name document-generation private-cloud.registry.mendix.com/mendix/document-generation-service:<tag>`.

### Available configuration options {#configuration-options}

| Environment variable | Default value | Description |
|----------------------|---------------|-------------|
| `MAX_DOCUMENT_SIZE` | `25000000` (25 MB) | The maximum size for PDF documents generated using the service. When a PDF exceeds this file size, the request is aborted. |
| `MAX_PAGE_RENDERING_TIME` | `30000` (30 seconds) | The maximum time to wait for the page to finish loading and rendering. If loading the page exceeds this time, a [Wait for Content](/appstore/modules/document-generation/#wait-for-content-exception) exception is sent to the module. |
| `ACCEPT_INSECURE_CERTIFICATES` | `false` | <p> Allows the use of untrusted certificates, such as when using self-signed certificates.</p> <p> **Warning:** This disables certificate validation, and allows the use of invalid certificates. Be aware of any resulting security risks.</p> |

## Configuring your Mendix Apps

When you have the PDF Document Generation container running in your environment, you need to configure your Mendix apps to use the private service, as follows:

* Make sure that you are using version 2.1.0 or higher of the PDF Document Generation module.
* Configure the `DocumentGeneration.OverrideServiceType` constant to `Private`.
* Configure the `DocumentGeneration.ServiceEndpoint` constant to point to the container address and port, such as `http://document-generation:8085`.
* To generate your first document, follow the instructions in the module documentation [PDF Document Generation](/appstore/modules/document-generation/).

{{% alert color="info" %}}You do not need to register your application when using a private service. In this case, it is therefore also not required to include the `Snip_AppRegistration` snippet in your app.{{% /alert %}}

## Logging

All application level errors are sent back to the module. Refer to [PDF Document Generation](/appstore/modules/document-generation/) for details. 

Technical logs of the service are available at the container level. If you run multiple containers, logs are spread across them. We recommend to set up a centralized monitoring solution yourself.
