---
title: "Getting Started with Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/quickstart/
description: "Documents the pre-implementation tasks for Mendix on Azure."
weight: 10
---

## Introduction

Before you can deploy your Mendix app on Azure, you must plan and complete a number of pre-implementation tasks.

## Prerequisites

To adopt Mendix on Azure, you need to have the following:

* A Mendix account; Mendix Studio Pro 10.10 or newer is required
* An Azure account with the following permissions:
    * Permission to grant admin consent on the Mendix on Azure portal app registration
    * Owner or Mendix on Azure Operator custom role assigned on the target subscription level

## Licensing

Mendix on Azure is available for purchase from the the [Azure Marketplace](https://azuremarketplace.microsoft.com/). Connecting to Azure services may also include additional cost in the form of Azure tokens. For more information, refer to Azure documentation.

Depending on your use case, your deployment environment, and the type of app that you want to build, you may also need a license for your Mendix app. For more information, refer to [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/).

In addition to the licenses for your apps, you will also need to license the Mendix Operator which helps deploy your app to a Mendix on Azure environment. For details on the Mendix Operator license, see [Licensing Mendix for Private Cloud](/developerportal/deploy/private-cloud/#licensing).

## Shared Responsibility Model

Under the shared responsibility model for Mendix on Azure deployments, Mendix, Microsoft, and customer organizations all have their own responsibilities in the deployment process and business-as-usual operations. Familiarize yourself with the responsibilities listed below:

### Microsoft Responsibilities

Microsoft is responsible for operating and securing the Azure services underlying the Mendix on Azure service. This includes the following services:

* Compute
    * Azure Kubernetes service
* Storage
    * Azure Blob Storage
    * Azure Container Registry
* Database
    * PostgreSQL Flexible Server
* Networking
    * Virtual networks
    * Load balancer
    * Private endpoints
* Monitoring
    * Managed Grafana and Prometheus

### Mendix Responsibilities

Mendix is responsible for orchestrating, operating, maintaining, securing, and supporting the Mendix on Azure service. This includes the following tasks:

* Orchestrating - Ensure that the underlying Azure services function together as one cohesive offering.
* Operating - Resolve regressions in how the underlying Azure services come together as one service.
* Maintaining - Ensure that the service absorbs changes in the underlying Azure services without impact on customers.
* Securing - Ensure that the service remains compliant with relevant security best practices and frameworks.
* Supporting - Reactively address customer issues with using the service.

### Customer Responsibilities

Customers are responsible for developing, deploying, operating, integrating, and securing apps on top of the Mendix on Azure service. This includes the following tasks:

* Developing - Create apps that deliver business outcomes.
* Deploying - Deploy apps.
* Operating - Monitor app behavior and address deviations.
* Integrating - Securely integrate apps with backend services and IAM.
* Securing - Comply with Mendix best practices for secure apps.

## Environment Planning

When planning the implementation, keep in mind the following environment specifications.