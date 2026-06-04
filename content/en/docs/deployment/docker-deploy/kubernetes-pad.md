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

1. Create a file named, for example, *k8s/namespace.yaml*, with the contents like the following:

    ```yaml
    apiVersion: v1
    kind: Namespace
    metadata:
        name: mendix-app
    ```

2. Apply the file by running the following command: `kubectl apply -f k8s/namespace.yaml`.

    Replace the name and path of the file as required.

#### Creating a Kubernetes Secret

Store all sensitive values in a Kubernetes Secret by performing the following steps:

1. Create a file named, for example, *k8s/secret.yaml*, with the contents like the following:

    ```yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: mendix-secret
      namespace: mendix-app
    type: Opaque
    stringData:
      RUNTIME_PARAMS_DATABASEJDBCURL: "postgresql://mendix:mendix@postgres:5432/mendix" # Defines the JDBC URL to use for the database connection (which overrides the other database connection settings).
      RUNTIME_PARAMS_DATABASE_TYPE: "PostgreSQL"
      RUNTIME_PARAMS_DATABASE_HOST: "postgresEndpointURL" #This will be overridden if you supply DatabaseJdbcUrl.
      RUNTIME_PARAMS_DATABASE_PORT: "5432"
      RUNTIME_PARAMS_DATABASE_NAME: "<your-database-name>"
      RUNTIME_PARAMS_DATABASE_USERNAME: "<your-database-username>"
      RUNTIME_PARAMS_DATABASE_PASSWORD: "<your-database-password>"
      RUNTIME_PARAMS_ADMIN_PASSWORD: "<your-admin-password>"
      RUNTIME_PARAMS_LICENSE_LICENSE_ID: "<your-license-id>"
      RUNTIME_PARAMS_LICENSE_LICENSE_KEY: "<your-license-key>"
    ```

2. Apply the file by running the following command: `kubectl apply -f k8s/secret.yaml`.

    Replace the name and path of the file as required.

    {{% alert color="info" %}}
    For production environments, consider using a secrets manager like Azure Key Vault, AWS Secrets Manager, or HashiCorp Vault with a CSI driver instead of plain Kubernetes Secrets.
    {{% /alert %}}

#### Creating a Persistent Volume Claim for Storage

Mendix requires persistent storage for uploaded files. Create a Persistent Volume Claim (PVC) by performing the following steps:

1. Create a file named, for example, *k8s/pvc.yaml*, with the contents like the following:

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mendix-storage
      namespace: mendix-app
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 10Gi
    ```

2. Apply the file by running the following command: `kubectl apply -f k8s/pvc.yaml`.

    Replace the name and path of the file as required.

    {{% alert color="info" %}}
    For multi-replica deployments or cloud-native setups, use S3-compatible storage instead. For more information, see [Configuring Deployment: S3 Storage](#s3-storage).
    {{% /alert %}}

#### Configuring Deployment

Configure deployment settings by performing the following steps:

1. Create a file named, for example, *k8s/deployment.yaml*, with the contents like the following:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mendix-app
      namespace: mendix-app
      labels:
        app: mendix-app
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mendix-app
      template:
        metadata:
          labels:
            app: mendix-app
        spec:
          containers:
            - name: mendix-app
              image: <your-registry>/mendix-app:1.0.0
              ports:
                - name: http
                  containerPort: 8080
            - name: admin
              containerPort: 8090

              # Load sensitive config from Secret
              envFrom:
                - secretRef:
                    name: mendix-secret

              # Non-sensitive environment variables
              env:
                - name: RUNTIME_PARAMS_MENDIX_CORE_STORAGESERVICE
                  value: "localfilesystem"
                - name: RUNTIME_PARAMS_MENDIX_STORAGE_PATH
                  value: "/data"
                - name: RUNTIME_PARAMS_MENDIX_LOG_LEVEL
                  value: "INFO"

              # Mount persistent storage
              volumeMounts:
                - name: mendix-storage
                  mountPath: /data

              # Resource limits
              resources:
                requests:
                  memory: "512Mi"
                  cpu: "250m"
                limits:
                  memory: "1Gi"
                  cpu: "500m"

              # Health checks
              livenessProbe:
                httpGet:
                  path: /health/live
                  port: 8080
                initialDelaySeconds: 60
                periodSeconds: 10
                failureThreshold: 3

              readinessProbe:
                httpGet:
                  path: /health/ready
                  port: 8080
                initialDelaySeconds: 30
                periodSeconds: 10
                failureThreshold: 3

          volumes:
            - name: mendix-storage
              persistentVolumeClaim:
                claimName: mendix-storage
    ```

