---
title: "Configuring External Secret Management with HashiCorp Vault"
url: /private-mendix-platform/configure-hashicorp-vault/
description: "Documents the configuration of Hashicorp Vault for the Private Mendix Platform."
weight: 40
---

## Introduction

The Private Mendix Platform offers enhanced security and flexibility for credential management by supporting HashiCorp Vault as an external secret management solution. This integration uses Vault's Kubernetes Auth Method, allowing your Mendix application running in Kubernetes to authenticate password-lessly using its assigned Kubernetes Service Account identity.

This document describes how to configure HashiCorp Vault integration using either a custom Kubernetes Service Account (for example, the recommended value is `pmp-secret-accessor`) or the built-in default Service Account. It utilizes a centralized secret structure within Vault for storing all platform credentials.

{{% alert color="info" %}}
This integration requires the use of HashiCorp Vault's Key-Value (KV) Version 2 secrets engine. Using KV Version 1 or other secret engines is not supported and will not work.
{{% /alert %}}

## Prerequisites

Before configuring HashiCorp Vault integration, prepare the following:

* A running HashiCorp Vault instance (v1.9+ recommended) accessible from your Kubernetes cluster, with the KV Version 2 secrets engine enabled at your chosen mount path (for example, `pmp-dev`).
* Vault administrative privileges to enable `auth` methods, create policies, and create roles.
* Access to the Private Mendix Platform project admin panel with administrative privileges.
* Basic knowledge of HashiCorp Vault, Vault policies, KV v2 engine, and Kubernetes.
* An existing Kubernetes cluster (for example, EKS, AKS, OpenShift, Generic) with the OIDC Issuer feature enabled and discoverable.
* If using a custom Service Account, permissions to create or modify `ServiceAccount` and Mendix Runtime resources in your Kubernetes cluster.
* Kubectl configured to connect to your target Kubernetes cluster.
* Vault CLI installed and configured (at least initially with admin access to Vault).
* Cloud provider CLI (for example, `aws`, `az`) if using EKS or AKS, for easier configuration discovery.
* Optional: `jq` command-line JSON processor (`brew install` jq or download) for easier parsing of CLI output.

## Configuring External Secret Management

To configure external secret management, perform the following steps:

