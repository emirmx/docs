---
title: "Configuring Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/configuration/
description: "Mendix on Azure configuration."
weight: 6
---
## Introduction

Mendix on Azure is delivered with a set of default configuration settings optimized for evaluation and initial deployment. These defaults provide a seamless experience for trying out the solution.

For production environments or to align with specific organizational requirements, additional configuration is typically required.

Mendix on Azure offers advanced configuration capabilities through the following methods:

1. Self-service configuration via Mendix on Azure Portal  
2. Self-service configuration via Microsoft Azure Portal
3. Self-service configuration via Mendix on Kubernetes Portal  
3. Configuration assistance upon request by submitting a support ticket through the Mendix on Azure Portal  

This document outlines the available configuration options and describes their functionality.

## Self-service configuration available via Mendix on Azure portal

The [Mendix on Azure Portal](https://mendixonazure.mendix.com) provides a range of self-service configuration options that can be modified at any time or specified once during the initial cluster setup. The following sections list the available options by theme:

### Networking options

| Advanced Option              | Description                                                                                                                                                                                                                                                                                                                                                                                    | Editable after initial creation |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| Load Balancer Type           | This option controls whether your applications will be reachable from the public internet or only privately via your own (Virtual) Network and/or Private Endpoints.                                                                                                                                                                                                                           | Yes                             |
| AKS Node CIDR IP Range       | The IP address range used on the VNet that will host AKS cluster nodes. This setting can only be changed at initial deployment and should match the IP address plan of your organization in case you intend to connect the Mendix on Azure environment to the rest of your network(s) via peering. The default value can be accepted when interconnection with other networks is not required. | No                              |
| AKS Network Isolated Cluster | Please see [network isolated cluster] for more information                                                                                                                                                                                                                                                                                                                                     | No                              |


### Application Cluster Settings

| Advanced Option        	| Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                	| Editable after initial creation 	|
|------------------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------	|
| AKS Node VM Size       	| The VM size used on the AKS application cluster. Default should suffice in most circumstances. Other sizes can be considered in the case of:<br>- Performance issues, e.g. using non-burstable instances can improve Mendix Runtime performance<br>- Mendix app environment instances requiring more RAM than available under current selection. In case a Mendix app environment instance is configured to require more RAM than available on the current VM size, switching to a larger VM size might be required to have the app instance start at all. 	| Yes                             	|
| AKS Maximum Node Count 	| The number of available cluster nodes will be increased and decreased automatically based on the combined capacity requirement of all deployed Mendix apps. This setting controls the upper limit to the number of available nodes in order to avoid cost surprises.                                                                                                                                                                                                                                                                                       	| Yes                             	|
| AKS Service Tier       	| The [AKS service tier](https://learn.microsoft.com/en-us/azure/aks/free-standard-pricing-tiers) determines the service level Microsoft provides on the Mendix on Azure Kubernetes cluster control plane. This DOES NOT impact application performance, only Microsoft’s SLA.<br><br><br> <br>- Free should suffice in most situations.<br>- Standard can be considered by organizations that value a financially backed SLA (this has a cost impact, please check the Microsoft documentation)<br>- Premium does not offer any additional value in combination with Mendix on Azure and is discouraged                                	| Yes                             	|


### Database Settings

| Advanced Option          	| Description                                                                                                                                                                                                                                                                                           	| Editable after initial creation 	|
|--------------------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------	|
| Enable Read Replica      	| Enables a read replica for direct app database access. Please review the detailed documentation for instructions on how to access this read replica.                                                                                                                                                  	| Yes                             	|
| Compute Tier & Size      	| The DB Compute Tier applied to the shared PostgreSQL database instance used by all Mendix application environments in this cluster. This might need to be increased in case of application performance issues. Please consult Microsoft documentation for an [overview of the compute options](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-compute).         	| Yes                             	|
| Storage Performance Tier 	| The Storage Performance Tier used for the shared PostgreSQL database instance used by all Mendix application environments in this cluster. This might needs to be increased in case of application performance issues. Please consult Microsoft documentation for an [overview of the storage options](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-storage).	| Yes                             	|

### Observability Settings

| Advanced Option               	| Description                                                                                                                                                                                                                                    	| Editable after initial creation 	|
|-------------------------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------	|
| Managed Grafana Accessibility 	| Determines whether observability dashboard delivered via Managed Grafana will be accessible from the public internet or only via your private endpoints. Virtual network integration is required to access a private Managed Grafana instance. 	| Yes                             	|


## Self-service configuration available via Microsoft Azure Portal

The following confgiuration options are available to the customer directly via [Microsoft Azure portal](https://portal.azure.com). These options can be modified directly on the resources deployed in the Managed Resource Group belonging to the Mendix on Azure Managed Application:

| Configuration Option                                                                      	| Description                                                                                                   	|
|-------------------------------------------------------------------------------------------	|---------------------------------------------------------------------------------------------------------------	|
| Configure virtual network peering on the vNet hosting Mendix on Azure                     	| See [Configuring virtual network peering] for more information                                                	|
| Ovwride DNS configuration on the vNet hosting Mendix on Azure                             	| See [Configuring virtual network peering] for more information                                                	|
| Deploy Private Link Service to expose Mendix apps in other Azure virtual networks         	| See [using Private Link Services to expose Mendix apps in other Azure virtual networks] for more information  	|
| Deploy Private Endpoints to establish connectivity between Mendix apps and other services 	| See [accessing private services via Private Endpoints] for more information                                   	|

## Self-service configuration available via Mendix on Kubernetes Portal:

Next to a  large range of options for configuring individiual app environments, the [Mendix on Kubernetes Portal](https://privatecloud.mendixcloud.com) also provides several cluster-wide options that are applicable to Mendix on Azure environments:
 
#### Adding additional Cluster Managers

After the initial cluster initialization, the Mendix account that performed the initialization automatically receives the Cluster Manager role on the newly created cluster.

The Mendix on Kubernetes portal allows you to add additional cluster managers to your Mendix on Azure cluster.

Once added, additional cluster managers can view and manage the cluster through both the Mendix on Azure portal and the Mendix on Kubernetes portal, as long as they hold an Owner or Contributor role on the Azure Managed Application hosting the cluster. These cluster managers have the same level of access to adjust cluster configuration as the user who originally created the cluster.

In addition, they can access support tickets associated with the cluster via the Mendix on Azure portal and add additional comments. However, note that newly added cluster managers cannot access the full Zendesk ticket linked to the cluster’s support case.

{{% alert color="info" %}}
Before adding a cluster manager, ensure that the invited user signs in to the Mendix on Azure portal before accepting the invitation. If they have not signed in beforehand, the invitation may appear as accepted, but the user will not have access to any Mendix on Azure resources.
{{% /alert %}}

## Configuration assistance available by submitting a support ticket through the Mendix on Azure Portal

The following configuration changes can only be manually executed by Mendix on your behalf and should be requested via a Support Tickets submitted through Mendix on Azure portal:

| Configuration Change          	| Description                                                                                                                                                                                                                                                                                                                                                                                                                                       	|
|-------------------------------	|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| PostgreSQL Maintenance Window 	| You can configure a dedicated maintenance window for the PostgreSQL database instance that hosts your Mendix app databases. Since maintenance activities during these windows may result in temporary app unavailability, you have the option to request a change from the default system-managed schedule to a custom schedule. For more information, please refer to the [Microsoft Documentation on configuring PostgreSQL maintenance windows](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-maintenance). 	|

{{% alert color="info" %}} Please only submit Mendix on Azure support tickets through the Mendix on Azure portal. Tickets created via this portal automatically capture important contextual details, such as cluster identifiers and relevant logs, helping our support team address your issue swiftly and minimizing the likelihood of errors or delays {{% /alert %}}



