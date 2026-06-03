---
title: "Portable App Distribution for Kubernetes"
url: /developerportal/deploy/docker-deploy-k8s/
weight: 20
description: "Describes how to use Portable App Distribution to deploy on Kubernetes without installing the Mendix Operator."
---

## Introduction

This guide provides a walkthrough for deploying your Mendix application using [Portable App Distribution](/developerportal/deploy/portable-app-distribution-deploy/) with Kubernetes, but without relying on the Mendix Operator. This is particularly useful for air-gapped environments, private cloud deployments, or scenarios where you need full control over the deployment process.

{{% alert color="info" %}}
This document is not an official Mendix implementation, or a substitute for recommended production deployment strategies. For more features, such as app management or governance, we suggest using [Mendix on Kubernetes](/developerportal/deploy/private-cloud/) or [Mendix on Azure](/developerportal/deploy/mendix-on-azure/), which offer a structured, tested experience with cloud infrastructure. 

For information about the scope of support, see [Support for Different Deployment Strategies](/support/deployment-strategy-support/).
{{% /alert %}}

## Benefits of Portable App Distribution

Portable App Distribution revolutionizes the way in which Mendix applications are packaged and delivered. This innovative approach bundles your application code with all its necessary dependencies into a single, self-contained, and runnable artifact. This greatly simplifies the deployment of Mendix applications, whether you are targeting on-premise infrastructure or modern containerized environments like Docker, making the entire process more efficient and seamless.

The ability to generate a Portable App Distribution with a single build command means that creating a Docker-ready artifact becomes a streamlined process, making the overall integration into existing Docker-based CI/CD pipelines more efficient and less prone to errors.

The Portable App Distribution feature allows you to package and deploy Mendix apps without relying on the Mendix Cloud or a Mendix Operator. This is particularly useful for the following use cases:

* Air-gapped environments where internet access is restricted or unavailable
* Private cloud deployments where you manage your own infrastructure
* Full control scenarios where you need complete ownership of the deployment pipeline

Docker provides a consistent and reproducible environment for running Mendix apps, making it ideal for cloud-native and containerized deployments.

Portable App Distribution offers a more agile, user-centric, and efficient deployment ecosystem, empowering customers with greater control over their Docker deployments and simplifying the internal deployment processes.

## Prerequisites

Before you begin, ensure you have the following:

* Mendix Studio Pro version 10.24.19, 11.6.5, 11.19, or above
* A Mendix app that you want to deploy
* Docker installed on your system (for building and running Docker images)
* Access to a container registry (for pushing and pulling Docker images)
* Any Kubernetes cluster (for example, AKS, EKS, GKE, or on-premises)
* `Kubectl` configured and connected to your cluster

## Deploying an App with Portable App Distribution

The Portable App Distribution feature in Mendix Studio Pro provides you with the necessary application files to build a Docker image. It packages your Mendix application as a self-contained distribution, ready for integration into your Docker environment.

To deploy your app to Docker, you must create a Portable App Distribution Package, build a Docker image, and then deploy the Docker image (including pushing it to a container registry). For more information, refer to the sections below.

### Creating a Portable App Distribution Package

To create a Portable Package from your Mendix app, perform the following steps:

1. Open your app in Studio Pro.
2. Go to **App** > **Create Deployment Package**.
3. In the **Create Deployment Package** dialog, select **Portable package**.
4. Click **OK**.

The Portable Package is saved to the **releases** folder of your app.

For more information about Portable Packages, see [Portable App Distribution](/developerportal/deploy/portable-app-distribution-deploy/). Files included in the Portable Package are the core of your Mendix application and are ready to be included in a Docker image.
   
### Building a Docker Image

To build a Docker image from the Portable Package, perform the following steps:

1. Extract the Portable Package to a directory of your choice

```text
# Create a working directory
mkdir mendix-docker && cd mendix-docker

# Extract the portable package (it's a zip file)
unzip /path/to/releases/YourApp.zip -d .
```

