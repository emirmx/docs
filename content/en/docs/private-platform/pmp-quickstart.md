---
title: "Private Mendix Platform Quick Start Guide"
url: /private-mendix-platform/quickstart/
description: "Documents the installation and upgrade process for the Private Mendix Platform."
weight: 20
aliases:
    - /private-mendix-platform-quickstart/
---

## Introduction

This document provides a comprehensive guide for installing Private Mendix Platform, along with its optional components, in your own Kubernetes environment.

The installer is integrated with the AWS Secrets Manager. If required, you can store some configuration in the the AWS Secrets Manager without setting up a storage plan, database plan, PCLM admin and Mendix admin info in the Private Mendix Platform installer.
 
{{% alert color="info" %}}
Using a secret storage incorrectly may reduce the security of your app. Consult your secrets store provider to ensure that it is set up securely for your production environment.  
{{% /alert %}}

### Prerequisites {#prerequisites}

Private Mendix Platform depends on Mendix on Kubernetes for the installation and deployment of Mendix apps.

Before starting the installation process, make sure that you have all the necessary prerequisites:

* A Kubernetes instance where the target namespace has already been created. For more information, see [Supported Providers: Supported Versions](/developerportal/deploy/private-cloud-supported-environments/#supported-versions).
* A PostgreSQL 12 database.
* File storage. For more information, see [Supported Providers: File Storage](/developerportal/deploy/private-cloud-supported-environments/#file-storage).
* A registry. For more information, see [Supported Providers: Container Registries](/developerportal/deploy/private-cloud-supported-environments/#container-registries).
* A domain.
* For the PCLM component:

    * Mendix Operator in version 2.21.0 or above
    * A dedicated Postgres or SQLServer database server with public accessibility set to **Yes**.

* Optionally, if your Private Mendix Platform app requires its own certificate: a TLS certificate with HTTPS support.
* An environment to run installer tools with the following requirements:

    * A kubeconfig file with administrator privileges for your Kubernetes or OpenShift platform
    * A command line terminal that supports the console API and mouse interactions. In Windows, this can be PowerShell or the Windows Command Prompt.
    * For OpenShift clusters, OpenShift CLI. For more information, see [Getting started with the CLI](https://docs.openshift.com/container-platform/4.1/cli_reference/getting-started-cli.html).
    * Kubectl installed if you are deploying to another Kubernetes platform. For more information, see [Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/).

* Optionally, if you plan to install the Svix component:

    * An existing PostgreSQL database instance.
    * An optional Redis server version 6.2.0 or higher, for the task queue and cache. Using Redis is recommended for high availability, where you expect a high volume of webhook calls, or if you have multiple Svix servers. As a best practice, enable persistence in Redis so that tasks are persisted across Redis server restarts and upgrades.

* If you plan to use the AWS Secret Manager, install an AWS provider at your cluster, as described in [Kubernetes Secrets Store CSI Driver](https://secrets-store-csi-driver.sigs.k8s.io/).

## Installing and Configuring the Mendix Operator {#install-operator}

To install and configure the Mendix Operator, perform the following steps:

1. Download the release binary from your [Private Mendix Platform download portal](https://privateplatform.mendix.com/). If you do not have access to the download portal, contact your Mendix partner for information.

2. Unzip the release binary to a local folder on your Windows or Linux server. The release binary contains the following files:

    * **Tools** - *mx-pclm-cli*, which can be used to manage PCLM
    * **helm**, and **helmfile** tools, which are used to deploy and manage Private Mendix Platform charts and Svix charts
    * **images** - Private Mendix Platform image, PCLM image, Svix image, test application image
    * **Installer** - installer tools
    * **mxpc-cli** - installation tools which can be used to manage or configure the Mendix Operator
    * **charts**  - charts, including Private Mendix Platform charts and Svix charts
    
    {{< figure src="/attachments/private-platform/pmp-binary.png" class="no-border" >}}

3. Optional: If your clusters can connect to a public registry with a passable network, skip to step 4 below, otherwise initialize the installation by performing the following steps:

    1. Upload the images to your private repository in an air-gapped environment.

        ```text
        ~/mpp-binary-linux$ ./installer init  migrate --help
        Migrate Mendix Private Platform related image to your own registry

        Usage:
        installer init migrate [flags]
        Flags:
            -h, --help                 help for migrate
            -r, --registryurl string   registry url (required)
            -e, --repo string          Repository name
            -u, --username string      Username (required) for your private registry
        ```

        The destination image is named `${registryurl }/${repo}/mendix-private-platform: ${tag}`.
    
    2. The `registryurl` and `repo` are read from the input parameters. The `tag` is automatically read by the installer. If the repository does not exist, you must create it before running the `init migrate` command.

        ```text
        ~/mpp-binary-linux$ ./installer init migrate   -r [registry] -u  user -e [repositoryName]
        Please enter user password: ******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************

        Confirm password: ******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
        the config checksum is empty
        The image destination[REDACTED] svix-server:v0.75.0
        The image destiation [REDACTED] mendix-private-platform:1.4.0.80d447b1
        the config checksum is empty
        The image destiation [REDACTED] mxpc-test:1.0
        the config checksum is empty
        The image destiation [REDACTED] privatecloud-license-manager:0.3.0
        svix-server_v0.75.0 => [REDACTED] svix-server:v0.75.0 - ok
        mendix-private-platform_1.4.0.80d447b1 => [REDACTED] mendix-private-platform:1.4.0.80d447b1 - ok
        mxpc-test_1.0 => [REDACTED] mxpc-test:1.0 - ok
        privatecloud-license-manager_0.3.0 => [REDACTED] privatecloud-license-manager:0.3.0 - ok
        ```

    3. By default, mxpc-cli tools install the latest version of Mendix Operator. You can specify a different Mendix Operator version by using the following command: `./installer operator init -v="version number"`

4. Perform the base installation by doing the following steps:

    1. Run one of the following commands, where `-n` indicates the namespace: 
    
        * `./mxpc-cli installer -n=<namespace name>` - To install the Operator in [Standard](/developerportal/deploy/standard-operator/) mode
        * `./mxpc-cli installer --global -n=<namespace name>` - To install the Operator in [Global](/developerportal/deploy/global-operator/) mode; you must use a Global namespace for this installation type.

            In order to install and configure a cluster with a Global installation of the Operator and the Agent, you must use Operator version 2.21.2 or above. 
    
    2. Click **Base Installation**, and then select the cluster type.

        {{< figure src="/attachments/private-platform/pmp-install1.png" class="no-border" >}}

    3. Click **Run Installer** to install the Mendix Operator in your cluster.

5. Configure the namespace by doing the following steps:

    1. Click **Configure Namespace**.
    2. Optional: If you want to run the Operator in Global mode, click **Global Operator**.

        You must use a different namespace here than the Global namespace that you selected in step 4 above. Ensure that you do not use a namespace that is intended to be a managed namespace, that is, a namespace where you plan to deploy a Mendix app. The Global Operator namespace must be separate from managed namespaces, otherwise you may encounter unexpected results.

    3. Optional: If you are not using the AWS Secret Manager, click **Database Plan** and fill out the required information.
        
        {{< figure src="/attachments/private-platform/pmp-install2.png" class="no-border" >}}

    4. Optional: If you are not using the AWS Secret Manager, click **Storage Plan** and fill out the required information.
    5. Click **Ingress** and fill out the required information.
        
        {{< figure src="/attachments/private-platform/pmp-install3.png" class="no-border" >}}
    
    6. Click **Registry** and fill out the required information.
    7. Click **Review and Apply** > **Evaluate Configuration**.
    8. Make any required changes or click **Apply Configuration**.
        
        {{< figure src="/attachments/private-platform/pmp-install4.png" class="no-border" >}}
    
    9. Click **Exit Installer** > **OK**.
    
        {{< figure src="/attachments/private-platform/pmp-install5.png" class="no-border" >}}

## Optional: Configuring the AWS Secret Manager

To use the secret provider option for your database plan or storage plan, configure the following keys in your AWS Secret Manager:

### Database Plan Keys

| Data Type | Key | Example Value |
| --- | --- | --- |
| Database type (for example, PostgreSQL) | **database-type** | `PostgreSQL` |
| Database Jdbc Url    | **database-jdbc-url**    | `jdbc:postgresql://pg.example.com:5432/my-app-1?sslmode=prefer` |
| Database host | **database-host**    | `pg.example.com:5432` |
| Database name    | **database-name** | `my-app-1` |
| Database user name | **database-username** | `my-app-user-1` |
| Database password | **database-password**    |  |

### Storage Plan Keys

| Data Type | Key | Example Value |
| --- | --- | --- |
| Storage service name | **storage-service-name** | `com.mendix.storage.s3` |
| S3 Storage endpoint | **storage-endpoint** | `https://my-app-bucket.s3.eu-west-1.amazonaws.com` |
| S3 Storage access key id | **storage-access-key-id** | `AKIA################` |
| S3 Storage secret access key | **storage-secret-access-key** | `A###################################` |
| S3 subdirectory (or bucket name for S3-like storage systems) | **storage-bucket-name** | `subdirectory` |

{{% alert color="info" %}}
Currently, only AWS S3 or S3-compatible providers are supported.  
{{% /alert %}}

### Administrator Passwords

| Data Type | Key |
| --- | --- |
| PCLM admin password | **pclm-admin-password** |
| Private Mendix Platform admin password | **mx-admin-password** |

### Installing Private Cloud License Manager {#install-pclm}

Private Cloud License Manager is a required component of Private Mendix Platform. Before you install the Platform, install PCLM by doing the following steps:

1. Run the command `./installer component -n=<namespace name>`, where `-n` indicates a namespace. The namespace must be the same as the namespace that you plan to use for Private Mendix Platform.
2. Select **PCLM** to install PCLM.
3. Specify the following parameters:

    * **Databasetype** – The database type, either **postgres** (default) or **sqlserver**.
    * **DB Authentication mode** - The authentication mode for the database:
        * **static** (default)
        * **aws-irsa**
        * **azure-wi**
    * **Host** – The host name of the database service.
    * **Port** – The port used to access the database. The default value is *5432*.
    * **Database Name** – The name of the database where you want to hold the PCLM data.
    * **Database User** – A database user with the rights described in the prerequisites section.
    * **Database Password** – The password for the database user. This setting is available only when **DB Authentication mode** is set to **static**. 
    * **AWS-iam-Role**  – The AWS IAM role. This setting is available only when **DB Authentication mode** is set to **aws-irsa**.
    * **Azure-client-id** –  The Azure client ID. This setting is available only when **DB Authentication mode** is set to **azure-wi**.
    * **ImageRepo** – The location of the image repo, for example, `private-cloud.registry.mendix.com/privatecloud-license-manager`.
    * **Imagetag** – The docker image tag, for example, `0.3.0`.
    * **DB SSL cert file** – If your database uses strict TLS, provide the location of the SSL Root certificate file. If not, leave this field blank.
    * **Admin Password** – A new PCLM admin password. When the PCLM server is set up, it contains an *administrator* user with a default password. This password should be modified immediately.
    * **PCLM Operator User** – A new PCLM operator user.
    * **PCLM Operator Password** – A new PCLM operator password.
    * **Global Operator Namespace** - If you are using Mendix Operator in Global mode, enter the Global namespace information. If not, leave this field blank. 
    * **Customized cluster domain** - The default is `cluster.local`. Change the value if you are using a different internal cluster domain.

4. Click **Install PCLM**.

## Optional: Installing the Svix Component {#install-svix}

Svix is required if you want to use webhooks. Install the Svix component by doing the following steps:

1. Optional: If you want to use AWS Secret Manager, configure it by performing the following steps:

    1. Configure the secret in AWS Secret Manager by providing the following information:

        * **POSTGRES DSN** - The key is `svix-db-dsn`; an example value may be similar to `postgresql://postgres:postgres@pgbouncer/postgres`.
        * **Redis DSN** - This value is only required if you also use Redis for Svix. The key is `svix-redis-dsn`; an example value may be similar to `redis://redis:6379`.
    
    2. Configure an IAM role with the **secretsmanager:GetSecretValue** and **secretsmanager:DescribeSecret** permissions and allow it to assume the Service Account which the Svix pod will use to retrieve the secret info.

2. Optional: If you are using a self-signed TLS certificate, build and deploy a private Svix server with custom self-signed TLS certification by performing the following steps:

    1. Prepare the following Docker file to build a private Svix server image:

        ```text
        # Base build
        FROM svix/svix-server:v1.25.0
        # Add customer certification into system cert trust chain
        COPY ./customer.crt /usr/local/share/ca-certificates/
        USER root
        RUN update-ca-certificates
        # Start svix service
        USER appuser
        CMD \
            set -ex ; \
            if [ ! -z "$WAIT_FOR" ]; then \
                WAIT_FOR_ARG="--wait-for 15"; \
            fi ; \
            exec svix-server --run-migrations $WAIT_FOR_ARG
        ```

    2. Build your private Svix server image with the above Docker file and your self-signed TLS certificate file by running the following command:
    
        ```text
        docker build -t {customer-private-image-registry-url}/svix/svix-server:v1.25.tls
        ```
    
    3. Push your private Svix server image to your private image registry by running the following command:
    
        ```text
        docker push {customer-private-image-registry-url}/svix/svix-server:v1.25.tls
        ```
    
3. Run the command `./installer component -n=<namespace name>`, where `-n` indicates a namespace. The namespace must be the same as the namespace that you plan to use for Private Mendix Platform.
4. Select **Svix**, and then specify the following parameters:

    * **Image** - The Svix image path. The default path is `svix/svix-server:v1.25.0`. If you are using a self-signed TLS certificate, set this path to `{customer-private-image-registry-url}/svix/svix-server:v1.25.tls`.
    * **Use Secret Provider** - Optional. Select this option to use the AWS Secret Manager. Selecting this option enables the following additional fields:

        * **Secret Provider** - Set to **AWS** by default.
        * **AWS-Role-ARN** - An AWS role ARN which can access the specified Secret Manager.
        * **AWS SecretManager Name** - The AWS Secret Manager name where the sensitive data is stored.

    * **POSTGRES_DSN** - Available only if you do not use the AWS Secret Manager. A Postgres DSN, for example, `postgresql://postgres:postgres@pgbouncer/postgres`.
    * **Use Redis** - Optional. Select this check box if you want to use Redis for message cache and queues.
    * **REDIS_DSN** - Available only if you do not use the AWS Secret Manager. The Redis DSN, for example, `redis://redis:6379`. This field is only available if you select the **Use Redis** check box.

5. Click **Install Svix** or **Upgrade Svix**.

{{% alert color="info" %}}
The installer does not catch your pod's running status. In case of issues, verify that the pod is running correctly.
{{% /alert %}}

## Installing the Private Mendix Platform

Install the Private Mendix Platform by doing the following steps:

1. Run the command `./installer platform -n=<namespace name>`, where `-n` is the same namespace as the one where you installed Svix and PCLM.
2. Click **Configure Namespace**.

    {{< figure src="/attachments/private-platform/pmp-install6.png" class="no-border" >}}

3. Click **Configure**, and then specify the following parameters:

    * **AppName** - The default app name is `mxplatform`. You can change it as required.
    * **DatabasePlan** - If you want to use AWS Secret Manager, select **USE-Secret-Provider**; the installer then uses the database configuration set in AWS Secret Manager. Otherwise, enter the name of the database plan that you created in [Installing and Configuring the Mendix Operator](#install-operator).
    * **Storageplan** - If you want to use AWS Secret Manager, select **USE-Secret-Provider**; the installer then uses the storage configuration set in AWS Secret Manager. Otherwise, enter the name of the storage plan that you created in [Installing and Configuring the Mendix Operator](#install-operator).
    * **AppUrl** - The endpoint where you can connect to your running app. It must be a URL which is supported by your platform. If you leave it blank, Mendix Operator will create it.
    * **EnableTLS** - Allows you to enable or disable TLS for the Mendix app's Ingress or OpenShift Router. The default value is use the default settings.
    * **TLS option** - Allows you to use an existing `kubernetes.io/tls` secret containing the TLS certificate, or to provide the `tls.crt` and `tls.key` values directly.
    * **TLS Secret** - An existing `kubernetes.io/tls` secret containing the TLS certificate. Cannot be used together with certificate and key. If you leave it blank, the default TLS certificate from the Ingress Controller or OpenShift Router will be used.
    * **TLS certificate** and **TLS key** – Allows you to provide the `tls.crt` and `tls.key` values directly (not recommended for production environments). Cannot be used together with secretName.
    * **SourceUrl** - The location of the deployment package, in the format `oci-image://<your image location>`. This location must be accessible from your cluster.
    * **Replicas** – When you deploy your app, one replica is deployed automatically. Do not increase the number of replicas yourself, as this may cause data to be duplicated.

    {{< figure src="/attachments/private-platform/pmp-install7.png" class="no-border" >}}

4. Click **Runtime**, and then specify the following parameters:

    * **MxAdminPassword** - Optional. The password for the admin user, required if you are not planning to use the AWS Secret Manager. It must have at least one number, one upper case letter, one lower case letter and one symbol, with a minimum length of 12 characters.
    * **dtapmode** - For production deployments, leave this value set to **P**. For the development of the app, for example acceptance testing, set the value to **D**.
    * **ApplicationRootUrl** - Optional. Manually specify the URL of your Private Mendix Platform, for example, for use with SSO or when sending emails. For more information about this functionality, see [ApplicationRootUrl Needs to be Set Manually](/developerportal/deploy/private-cloud-operator/#applicationrooturl-needs-to-be-set-manually).
    * **Use Secret Provider** - Optional. Select this option to use the AWS Secret Manager. Selecting this option enables the following additional fields:
        * **Secret Provider** - Set to **AWS** by default.
        * **AWS-Role-ARN** - An [AWS role ARN](https://docs.mendix.com/developerportal/deploy/secret-store-credentials/#aws-secrets-manager) which can access the specified Secret Manager.
        * **AWS SecretManager Name** - The AWS Secret Manager name where the sensitive data is stored.

5. In the **Enabled Functions** section, select or clear the functions that you want to enable or disable:
 
    * **Persist Config** - When enabled, this setting locks the Private Mendix Platform configuration, so that it can no longer be modified from the user interface.
    * **Project Management** - Recommended. Enables you to create and manage your app projects. Enables app projects and related settings across the portal. Must be enabled for CI/CD capabilities.
    * **Marketplace** - Recommended. Enables you to use the Private Platform's Marketplace capabilities to upload, import and manage Marketplace contents. The Marketplace enabled here is hosted entirely within your Private Mendix Platform.
    * **Marketplace Approvals** - Optional. If enabled, contents that users publish to the private Marketplace require administrator approval before publishing.
    * **Marketplace Import** - Optional. Enables content import with an external source.
    * **IDP** - Optional. Enable users to login using SSO by configuring your IdP integration.
    * **Webhook** - Optional. Webhooks allow to send information between platform and external systems, and can be triggered by events around Apps, Users, Groups, Marketplace and CI/CD.

6. Click **Review and Apply** > **Evaluate Configuration**.
7. Make any required changes or click **Run Test App**.

    {{< figure src="/attachments/private-platform/pmp-install9.png" class="no-border" >}}

8. After the test installation is completed, keep the installer open so you can reuse the settings and apply them to the installation later.
9. Open the endpoint URL that you configured as the **AppURL** in step 3 above and verify that you can upload a test file.
10. In the Private Mendix Platform installer, click **Apply Configuration**.
11. Click **OK** to remove the test installation and install Private Mendix Platform.

{{< figure src="/attachments/private-platform/pmp-install10.png" class="no-border" >}}

### Adding Additional Components After Installing the Private Mendix Platform

To ensure that components such as svix and PCLM work correctly, you should install them before you install the Private Mendix Platform itself. If you want to add a component after the Platform installation (for example, if you want to install svix because you decided to enable webhooks), you must perform the following steps:

1. Install the component as described in [Installing Private Cloud License Manager](#install-pclm) and [Installing the Svix Component](#install-svix).
2. Run the command `./installer platform -n=<namespace name>`, where `-n` is the same namespace as the one where you installed Svix and PCLM.

Re-running the installation command ensures that the installer fetches the relevant information from the components that you added.

## Upgrading the Private Mendix Platform {#upgrade}

If you have installed Private Mendix Platform before, you can upgrade it by doing the following steps:

1. Ensure that your Mendix Operator version is 2.12 or newer.
2. Run the command `./installer platform -n=<namespace name>`, where `-n` indicates the namespace where your Private Mendix Platform is installed.
3. Click **Upgrade Namespace**.

    {{< figure src="/attachments/private-platform/pmp-upgrade1.png" class="no-border" >}}

4. Verify the following settings:
    
    * **Persist Config** - When enabled, this setting locks the Private Mendix Platform configuration, so that it can no longer be modified from the user interface.
    * **Project Management** - Recommended. Enables you to create and manage your app projects. Enables app projects and related settings across the portal. Must be enabled for CI/CD capabilities.
    * **Marketplace** - Recommended. Enables you to use the Private Platform's Marketplace capabilities to upload, import and manage Marketplace contents. The Marketplace enabled here is hosted entirely within your Private Mendix Platform.
    * **Marketplace Approvals** - Optional. If enabled, contents that users publish to the private Marketplace require administrator approval before publishing.
    * **Marketplace Import** - Optional. Enables content import with an external source.
    * **IDP** - Optional. Enable users to login using SSO by configuring your IdP integration.
    * **Webhook** - Optional. Webhooks allow to send information between platform and external systems, and can be triggered by events around Apps, Users, Groups, Marketplace and CI/CD.

5. Click **Run Upgrade**.

    {{< figure src="/attachments/private-platform/pmp-upgrade2.png" class="no-border" >}}

{{% alert color="info" %}}
To upgrade the PCLM component, select the option **Upgrade PCLM** in the upgrade wizard. For the Svix component, you can use the Svix panel to upgrade directly.
{{% /alert %}}

## Running the Private Platform Configuration Wizard {#wizard}

After you install Private Mendix Platform, run a one-time configuration wizard to configure the necessary settings.

To start the wizard, log in to your Private Mendix Platform app with the user ID *Admin*. The wizard starts automatically and walks you through the required configuration steps. For more information about the available options, refer to the sections below.

{{% alert color="info" %}}
The settings that are enabled for your Private Mendix Platform depend on the service package that you have purchased. Because of that, some of the settings listed below may be disabled for your platform.
{{% /alert %}}

### Configuring IdP Settings

In this step, you can specify whether you want to enable logging in via SSO for your users. Private Mendix Platform supports OIDC and SAML identity providers.

{{< figure src="/attachments/private-platform/pmp-wizard1.png" class="no-border" >}}

### Configuring Management Settings

In this step, you can specify whether you want to create and manage your app projects in Private Mendix Platform. If you enable the project management, you must also specify the Git host that will be used for the project. This option must be enabled if you want your Private Mendix Platform to support CI/CD capabilities.

{{< figure src="/attachments/private-platform/pmp-wizard2.png" class="no-border" >}}

### Configuring CI/CD Settings

In this step, you can enable CI/CD capabilities for your app. If you enable this option, you must also specify your CI system, configure the necessary settings, and register a Kubernetes cluster.

{{< figure src="/attachments/private-platform/pmp-wizard3.png" class="no-border" >}}

### Configuring Marketplace Settings

In this step, you can enable your app to upload and download connectors from the Marketplace.

{{% alert color="info" %}}
The Marketplace enabled here is hosted entirely within your Private Mendix Platform.
{{% /alert %}}

{{< figure src="/attachments/private-platform/pmp-wizard4.png" class="no-border" >}}

### Configuring Custom Branding Settings

In this step, you can customize the branding for your app. You may change the name that is displayed in the top bar, upload a new logo, or change the default login page image.

{{< figure src="/attachments/private-platform/pmp-wizard5.png" class="no-border" >}}

### Reviewing and Confirming the Settings

After the wizard finishes running, you are logged in to your Private Mendix Platform. The settings that you previously selected are displayed on screen. You can review and update them now, or at a later point by using the **Settings** menu in the upper left corner of the screen.

## Next Steps

After completing the installation and first-time configuration wizard, configure the remaining necessary settings. For more information, see [Configuring Private Mendix Platform](/private-mendix-platform-configuration/).
