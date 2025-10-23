---
title: "Security & Compliance for Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/security-and-compliance/
weight: 20
description: "Describes the security & compliance considerations for apps running on Mendix on Azure."
---

## Security & Compliance

Mendix accesses customer environments in a secure, auditable way:

* We use [cross-tenant access](https://learn.microsoft.com/en-us/entra/external-id/cross-tenant-access-overview), which is native to Azure and complies with Microsoft best practices.
* Most access is performed programmatically, that is, by the system rather than manually by normal users. There is usually no human intervention in customer environments.
* Any network connectivity is  using a private Azure link service, not through the public internet.

### SOC 2 Type 2 Compliance Exceptions

The Azure Policy add-on is not enabled inside Mendix Azure clusters, because Mendix can control which workloads can access the cluster. Because of that, the following exceptions to the SOC 2 Type 2 policy are considered acceptable:

* Azure Container Registry:
    * [Container registries should be encrypted with a customer-managed key](https://www.azadvertizer.net/azpolicyadvertizer/5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580.html) - The standard Microsoft key is used instead.
* AKS - cluster resource:
    * [Azure Policy Add-on for Kubernetes service (AKS) should be installed and enabled on your clusters](https://www.azadvertizer.net/azpolicyadvertizer/0a15ec92-a229-4763-bb14-0ea34a568f8d.html) - The cluster is deployed and managed by Mendix, so the policy is not needed.
    * [Azure Kubernetes Service clusters should have Defender profile enabled](https://www.azadvertizer.net/azpolicyadvertizer/a1840de2-8088-4ea8-b153-b4c723e9cb01.html) - This is not automated for cost-saving reasons.
* AKS - cluster VNET:
    * [All Internet traffic should be routed via your deployed Azure Firewall](https://www.azadvertizer.net/azpolicyadvertizer/fc5e4038-4584-4632-8c85-c0448d374b2c.html) - This is not automated, but the customer can deploy their own Firewall if required.
* Storage Account:
    * [Storage accounts should use customer-managed key for encryption](https://www.azadvertizer.net/azpolicyadvertizer/6fac406b-40ca-413b-bf8e-0bf964659c25.html) - The cluster is deployed and managed by Mendix, so this is not needed.