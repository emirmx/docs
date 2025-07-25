---
title: "Configuring External Secret Management with AWS Secret Manager"
url: /private-mendix-platform/configure-aws-secret-manager/
description: "Documents the configuration of AWS Secret Manager for the Private Mendix Platform."
weight: 40
---

## Introduction

The Private Mendix Platform offers enhanced security and flexibility for credential management by supporting [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) as an external secret management solution, alongside the traditional database storage option. In the legacy database storage approach, the credentials are encrypted and stored directly in the Private Mendix Platform database. With AWS Secrets Manager, credentials are instead stored in AWS Secrets Manager and accessed securely via IAM roles for improved security, centralized management, and compliance with enterprise security policies. This document describes how you can configure AWS Secrets Manager integration for your Private Mendix Platform project.

## Prerequisites

Before configuring AWS Secrets Manager integration, prepare the following:

* An AWS account with appropriate permissions to create and manage secrets
* IAM permissions to create roles and policies for AWS Secrets Manager access
* Access to the PPrivate Mendix Platform project admin panel with administrative privileges
* Basic knowledge of AWS services, IAM roles, and Kubernetes (if using EKS deployment)
* An existing EKS cluster (if your PMP deployment runs on Kubernetes)

## Configuring External Secret Management

To configure external secret management, you must first create a secret in AWS Secret Manager, configure the IAM permissions and service accounts, and then configure the required credentials in the Private Mendix Platform administrator panel. For more information, refer to the sections below.

### Creating a Secret

To create a secret in AWS Secret Manager, perform the following steps:

1. Log in to the AWS Management Console.
2. Navigate to the **AWS Secrets Manager** service.
3. Click **"Store a new secret**.
4. Choose the type of secret as **Other type of secret**.
5. Select the **JSON** format for storing secrets.
6. Enter the key-value pairs for your secrets using the PMP naming convention. You can get the complete template from [here](!!link tbd).
7. Click **Next**.
8. Enter a descriptive name for your secret, for example, *PMP-Production-Credentials* or *PMP-VCS-Credentials*.
9. Optional: Add a description and tags for better organization and compliance tracking.
10. Click **Next** to review your secret settings.
11. Review the details and click **Store** to create the secret.

{{% alert color="info" %}}
Make note of the secret name and ARN. You will need these when configuring Private Mendix Platform to use the secret.
{{% /alert %}}


### Configuring IAM Permissions and Service Accounts

### Configuring the Credentials