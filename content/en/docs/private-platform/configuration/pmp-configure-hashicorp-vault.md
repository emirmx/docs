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



### Creating a Secret



#### Naming Convention for Key Properties {#naming-convention}