2. Apply the file by running the following command: `kubectl apply -f k8s/deployment.yaml`.

    Replace the name and path of the file as required.

##### S3 Storage {#s3-storage}

If you are using S3 instead of local storage, make the following changes to the above example file:

* Remove the `volumeMounts` and `volumes` sections.
* Replace the `env` section with the following:

    ```yaml
    env:
    env:
      - name: RUNTIME_PARAMS_MENDIX_CORE_STORAGESERVICE
        value: "com.mendix.storage.s3"
      - name: RUNTIME_PARAMS_MENDIX_STORAGE_S3_ENDPOINT
        value: "<your-s3-endpoint>"
      - name: RUNTIME_PARAMS_MENDIX_STORAGE_S3_BUCKETNAME
        value: "<your-s3-bucket>"
      - name: RUNTIME_PARAMS_MENDIX_STORAGE_S3_REGION
        value: "<your-s3-region>" 
        ...
      - name: RUNTIME_PARAMS_MENDIX_STORAGE_S3_ACCESS_KEYID
        valueFrom:
          secretKeyRef:
            name: mendix-secret
            key: RUNTIME_PARAMS_MENDIX_STORAGE_S3_ACCESS_KEYID
      - name: RUNTIME_PARAMS_MENDIX_STORAGE_S3_SECRETACCESSKEY
        valueFrom:
          secretKeyRef:
            name: mendix-secret
            key: RUNTIME_PARAMS_MENDIX_STORAGE_S3_SECRETACCESSKEY
    ```

#### Configuring the Service

Configure the Service settings by performing the following steps:

1. Create a file named, for example, *k8s/service.yaml*, with the contents like the following:

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: mendix-app-service
      namespace: mendix-app
    spec:
      selector:
        app: mendix-app
      ports:
        - name: http
          protocol: TCP
          port: 80
          targetPort: 8080
        - name: admin
          protocol: TCP
          port: 8090
          targetPort: 8090
      type: ClusterIP
    ```

2. Apply the file by running the following command: `kubectl apply -f k8s/service.yaml`.

    Replace the name and path of the file as required.

#### Configuring the Ingress

Configure the Ingress settings by performing the following steps:

1. Create a file named, for example, *k8s/ingress.yaml*, with the contents like the following:

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: mendix-app-ingress
      namespace: mendix-app
      annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /
    spec:
      rules:
        - host: your-app.your-domain.com
          http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: mendix-app-service
                    port:
                      number: 80
    ```

2. Optional: To use HTTPS, add a `tls` section to the `spec` of the above example, and reference a TLS secret (for example, from **cert-manager**):

    ```yaml
    spec:
      tls:
        - hosts:
            - your-app.your-domain.com
          secretName: mendix-tls-secret
    ```

3. Apply the file by running the following command: `kubectl apply -f k8s/ingress.yaml`.

    Replace the name and path of the file as required.

### Verifying the Deployment

Verify the deployment by running the following commands:

```text
# Check all resources in the namespace
kubectl get all -n mendix-app

# Watch pod startup
kubectl get pods -n mendix-app -w

# View app logs
kubectl logs -f deployment/mendix-app -n mendix-app

# Check ingress
kubectl get ingress -n mendix-app
```

## Reference

### Environment Variables

The following is a list of available environment variables that you can use.

