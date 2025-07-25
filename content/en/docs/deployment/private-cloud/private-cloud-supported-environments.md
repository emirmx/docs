---
title: "Supported Providers"
url: /developerportal/deploy/private-cloud-supported-environments/
description: "Describes which providers are supported by Mendix on Kubernetes"
weight: 100
---

## Introduction

Mendix on Kubernetes depends on external services to deploy and run Mendix apps.
This document covers which providers and services are officially supported by the Mendix Operator.

## Kubernetes Cluster Types

### Supported Cluster Types{#supported-clusters}

We currently support deploying to the following Kubernetes cluster types:

* [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (EKS)
{{% alert color="info" %}}
If you want to deploy your app to Amazon EKS, consider using the Mendix for Amazon EKS Reference Deployment. For more information, see [Mendix for Amazon EKS—Terraform module](https://aws.amazon.com/solutions/partners/terraform-modules/mendix-eks/).
{{% /alert %}}
* [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/)
* [Red Hat OpenShift Container Platform](https://www.openshift.com/)
* [MicroK8s](https://microk8s.io/)
* [k3s](https://k3s.io/)
* [minikube](https://minikube.sigs.k8s.io/docs/)
* [Google Cloud Platform](https://cloud.google.com/)
* [Google Kubernetes Engine- Autopilot](https://cloud.google.com/kubernetes-engine/docs/concepts/autopilot-overview). For more information, see [Mendix on Kubernetes Cluster: GKE Autopilot Workarounds](/developerportal/deploy/private-cloud-cluster/#gke-autopilot-workarounds)

{{% alert color="warning" %}}
If deploying to Red Hat OpenShift, you need to specify that specifically when creating your deployment. All other cluster types use generic Kubernetes operations.
{{% /alert %}}

#### Supported Versions{#supported-versions}

Mendix on Kubernetes Operator `v2.*.*` is the latest version which officially supports:

* Kubernetes versions 1.19 through 1.33
* OpenShift 4.6 through 4.18

{{% alert color="warning" %}}
Kubernetes 1.22 is a [new release](https://kubernetes.io/blog/2021/08/04/kubernetes-1-22-release-announcement/) which removes support for several deprecated APIs and features.

This version of Kubernetes is not yet offered or fully supported by most distributions and providers.

Mendix on Kubernetes Operator v2.*.*. extends support for Kubernetes versions starting from 1.20 onwards and is confirmed to work seamlessly with Kubernetes version 1.22.

Existing clusters running Mendix on Kubernetes Operator v1.\*.\* will need to be upgraded to Kubernetes 1.21 and Mendix on Kubernetes Operator v2.\*.\* **before** upgrading to Kubernetes 1.22.

While EOLed components are expected to remain compatible, it is important to note that we do not actively test them. This is because vendors may remove End-of-Life (EOL) versions due to security vulnerabilities (CVEs).

{{% /alert %}}

Mendix on Kubernetes Operator `v1.12.*` is an LTS release which officially supports older Kubernetes versions:

* Kubernetes versions 1.13 through 1.21
* OpenShift 3.11 through 4.7

### Cluster Requirements

To install the Mendix Operator, the cluster administrator will need permissions to do the following:

* Create Custom Resource Definitions
* Create roles in the target namespace or project
* Create role bindings in the target namespace or project

The cluster should have at least 2 CPU cores, 2 GB memory and 3 GB ephemeral-storage available on a Kubernetes node. This is enough to run one simple app, but does not include additional resources required by Kubernetes core components.

In OpenShift, the cluster administrator must have a `system:admin` role.

#### CPU requirements

Mendix Operator runs on CPUs with the [x86-64](https://en.wikipedia.org/wiki/X86-64) architecture.

{{% alert color="info" %}}

Starting with Mendix Operator v2.5.0, container images used in *Connected Mode* also support [ARM64/AArch64](https://en.wikipedia.org/wiki/AArch64). *ARM64* support is experimental at this moment and should only be used for non-production environments.

Only core *Connected mode* features support *ARM64*. The following features **do not** support *ARM64* CPUs at the moment:

* [Migrating to Your Own Registry](/developerportal/deploy/private-cloud-migrating/)

{{% /alert %}}

{{% alert color="warning" %}}
If the cluster is running nodes with multiple architectures (for example, *x86-64* and *ARM64*), the namespace where Mendix on Kubernetes is installed should use a fixed (specified) architecture. One way to do this is by configuring a [PodNodeSelector](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#podnodeselector) for that namespace, and only using nodes with a specific architecture (for example, `amd64`).

The image builder doesn't build multiple architecture images at the moment.
{{% /alert %}}

### Unsupported Cluster Types

It is not possible to use Mendix on Kubernetes in [OpenShift Online](https://www.openshift.com/products/online/) (all editions, including Starter and Pro) or [OpenShift Developer Sandbox](https://developers.redhat.com/developer-sandbox) because they don't allow the installation of Custom Resource Definitions.

Kubernetes included with [Docker Desktop](https://docs.docker.com/desktop/kubernetes/) is not officially supported.

## Container Registries{#container-registries}

Mendix on Kubernetes builds container images for every app and pushes them to the registry. It needs credentials to access the registry and permissions to push images into the registry.

Images are pulled from the registry by Kubernetes, not by Mendix on Kubernetes.
The configuration script for Mendix on Kubernetes can configure Kubernetes image pull secrets and use the same credentials it uses for pushing images (for all registries except EKS).
For large-scale or enterprise deployments, it may be better to configure image pulls on a cluster-wide level, or to configure separate, read-only image pull credentials.

### Local Registry

A local, self-hosted, registry is supported for non-production use with the following bring-your-own infrastructure clusters:

* MicroK8s
* k3s
* minikube

To use a local registry, it must be available from Kubernetes pods (for pushing images) and from the cluster itself (for pulling images). In most cases, the push URL and pull URL will be different.

It is possible to have username/password authentication or to push without authentication.

### Externally Hosted Registry

Externally hosted [OCI compliant](https://github.com/opencontainers/distribution-spec/blob/main/spec.md) registries are supported if they allow username/password authentication. This includes:

* [Docker Hub](https://hub.docker.com/)
* [quay.io](https://quay.io/)
* [JFROG Artifactory](https://jfrog.com/artifactory/)
* [Sonatype Nexus](https://www.sonatype.com/products/nexus-repository)
* [Harbor](https://goharbor.io)

When using ACR in combination with Azure Kubernetes Service, it is possible to set up [native authentication](https://docs.microsoft.com/en-us/azure/aks/cluster-container-registry-integration#create-a-new-aks-cluster-with-acr-integration) for pulling images from ACR.

### OpenShift Image Registry

The local image registry can be used in an OpenShift cluster. It is not possible to use an OpenShift registry in a non-OpenShift cluster.

Image pull authentication will be configured out of the box.

OpenShift 4 registries don't need any configuration and will be configured automatically.

For an OpenShift 3 registry, the pull URL should be set to `docker-registry.default.svc:5000`.
The push URL should be set to `<registry ip>:5000` where `<registry ip>` can be obtained by running `oc get svc docker-registry -n default`.

The OpenShift registry must be installed and enabled for use.

### Amazon Elastic Container Registry (ECR)

[Amazon ECR](https://aws.amazon.com/ecr/) can only be used together with EKS clusters.

To use an ECR registry, the Mendix Operator will need an AWS Identity and Access Management (IAM) account with permissions to push and pull images.

The EKS cluster should be configured so that it can [pull images from ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ECR_on_EKS.html).

### Google Artifact Registry

[Google Cloud Platform](https://cloud.google.com/) provides the [artifact registry](https://cloud.google.com/artifact-registry).

Mendix Operator supports registry authentication with [workload identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity). The Mendix Operator will need a Kubernetes service account [bound](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#authenticating_to) to a [google service account](https://cloud.google.com/iam/docs/service-accounts) with permissions to authenticate to a registry.

### Azure Container Registry

[Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) can be used with any cluster (if static credential authentication is used).

When used together with an [Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service), Mendix Operator can use [managed identity authentication](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-authentication-managed-identity) assigned to the Mendix Operator's Kubernetes service account.

## Databases{#databases}

The following databases are supported, and provide the features listed.

| Database | Data Persists | Provisioned by Operator |
| --- | --- | --- |
| Ephemeral | No | Yes |
| Standard PostgreSQL | Yes | Yes |
| Microsoft SQL Server | Yes | Yes |
| Dedicated JDBC | Yes | No |

### Ephemeral Database

The ephemeral database plan uses an in-memory database running directly in a Mendix Runtime container.
It doesn't require any external database or provider and is great for quick tests or apps that don't require any file storage.

{{% alert color="info" %}}
An app using an ephemeral database will lose all data if its environment is stopped or restarted.

An app with an ephemeral database cannot have more than one replica. Only the first (leader) replica will be able to start.
{{% /alert %}}

### Standard PostgreSQL Database

This refers to a PostgreSQL database which is automatically provisioned by the Operator. If you are connecting to an existing database, you should use the [Dedicated JDBC database](#jdbc) option described below.

The following standard PostgreSQL databases are supported:

* PostgreSQL 13
* PostgreSQL 14
* PostgreSQL 15
* PostgreSQL 16
* PostgreSQL 17

{{% alert color="info" %}}
While Mendix on Kubernetes supports all Postgres versions listed above, the Mendix Runtime might require a more specific Postgres version.

For best compatibility, use the newest available version of Postgres.
{{% /alert %}}

A standard PostgreSQL database is an unmodified PostgreSQL database installed from a Helm chart or from an installation package.

The following managed PostgreSQL databases are supported:

* [Amazon RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/)
* [Azure Database for PostgreSQL](https://azure.microsoft.com/en-us/services/postgresql/).
* [Google Cloud SQL for PostgreSQL](https://cloud.google.com/sql/docs/postgres).
* [Amazon RDS Aurora for PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.AuroraPostgreSQL.html)

Amazon PostgreSQL instances require additional firewall configuration to allow connections from the Kubernetes cluster.

Amazon Aurora PostgreSQL instances require additional firewall configuration to allow connections from the Kubernetes cluster.

Azure PostgreSQL databases require additional firewall configuration to allow connections from the Kubernetes cluster.

Some managed PostgreSQL databases might have restrictions or require additional configuration.

As an alternative to static password authentication, Mendix Operator can use its Kubernetes Service Account to authenticate with:

* AWS RDS databases using IAM roles
* Azure Database for PostgreSQL (Flexible Server) databases using managed identities

{{% alert color="info" %}}
To use a PostgreSQL database, the Mendix Operator requires a Superuser account with root privileges and permissions to create new users and databases.

For every Mendix app environment, a new database schema and user (role) will be created so that the app can only access its own data.
{{% /alert %}}

{{% alert color="info" %}}
By default, the Mendix Operator will first connect to the database server with TLS enabled; if the database server doesn't support TLS, the Mendix Operator will reconnect without TLS.
To ensure compatibility with all PostgreSQL databases (including ones with self-signed certificates), all TLS CAs are trusted by default.

If Strict TLS is enabled, Mendix on Kubernetes will connect to the PostgreSQL server with TLS and validate the PostgreSQL server's TLS certificate. In this case, the connection will fail if:

* the PostgreSQL server has an invalid certificate
* or its certificate is signed by an unknown certificate authority
* or the PostgreSQL server doesn't support TLS connections.

The Mendix Operator allows you to specify custom Certificate Authorities to trust. This allows you to enable Strict TLS even for databases with self-signed certificates.

Strict TLS mode should only be used with apps created in Mendix 8.15.2 (or later versions), earlier Mendix versions will fail to start when validating the TLS certificate.
{{% /alert %}}

### Microsoft SQL Server

This refers to a SQL Server database which is automatically provisioned by the Operator. If you are connecting to an existing database, you should use the [Dedicated JDBC database](#jdbc) option described below.

The following Microsoft SQL Server editions are supported:

* SQL Server 2019
* SQL Server 2022

The following managed Microsoft SQL Server databases are supported:

* [Amazon RDS for SQL Server](https://aws.amazon.com/rds/sqlserver/)
* [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)

Amazon and Azure SQL servers require additional firewall configuration to allow connections from the Kubernetes cluster.

Some managed SQL Server databases might have restrictions or require additional configuration.

As an alternative to static password authentication, Mendix Operator can use its Kubernetes Service Account to authenticate with Azure SQL databases. The Kubernetes Service Account is linked with a Managed Identity, and the Managed Identity replaces a static username/password. This feature requires Mendix Operator version 2.17 (or above) and Mendix 10.10 (or above).

{{% alert color="info" %}}
To use a SQL Server database, the Mendix Operator requires Superuser account with permissions to create new users and databases.

For every Mendix app environment, a new database, user, and login will be created so that the app can only access its own data.
{{% /alert %}}

{{% alert color="info" %}}
By default, Mendix on Kubernetes will not enforce encryption. Encryption can be enforced in SQL Server if required.

If Strict TLS is enabled, the Mendix Operator will connect to SQL server with TLS and validate the SQL Server's TLS certificate. In this case, the connection will fail if

* SQL Server doesn't support encryption
* the SQL Server server has an invalid certificate
* or its certificate is signed by an unknown certificate authority

The Mendix Operator allows you to specify custom Certificate Authorities to trust. This allows you to enable Strict TLS even for databases with self-signed certificates.

Strict TLS mode should only be used with apps created in Mendix 8.15.2 (or later versions), earlier Mendix versions will fail to start when validating the TLS certificate.
{{% /alert %}}

### Dedicated JDBC database{#jdbc}

This allows you to use an existing database (schema) [database configuration parameters](/refguide/custom-settings/) directly as supported by the Mendix Runtime.

{{% alert color="info" %}}
A dedicated JDBC database cannot be used by more than one Mendix app.
{{% /alert %}}

## File storage{#file-storage}

### Ephemeral File Storage

The ephemeral file storage plan will store files directly in the Mendix Runtime container.
It doesn't require any external file storage provider and is great for quick tests or stateless apps that don't require any file storage.

{{% alert color="info" %}}
An app using an ephemeral file storage will lose all files if its environment is stopped or restarted.
{{% /alert %}}

### MinIO

The latest version of [MinIO](https://min.io/) is supported if it is running in server mode.

{{% alert color="info" %}}
An admin account is required with permissions to create and delete users, policies and buckets.

For every Mendix app environment, a new bucket and user will be created so that the app can only access its own data.
{{% /alert %}}

{{% alert color="warning" %}}
If MinIO is installed in the deprecated Gateway mode, it needs to be configured to use etcd.
MinIO uses etcd to store its configuration.
Without etcd, MinIO will disable its admin API – which is required by the Mendix Operator to create new users for each environment.
{{% /alert %}}

### Amazon S3

[Amazon S3](https://aws.amazon.com/s3/) is supported. Mendix on Kubernetes supports multiple ways of managing and accessing S3 buckets: from creating a new S3 bucket and IAM account per environment to sharing an account and bucket by all environments in a namespace.

A complete list of supported S3 modes and their required IAM permissions for each one is available in [storage plan](/developerportal/deploy/standard-operator/#storage-plan)
configuration details.

### Azure Blob Storage

[Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/) is supported.

Mendix Operator can perform the following tasks:

* Provide a static access key and other credentials to environments (a static config).
* Handle the lifecycle of a storage container by creating a dedicated container and Azure Managed Identity for every new environment, and ensuring that an environment can only access its dedicated container (through the environment's Managed Identity); this feature works with Mendix 10.10 and above.

A complete list of supported Azure Blob Storage modes and their required role assignments (permissions) for each one is available in [storage plan](/developerportal/deploy/standard-operator/#storage-plan) configuration details.

### Google Cloud Storage

[Google Cloud Storage](https://cloud.google.com/storage) is supported with [Cloud Storage Interoperability](https://cloud.google.com/storage/docs/interoperability) mode.

Mendix Operator will need the endpoint, access key, and secret key to access the storage that can be configured in the interoperability setting.

### Ceph

[Ceph](https://ceph.io/en/) is supported with the S3-compatible interface [Ceph Object Gateway](https://docs.ceph.com/en/mimic/radosgw/). The Mendix Operator will need the endpoint, access key, and secret key to access the storage. Please check the Ceph documentation for information on how to get the credentials.

## Networking

{{% alert color="info" %}}
DNS, load balancing and the ingress controller should be configured first for the whole Kubernetes cluster.
Mendix on Kubernetes will use the existing ingress controller.
{{% /alert %}}

{{% alert color="warning" %}}
We strongly recommend using the [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/), even if other Ingress controllers or OpenShift Routes are available. You may need to check which of the [several versions of the NGINX Ingress Controller](https://www.nginx.com/blog/guide-to-choosing-ingress-controller-part-4-nginx-ingress-controller-options/#NGINX-vs.-Kubernetes-Community-Ingress-Controller) is installed in your cluster. Mendix recommends the "community version".

NGINX Ingress can be used to deny access to sensitive URLs, add HTTP headers, enable compression, and cache static content.
NGINX Ingress is fully compatible with [cert-manager](https://cert-manager.io/), removing the need to manually manage TLS certificates. In addition, NGINX Ingress can use a [Linkerd](https://linkerd.io/) Service Mesh to encrypt network traffic between the Ingress Controller and the Pod running a Mendix app.

These features will likely be required once your application is ready for production.
{{% /alert %}}

### OpenShift Route

OpenShift routes are supported only in OpenShift.

The only configuration option currently supported is turning TLS on or off.
When TLS is turned on, `Edge` termination (where TLS termination occurs at the router, before the traffic gets routed to the pods) will be used, with automatic redirection from HTTP to HTTPS.

The following configuration options are available in OpenShift:

* Turn TLS on and off
* Add route annotations
* Provide the name of an existing TLS certificate secret to use instead of the default router certificate
* Provide a custom domain name (for example, mendix.example.com) to use instead of the default OpenShift route domain

It is also possible to provide a custom TLS configuration for individual environments, overriding the default configuration (only available in **Standalone** Mendix Operator installations):

* Turn TLS on and off
* Specify the name of an existing TLS certificate secret to use
* Provide the TLS Certificate and Private Key values directly in the environment specification

### Ingress

Mendix on Kubernetes is compatible with the following ingress controllers:

* [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/)
* [Traefik](https://traefik.io/traefik/)
* [AWS Application Load Balancer](https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html)
* [Ingress for External Application Load Balancer](https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-xlb)
* [Azure Application Gateway Ingress Controller](https://learn.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview)

For ingress, it is possible to do the following:

* Turn TLS on and off
* Add ingress annotations
* Add service annotations
* Specify the ingress class, path and path type
* Provide the name of an existing TLS secret to use
* Provide a domain name (for example, mendix.example.com)

For each environment, the URL will be automatically generated based on the domain name.
For example, if the domain name is set to mendix.example.com, then apps will have URLs such as myapp1-dev.mendix.example.com, myapp1-prod.mendix.example.com and so on.

The DNS server should be configured to route all subdomains (the `*` subdomain, for example, `*.mendix.example.com`) to the ingress/load balancer.

It is also possible to provide a custom TLS configuration for individual environments, overriding the default configuration (only available in **Standalone** Mendix Operator installations):

* Turn TLS on and off
* Specify the name of an existing TLS certificate secret to use
* Provide the TLS Certificate and Private Key values directly in the environment specification

There are multiple ways of managing TLS certificates:

* The Ingress controller can have a default certificate with a wildcard domain (for example, `*.mendix.example.com`). For Ingress controllers which support for [Let's Encrypt](https://letsencrypt.org/), the Ingress controller can also request and manage TLS certificates automatically.
* Providing a TLS certificate secret for each environment.
* Using [cert-manager](https://cert-manager.io/) or a similar solution by using Ingress annotations. This service can be used to automatically request TLS certificates and create secrets for the Ingress controller.

Starting from Mendix Operator v1.11.0, Mendix app environments can use a [Linkerd](https://linkerd.io/) Service Mesh. Linkerd can be used to monitor and re-encrypt HTTP (or HTTPs) traffic between the Ingress Controller and the Pod running a Mendix app.

### Service Only

Mendix on Kubernetes can create Services without an Ingress.
In this way, the Ingress objects can be managed separately from Mendix on Kubernetes.

Mendix on Kubernetes can create Services that are compatible with:

* [AWS Network Load Balancer](https://docs.aws.amazon.com/eks/latest/userguide/network-load-balancing.html)
* AWS Classic Load Balancer

### Service Mesh Support

Starting with Mendix Operator v2.5.0, the following service meshes can be enabled for the entire Mendix on Kubernetes namespace:

* [Istio](https://istio.io/)
* [Linkerd](https://linkerd.io)

If service mesh sidecar injection is enabled, all communication between pods in the Mendix on Kubernetes namespace will happen through the service mesh.

Mendix Operator v1.11.0 added support for service mesh sidecar injection, but only for app environment pods.
