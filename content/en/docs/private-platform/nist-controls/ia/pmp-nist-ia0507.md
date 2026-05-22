---
title: "IA-05 (07) Authenticators Encryption"
linktitle: "IA-05 (07)"
url: /private-mendix-platform/nist-controls/ia-0507/
description: "Documents the Private Mendix Platform's compliance with the IA-05 (07) control of the NIST 800-53 framework."
weight: 20
---

## Introduction

This document describes how Private Mendix Platform fulfills the IA-05 (07) control.

| Control ID | IA-05 (07) |
| --- | --- |
| Control category | IA -  Identification and Authentication |
| Requirement baseline | FEDRAMP MODERATE |
| Responsibility and ownership | Mendix - Private Mendix Platform, Mendix - Operator, Mendix - Studio Pro/Runtime, Customer - Infra |

## Control {#control}

The organization ensures that unencrypted static authenticators are not embedded in applications or access scripts or stored on function keys.

### Supplemental Guidance

Organizations exercise caution in determining whether embedded or stored authenticators are in encrypted or unencrypted form. If authenticators are used in the manner stored, then those representations are considered unencrypted authenticators. This is irrespective of whether that representation is perhaps an encrypted version of something else (for example, a password).

## Responsibility

### Mendix Responsibility

Private Mendix Platform provides two ways to store authentication credentials. One is internal database storage, another is external secret storage (AWS secrets manager, Key Vault, and so on).

### Customer Responsibility

The customer must set up the external secret manager and configure the corresponding secrets in external secret storage.

## Guidance

### Mendix Responsibility

In Private Mendix Platform, in Admin mode, navigate to **Settings > Version Control > Project Admin PAT** to configure the **Project Admin Personal Access Token (PAT)**. Private Mendix Platform supports two secret storage options:

* Database - The Project Admin PAT is stored in the Private Mendix Platform database and encrypted using the Advanced Encryption Standard (AES) algorithm.
* AWS Secrets Manager - Private Mendix Platform retrieves the Project Admin PAT from AWS Secrets Manager, where it is securely stored and encrypted using the Advanced Encryption Standard (AES) managed by AWS Key Management Service (AWS KMS).

All credentials managed by Private Mendix Platform are stored securely using one of the above methods.

## Proof and Remarks

The **Project Admin PAT** field is available at ** Settings > Version Control**: 

{{< figure src="/attachments/private-platform/nist-ia/nist-ia-0507-1.png" class="no-border" >}}

The **Token** field is available at **Settings > Build > Build Cluster Setting**

{{< figure src="/attachments/private-platform/nist-ia/nist-ia-0507-2.png" class="no-border" >}}