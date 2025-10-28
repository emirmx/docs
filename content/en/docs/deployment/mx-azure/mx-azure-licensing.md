---
title: "Licensing Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/license/
description: "Provides information about licensing Mendix on Azure."
weight: 35
---

## Licensing Mendix on Azure

### Introduction

Mendix on Azure is one of the deployment options for Mendix, featuring its own licensing model separate from the base Mendix platform and user fees.

{{% alert color="info" %}}  
All details below apply exclusively to Mendix on Azure charges. Licensing and user fees for the Mendix platform itself are billed separately.  
{{% /alert %}}

### Trial Period

Mendix on Azure is available to deploy via the Azure Marketplace. While the marketplace offering itself is free to deploy, Microsoft will charge you directly for any Azure resources deployed under your organization's Azure subscription according to your agreement with Microsoft.

For the first 120 days after creating any new app environment within your Mendix on Azure cluster, Mendix does **not** charge for usage of Mendix on Azure.

### Paid Period

After 120 days from an app environment’s creation, Mendix begins charging a fixed number of Cloud Tokens per app environment (14 tokens as of 2025). If your token balance is insufficient, Mendix will contact you to arrange replenishment.

### Frequently Asked Questions

**1. Will I still need to request runtime licenses for my Mendix apps?**

Yes. For production environments, you must supply runtime licenses for your Mendix apps to move the Mendix Runtime out of trial mode. This licensing process aligns with how Mendix on Kubernetes works (e.g., requesting and applying subscription keys). See [Licensing Apps outside Mendix Cloud](http://localhost:1313/developerportal/deploy/licensing-apps-outside-mxcloud/).

Note that the Operator license is applied automatically with Mendix on Azure and does not require a separate request.