2. Create a Dockerfile in the extracted directory with contents like the following.

    ```text
    # This file provides an example on how to start the runtime in Docker.
    # It is based on the configuration named Default.

    FROM eclipse-temurin:21-jdk
    
    # Set working directory
    WORKDIR /mendix
    
    # Copy Mendix app files into the image
    COPY ./app ./app
    COPY ./bin ./bin
    COPY ./etc ./etc
    COPY ./lib ./lib
    
    # Environment variables (optional)
    ENV MX_LOG_LEVEL=info
    ENV M2EE_ADMIN_PASS=${M2EE_ADMIN_PASS}
    
    # Expose ports
    EXPOSE 8090
    EXPOSE 8080
    
    # Start command
    CMD ["./bin/start", "etc/Default"]
    ```

    You must create this Dockerfile yourself and place it alongside the application files generated by the Portable App Distribution. The `COPY` commands in the example above assume that the `app`, `bin`, `etc`, and `lib` directories are in the same location as your Dockerfile.

3. Build the Docker image by using the following command: `docker build -t <your-image-name>:<tag> .`, where `<your-image-name>` and `<tag>` indicate your required image name and version tag (for example, `my-mendix-app:1.0.0`).

### Pushing the Docker Image

To push the Docker image to a container registry, perform the following steps:

1. Log in to your container registry by running the following command: `docker login <your-registry>`.
2. Tag the Docker image with the registry URL by running the following command: `docker tag <your-image-name>:<tag> <your-registry>/<your-image-name>:<tag>`.
3. Push the Docker image to the registry by running the following command: `docker push <your-registry>/<your-image-name>:<tag>`.

### Deploying the Docker Image

Once the Docker image is available in your container registry, you can deploy it to Kubernetes by applying the following .yaml files. The .yaml files must be organized in a folder, for example, *k8s/*. You must apply them in the same order as below.

#### Creating a Namespace

Create a namespace by performing the following steps:

1. Create a file named, for example, *k8s/namespace.yaml*, with the following contents:

    ```yaml
    apiVersion: v1
    kind: Namespace
    metadata:
        name: mendix-app
    ```

2. Apply the file by running the following command: `kubectl apply -f k8s/namespace.yaml`.

    Replace the name and path of the file as required.



## Environment Variables {#env-variables}

You can configure the Mendix Runtime by using environment variables. The following environment variables are supported:

| Environment Variable | Description |
| --- | --- |
| `MENDIX_DATABASE_URL` | The URL of the database|
| `MENDIX_DATABASE_TYPE` | The type of the database (for example, PostgreSQL, MySQL)|
| `MENDIX_DATABASE_HOST` | The host name of the database server |
| `MENDIX_DATABASE_PORT` | The port of the database server |
| `MENDIX_DATABASE_NAME` |The name of the database |

For more information, see [Runtime Customization](/refguide/custom-settings/#introduction).

## Configuration File {#config-file}

Alternatively, you can configure the Mendix Runtime by using a configuration file. The configuration file is a JSON file that contains the same settings as the environment variables.

### Example Configuration File

```json
{
  "DatabaseType": "PostgreSQL",
  "DatabaseHost": "localhost:5432",
  "DatabaseName": "mendix",
  "DatabaseUserName": "mendix",
  "DatabasePassword": "mendix",
  "AdminPassword": "Admin1234!",
  "RuntimePort": 8080,
  "RuntimeAdminPort": 8090
}
```

### Using the Configuration File

To use the configuration file, set the `MENDIX_CONFIG_FILE` environment variable to the path of the configuration file:

`export MENDIX_CONFIG_FILE=/path/to/config.json`
 
## Logging

The Mendix Runtime logs to a standard output by default. You can configure the log level using the `MX_LOG_LEVEL` environment variable.

The following log levels are supported (in order of verbosity):

| Log Level | Description |
| --------- | ----------- |
| `TRACE` | Most verbose — logs all internal operations |
| `DEBUG` | Detailed diagnostic information |
| `INFO` | General operational messages (default) |
| `WARNING` | Potentially harmful situations |
| `ERROR` | Error events that may still allow the app to continue |
| `CRITICAL` | Severe errors that may cause the app to stop |

## Health Checks

The Mendix Runtime exposes health check endpoints that can be used to monitor the status of your app:

| EndPoint | Description |
| -------- | ----------- |
| `/health` | Returns the overall health status of the app |
| `/health/live` | Returns the liveness status — indicates if the app is running |
| `/health/ready` | Returns the readiness status — indicates if the app is ready to serve traffic |

These endpoints are especially useful when integrating with orchestration platforms such as Kubernetes, which rely on liveness and readiness probes to manage container lifecycle.