1. Store your credentials in a centralized KV v2 path. For more information, see [Creating Secrets in Vault](#create-vault-secret).
2. Enable and configure the Kubernetes authentication method, create a policy, and create a role bound to the chosen Service Account (custom or default). For more information, see [Configuring the Kubernetes Authentication Method](#configure-k8s-auth).
3. 

### Creating Secrets in Vault {#create-vault-secret}

Store all Private Mendix Platform secrets as key-value pairs within a single secret path in Vault's KV Version 2 engine (for example, `pmp-dev/admin`). This centralizes all credentials.

Within this single secret path, the key must exactly match the credential name required by Mendix (using dots, for example, `Marketplace.ImportCDNPassword`), and the value must be the actual secret string. 

{{% alert color="info" %}}
Do not use a nested `value=` key structure.
{{% /alert %}}

1. Log in to the Vault CLI with appropriate permissions (for example, `set VAULT_ADDR` and `VAULT_TOKEN`).
2. If your target mount path (for example, `pmp-dev/`) does not exist, or is not a KV v2 engine, run the following command:

```bash
vault secrets enable -path=pmp-dev kv-v2
```
    If the path is already in use, the command fails safely.

3. Run the `vault kv put` command once for the central secret path (for example, `pmp-dev/admin`), listing all non-empty key-value pairs as arguments.

{{% alert color="info" %}}
Crucially for KV v2, the path structure for the `put` command is `{KV_MOUNT}/data/{CENTRAL_SECRET_NAME}`.

Use the Mendix Key Name as the key. For more information about the naming convention, see [Naming Convention for Key Properties](#naming-convention). Only include keys that have non-empty values. Replace placeholder values with your actual secrets.
{{% /alert %}}

For example, if your **Central Secret Name** in the Mendix UI will be `http://<VAULT_ADDR>:8200/pmp-dev/admin`, run the following Bash script:

```bash
# Note the required "/data/" in the path for the CLI 'put' command with KV v2
# Add ALL non-empty secrets as key=value pairs to this single command
vault kv put pmp-dev/data/admin \
    VCS.BitbucketProjectAdminPAT="YOUR_PAT_TOKEN" \
    VCS.BitbucketAdminPassword="YOUR_PASSWORD" \
    VCS.AzureDevOpsOrgAdminPAT="YOUR_AZDO_PAT" \
    BuildPackage.FileBasicAuthPassword="YOUR_PASSWORD" \
    BuildPackage.AwsSecretAccessKey="YOUR_AWS_SECRET_KEY" \
    RuntimeBaseImage.PrivateRegistryPassword="YOUR_REGISTRY_PASSWORD" \
    MDAStorage.FileBasicAuthPassword="YOUR_PASSWORD" \
    MDAStorage.AwsSecretAccessKey="YOUR_AWS_SECRET_KEY" \
    OCIRegistry.PrivateRegistryPassword="YOUR_OCI_PASSWORD" \
    OCIRegistry.S3CompatibleAccessKey="YOUR_S3_KEY" \
    BuildCluster.KubernetesConfigureToken="YOUR_K8S_TOKEN" \
    CIAdmin.JenkinsConfigureAPIToken="YOUR_JENKINS_TOKEN" \
    CIAdmin.JenkinsTriggerAuthToken="YOUR_TRIGGER_TOKEN" \
    ClusterManager.KubernetesApiToken="YOUR_K8S_API_TOKEN" \
    Marketplace.ImportCDNPassword="YOUR_MARKETPLACE_PASSWORD" \
    Email.SMTPPassword="YOUR_SMTP_PASSWORD"
    # Add any other non-empty key=value pairs here based on the Naming Convention
```

This command creates or updates the single `admin` secret under the `pmp-dev` mount path to contain all the specified credentials.

#### Example Structure

* **KV v2 Engine Mount Path** - `pmp-dev/`
* **Central Secret Name** - `admin` (This single path under the mount holds all secrets.)
* **Your UI Input (SecretName)** - `http://<VAULT_ADDR>:8200/pmp-dev/admin` (Used for all credentials.)
* **Credential Key (KeyName)** - `Marketplace.ImportCDNPassword`
* **Vault API Path** - `pmp-dev/data/admin`
* **Data Stored** - `{"Marketplace.ImportCDNPassword": "PLACEHOLDER_PASSWORD", "VCS.BitbucketAdminPassword": "PLACEHOLDER_PASSWORD", ...}`

#### Naming Convention for Key Properties {#naming-convention}

Use the exact key names specified by Private Mendix Platform, with dots (`.`) as separators, as the keys within your central Vault secret (for example, `pmp-dev/admin`).

* **VCS**

    * `VCS.BitbucketProjectAdminPAT`
    * `VCS.BitbucketAdminPassword`
    * `VCS.GitlabGroupOwnerPAT`
    * `VCS.GitlabAdminPAT`
    * `VCS.GithubOrgOwnerPAT`
    * `VCS.GithubAdminPAT`
    * `VCS.GithubEnterpriseClientSecret`
    * `VCS.AzureDevOpsOrgAdminPAT`
    * `VCS.AzureAuthSecret`

* **Kubernetes Build Settings**

    * `BuildPackage.FileBasicAuthPassword`
    * `BuildPackage.AwsSecretAccessKey`
    * `RuntimeBaseImage.PrivateRegistryPassword`
    * `RuntimeBaseImage.S3CompatibleAccessKey`
    * `MDAStorage.FileBasicAuthPassword`
    * `MDAStorage.AwsSecretAccessKey`
    * `OCIRegistry.PrivateRegistryPassword`
    * `OCIRegistry.S3CompatibleAccessKey`

* **Build Cluster Settings**

    * `BuildCluster.KubernetesConfigureToken`
    * `CIAdmin.JenkinsConfigureAPIToken`
    * `CIAdmin.JenkinsTriggerAuthToken`
    * `CIAdmin.AzureOrgAdminPAT`
    * `CIAdmin.AzureBlobStorageToken`
    * `CIAdmin.AzureAwsS3SK`

* **Cluster Manager**

    * `ClusterManager.KubernetesApiToken`
    * `ClusterSettings.KubernetesAdminPassword`
    * `ClusterSettings.GrafanaAPIKey`
    * `ClusterSettings.MDAAWSS3AccessKey`
    * `ClusterSettings.OCIRegistryPassword`

* **Marketplace**

    * `Marketplace.ImportCDNPassword`

* **Email**

    * `Email.SMTPPassword`

### Configuring the Kubernetes Authentication Method {#configure-k8s-auth}

This allows pods to authenticate using their Kubernetes Service Account token.

### Configuring Kubernetes for Private Mendix Platform

To configure Kubernetes for Private Mendix Platform, ensure that your Mendix application runs with the Service Account specified in the Vault Role, and then perform the actions described below.

#### Configuring the Mendix Operator

To configure the Mendix Operator, perform the following steps:

1. To ensure that the Mendix Operator allows pods to mount their Service Account token, edit the `OperatorConfiguration`:

        ```bash
        # Replace <your-operator-ns> with the namespace where the Mendix Operator runs
        kubectl edit operatorconfiguration mendix-operator-configuration -n <your-operator-ns>
        ```

2. Add or confirm the following line in the `spec:` section:

        ```yaml
        spec:
            runtimeAutomountServiceAccountToken: true
            # ... other existing spec fields ...
        ```

#### Choosing and Configuring the Service Account

To configure the Service Account, select one of the following options.

##### Custom Service Account

{{% alert color="info" %}}
Ensure that you used `bound_service_account_names=pmp-secret-accessor` when creating the Vault Role.
{{% /alert %}}

Using a custom Service Account, for example, `pmp-secret-accessor`, is recommended for better isolation. To use a custom account, perform the following steps:

1. Create the Service Account in your Mendix application's namespace (replace `feature-test` with your own value if required):

    ```bash
    kubectl create serviceaccount pmp-secret-accessor --namespace feature-test --dry-run=client -o yaml | kubectl apply -f -
    ```
2. Assign the Service Account to your Mendix app by editing the Mendix Runtime custom resource. Replace `mxplatform` and `feature-test` with your own values if needed.

    ```bash
    kubectl edit runtime mxplatform -n feature-test
    ```

3. Find the pod template section within the spec (for example, `spec.template.spec`), and add or modify the `serviceAccountName` field:

    ```yaml
    # ... inside spec: template: spec: ...
    serviceAccountName: pmp-secret-accessor # Set your custom SA name
    ```

4. Save the changes. The Mendix Operator updates the deployment.

##### Default Service Account

No specific Kubernetes action is needed for the Service Account itself. Pods use the `default` Service Account if none is specified.

{{% alert color="info" %}}
Ensure that you used `bound_service_account_names=default` when creating the Vault Role.

Ensure the **serviceAccountName** field in the template specification of your Mendix Runtime custom resource pod is either not set, or explicitly set to `default`.
{{% /alert %}}