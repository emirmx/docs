---
title: "Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/
description: "Presents documentation on deploying your Mendix app on Microsoft Azure."
weight: 42
---

## Introduction

Mendix on Azure provides a simplified, integrated way to deploy Mendix applications to a Microsoft Azure environment. With this solution, users are empowered to deploy their Mendix applications in Azure environments without the need for intricate infrastructure setup in cloud services. They can also seamlessly manage infrastructure services through an intuitive user interface. No matter their IT skills, users can realize their project value quickly and securely with Azure.

## Features of Mendix on Azure

Mendix on Azure has the following features:

* You can create the managed app in Azure and link it to your Mendix Private Cloud environment.
* You do not need to perform software upgrades, as they are done for you.
* The environment is set up in an opinionated way according to the architecture prepared by Mendix.

## Typical Use Cases

Mendix on Azure supports the following use cases:

* Geographical data control - For organizations which must keep data within certain regions due to legal or contractual obligations.
* Industry-specific compliance - For industries such as healthcare, finance, or government, which have strict regulatory compliance requirements.
* App data sensitivity - For applications which deal with highly sensitive data or are subject to stringent security regulations, Mendix on Azure provides the option to keep this data within the organization's own security perimeter.
* Legacy systems integration - For integrating with existing legacy systems that are not easily migrated to a public cloud.

## Mendix on Azure and Mendix for Private Cloud

Mendix on Azure is a new deployment option that makes use of some of the features of Mendix for Private Cloud, but does so in an opinionated way. Mendix for Private Cloud offers its users flexibility coupled with the ability to keep their deployment within their enterprise firewall, but requires more effort to configure and more time to value than deployments on Mendix Cloud. Mendix on Azure builds on that by providing an automated, preconfigured solution with access to private customer networks, which can be deployed in 30 minutes by a user without IT skills at no extra operational costs. The architecture, its maintenance, updates, and security hardening are all fully managed by Mendix.

## Architecture

The diagram in this section presents the high-level architecture of the Mendix for Azure solution.

{{< figure src="/attachments/deployment/mx-azure/architecture.png" class="no-border" >}}

The architecture is assessed against the [Azure well-architected framework](https://learn.microsoft.com/en-us/azure/well-architected/) to ensure its reliability, accessibility, and performance.

### Use of PostgreSQL over Azure SQL

The Mendix on Azure solution uses PostgreSQL rather than Azure SQL.

## Security

Mendix access to customer environments uses private customer endpoints.

### SOC 2 Type 2 Compliance Exceptions

The Azure Policy add-on is not enabled inside Mendix Azure clusters, because Mendix can control which workloads can access the cluster. Because of that, the following exceptions to the SOC 2 Type 2 policy are considered acceptable:

* Azure Container Registry:
    * Container registries should be encrypted with a customer-managed key
* AKS - cluster resource:
    * Azure Policy Add-on for Kubernetes service (AKS) should be installed and enabled on your clusters
    * Azure Kubernetes Service clusters should have Defender profile enabled
* AKS - cluster VNET:
    * All Internet traffic should be routed via your deployed Azure Firewall
    * Subnets should be associated with a Network Security Group
* PostgreSQL:
    * Subnets should be associated with a Network Security Group
* Storage Account:
    * Storage accounts should use customer-managed key for encryption

## Read More
