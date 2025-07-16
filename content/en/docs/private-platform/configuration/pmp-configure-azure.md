---
title: "Configuring CI/CD on Azure"
url: /private-mendix-platform/configure-azure/
description: "Documents the initial configuration for the Private Mendix Platform."
weight: 30
aliases:
    - /private-mendix-platform-configure-azure/
---

## Introduction

This document explains the configuration options available when configuring a Continuous Integration and Delivery (CI/CD) solution for Private Mendix Platform on the Azure DevOps service.

### Prerequisites

To configure the CI/CD pipeline, prepare the following:

* An Azure organization where you want to build your Mendix app.
* An Azure blob or an AWS S3 endpoint where you can store the built MDA files.

## Configuring the CI/CD Pipeline

If you have an Azure organization, you can set Azure as your CI System in **Switch to Admin Mode** > **Settings** > **Build Settings** > **Build Method** > **Build Utility**. You need to first obtain a [Personal Access Token](#pat), and then configure the followings settings:

* [Azure blob settings](#blob)
* [S3 bucket settings](#bucket)

Finally, you must also [register your Kubernetes cluster](/private-mendix-platform/reference-guide/admin/company/#cluster-manager).

{{< figure src="/attachments/private-platform/pmp-cicd4.png" class="no-border" >}}

### Obtaining a Personal Access Token {#pat}

A Personal Access Token (PAT) is used to authenticate in Azure DevOps. For information about obtaining the token, see [Create a PAT](https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Windows#create-a-pat) in the Azure DevOps documentation.

### Configuring Azure Blob Settings {#blob}

The settings in this section configure the Azure blob settings.

* **Azure Blob URL** - For example, `https://{your domain name}.blob.core.windows.net/pmp`.
* **Azure Blob Token** - This secret value is used to access the Azure Blob storage.

### Configuring Build Images Setting {#bucket}

The settings in this section configure the S3 bucket.

* **S3 Endpoint** - For example, `Cloud Object Storage - Amazon S3 - AWS`.
* **S3 Bucket Name** - Your S3 bucket name, for example, `mybucket`.
* **Region** - For example, `ap-southeast-1`.
* **Access Key ID** - This ID value is used to access the S3 bucket.
* **Secret Access Key** - This secret key value is used to access the S3 bucket.

### Building an App with the Azure DevOps Pipeline

In Admin mode, after input all correct settings include Azure DevOps URL, Organization, PAT, Blob or S3 Storage, Click Save to store all settings.
Enter User mode, select one App to create package with Azure DevOps build utility, it's expected that App package is built successfully.
If App package build failed, please access Azure DevOps Organization pipeline build page https://dev.azure.com/rax-mh/ProjectForPmpBuildAppMda/_build to check what error happen, if the error message "No hosted parallelism has been purchased or granted" reported in pipeline job, please buy or request a free parallelism grant from Microsoft Azure DevOps Service, by filling out and submit the form from the page  Illuminated by Isis , wait for Microsoft Azure approve your request and then retry your building.

## Architecture of the CI/CD Pipeline

The diagrams in this section present the architecture and components of the pipeline. The architecture is different depending on whether you enabled the Auto Detect Mx Version build image setting.

### Architecture with the Auto Detect Mx Version Setting Enabled

The following diagram shows the architecture of the pipeline if you enable the **Auto Detect Mx Version** setting. For more information, see [Build Images Setting](/private-mendix-platform/configure-k8s/#build-images).

{{< figure src="/attachments/private-platform/pmp-cicd2.png" alt="Auto Detect Mx Runtime Version" class="no-border" >}}

### Architecture with the Auto Detect Mx Version Setting Disabled

The following diagram shows the architecture of the pipeline if you disable the **Auto Detect Mx Version** setting. For more information, see [Build Images Setting](/private-mendix-platform/configure-k8s/#build-images).

{{< figure src="/attachments/private-platform/pmp-cicd3.png" alt="User Input Mx Runtime Version" class="no-border" >}}
