---
title: "Configuring Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/configuration/
description: "Mendix on Azure configuration."
weight: 6
---
## Introduction

Mendix on Azure is delivered with default configuration settings optimized for evaluation and initial deployments, providing a seamless experience for trying out the solution.

For production environments or to meet specific organizational requirements, additional configuration is typically needed.

Mendix on Azure offers advanced configuration through the following methods:

1. Self-service configuration via Mendix on Azure Portal  
2. Self-service configuration via Microsoft Azure Portal  
3. Self-service configuration via Mendix on Kubernetes Portal  
4. Configuration assistance upon request by submitting a support ticket through the Mendix on Azure Portal  

This document outlines the available configuration options and their functionalities.


## Self-service configuration available via Mendix on Azure portal

The [Mendix on Azure Portal](https://mendixonazure.mendix.com) provides a variety of self-service configuration options that can be modified anytime or specified once during initial cluster setup. The following sections categorize available options:

### Networking settings {#networking-settings}

| Advanced Option              | Description                                                                                                                                                                                                                                                                                                                                                                                    | Editable after initial creation |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| Load Balancer Type           | Controls whether your applications are reachable publicly or only privately via your own (Virtual) Network and/or Private Endpoints.  Endpoints.                                                                                                                                                                                                                           | Yes                             |
| AKS Node CIDR IP Range       | Defines the IP address range on the VNet hosting AKS cluster nodes. This can only be set during initial deployment and should align with your organization's IP plan if you plan to connect Mendix on Azure to other networks via peering. Default is acceptable when no interconnection is required.  | No                              |
| AKS Network Isolated Cluster | When set to true will lead to a cluster without egress configuration, please carefully read the [documentation on cluster networking modes ](/developerportal/deploy/mendix-on-azure/configuration/ingress-egress) to understand the implications                                                                                                                                                                                                                                                                                                                                     | No                              |

Please consult the documentation on [configuring Ingress and Egress](/developerportal/deploy/mendix-on-azure/configuration/ingress-egress) to ensure you deeply understand the interplay of above three options.

### Application Cluster Settings

| Advanced Option        	| Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                	| Editable after initial creation 	|
|------------------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------	|
| AKS Node VM Size       	| The VM size used on the AKS application cluster. Default should suffice in most circumstances. Other sizes can be considered in the case of:<br>- Performance issues, e.g. using non-burstable instances can improve Mendix Runtime performance<br>- Mendix app environment instances requiring more RAM than available under current selection. In case a Mendix app environment instance is configured to require more RAM than available on the current VM size, switching to a larger VM size might be required to have the app instance start at all. 	| Yes                             	|
| AKS Maximum Node Count 	| The number of available cluster nodes will be increased and decreased automatically based on the combined capacity requirement of all deployed Mendix apps. This setting controls the upper limit to the number of available nodes in order to avoid cost surprises.                                                                                                                                                                                                                                                                                       	| Yes                             	|
| AKS Service Tier       	| The [AKS service tier](https://learn.microsoft.com/en-us/azure/aks/free-standard-pricing-tiers) determines the service level Microsoft provides on the Mendix on Azure Kubernetes cluster control plane. This DOES NOT impact application performance, only Microsoft’s SLA.<br><br><br> <br>- Free should suffice in most situations.<br>- Standard can be considered by organizations that value a financially backed SLA (this has a cost impact, please check the Microsoft documentation)<br>- Premium does not offer any additional value in combination with Mendix on Azure and is discouraged                                	| Yes                             	|


### Database Settings

| Advanced Option          	| Description                                                                                                                                                                                                                                                                                           	| Editable after initial creation 	|
|--------------------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------	|
| Enable Read Replica      	| Enables a read replica for direct app database access. Please review the [detailed documentation](/developerportal/deploy/mendix-on-azure/configuration/direct-database-access/) for instructions on how to access this read replica.                                                                                                                                                  	| Yes                             	|
| Compute Tier & Size      	| Specifies the DB Compute Tier for the shared PostgreSQL database used by all Mendix app environments. You may need to increase it for better app performance. See Microsoft’s [compute options overview](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-compute).                	| Yes                             	|
| Storage Performance Tier 	| Specifies the Storage Performance Tier for the shared PostgreSQL database. Consider increasing if performance issues arise. See Microsoft’s [storage options overview](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-storage).        	| Yes                             	|

### Observability Settings

| Advanced Option               	| Description                                                                                                                                                                                                                                    	| Editable after initial creation 	|
|-------------------------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------	|
| Managed Grafana Accessibility 	| Determines whether the Managed Grafana observability dashboard is accessible publicly or only via private endpoints. [Virtual network peering](/developerportal/deploy/mendix-on-azure/configuration/interconnecting-networks#network-peering) is required for private access.     	| Yes                             	|


## Self-service configuration available via Microsoft Azure Portal

The following configurations can be modified directly through the [Microsoft Azure portal](https://portal.azure.com) on resources within the Managed Resource Group of your Mendix on Azure Managed Application:

| Configuration Option                                                                            | Description                                                                                                             |
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Configure virtual network peering on the vNet hosting Mendix on Azure                           | See [Implementing private connectivity using Azure Virtual Network Peering](/developerportal/deploy/mendix-on-azure/configuration/interconnecting-networks#network-peering) |
| Deploy Private Link Service to expose Mendix apps in other Azure virtual networks               | See [Using Private Link Service to expose Mendix apps in other Azure virtual networks](/developerportal/deploy/mendix-on-azure/configuration/interconnecting-networks#pls) |
| Deploy Private Endpoints to establish connectivity between Mendix apps and other services       | See [Accessing private services via Private Endpoints](/developerportal/deploy/mendix-on-azure/configuration/interconnecting-networks#pe-internal)                          |
| Override DNS configuration on the vNet hosting Mendix on Azure                                  | See [DNS name resolution towards resources in other networks](/developerportal/deploy/mendix-on-azure/configuration/interconnecting-networks#name-resolution-dns-override)  |

### The Mendix on Azure Managed Resource Group {#mrg}

Many Azure Portal configurations require modifying Azure resources located within the Managed Resource Group (MRG) of your Mendix on Azure environment. This resource group can be found via the Mendix on Azure Managed Application:

{{< figure src="/attachments/deployment/mx-azure/mrg.png" class="no-border" >}}

## Self-service configuration available via Mendix on Kubernetes Portal

In addition to extensive individual app environment configuration options, the [Mendix on Kubernetes Portal](https://privatecloud.mendixcloud.com) also offers cluster-wide settings applicable to Mendix on Azure clusters:
 
### Adding additional Cluster Managers

The Mendix account that initializes the cluster automatically gains the Cluster Manager role.

The Mendix on Kubernetes portal lets you add additional cluster managers. These users can view and manage the cluster through both the Mendix on Azure and Mendix on Kubernetes portals, provided they have an Owner or Contributor role on the Azure Managed Application hosting the cluster.

Additional cluster managers have the same configuration privileges as the original initializer. They can also view and comment on support tickets related to the cluster via the Mendix on Azure portal but cannot access the full Zendesk ticket.

{{% alert color="info" %}}  
Before adding a cluster manager, ensure the invited user signs in to the Mendix on Azure portal prior to accepting the invitation. Otherwise, the invitation might show as accepted, but the user won't have access to any Mendix on Azure resources.  
{{% /alert %}}

## Configuration assistance available by submitting a support ticket through the Mendix on Azure Portal

Certain configuration changes require Mendix intervention and can only be performed by submitting a support ticket via the Mendix on Azure portal:

| Configuration Change           | Description                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PostgreSQL Maintenance Window | Configure a dedicated maintenance window for the PostgreSQL database hosting your Mendix app databases. Since maintenance might cause temporary app downtime, you can request a custom schedule instead of the default system-managed one. For more details, see the [Microsoft documentation on PostgreSQL maintenance windows](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-maintenance). |

{{% alert color="info" %}}  
Please submit Mendix on Azure support tickets exclusively through the Mendix on Azure portal. Tickets created here automatically capture vital context such as cluster identifiers and logs, enabling faster, more accurate support.  
{{% /alert %}}


