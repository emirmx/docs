---
title: "Enabling connectivity between Mendix on Azure and other private networks"
url: /developerportal/deploy/mendix-on-azure/configuration/interconnecting-networks
description: "Interconnecting Mendix on Azure with private networks"
weight: 10
---
## Introduction

Mendix on Azure supports various use cases where connectivity to other private Azure networks is required. This section explains those use cases, outlines supported connectivity methods, and provides detailed guidance on establishing such network connectivity.

## Use-cases that require private connectivity

Typical use cases depending on private connectivity between Mendix on Azure and other private networks include:

- Ensuring Mendix apps are exposed only to internal private networks for security and compliance, allowing access only to authorized personnel.
- Integrating Mendix applications with services available solely within your organization's private network for security and compliance reasons.
- Establishing direct read-only database access to the databases supporting your Mendix applications, for example in case your company policy mandates secure ingestion of all corporate data into a central Data Lake.


## Supported private connectivity methods

During cluster initialization, Mendix on Azure automatically deploys into a new Azure Virtual Network. Resources on this network can connect with other resources via two native Azure methods:

1. [Azure Virtual Network Peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)
2. [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview) with [Azure Private Endpoints](https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-overview)

{{% alert color="info" %}} Mendix on Azure always deploys a dedicated Azure virtual network during cluster initialization. Deploying onto an existing Azure virtual network is currently not supported. {{% /alert %}}

### Pros and cons of vNet peering and Private Link with Private Endpoints

The table below compares the possibilities and constraints of both supported methods. More details are available in the Microsoft documentation linked above.

| Aspect               	| Azure VNet Peering              	| Azure Private Link with Azure Private Endpoints 	|
|----------------------	|---------------------------------	|-------------------------------------------------	|
| Primary Use          	| Connect entire virtual networks 	| Secure access to specific services              	|
| Security Level       	| High (exposes entire vNet)        | Highest (exposes only a single endpoint)          |
| Overlapping IP space 	| Not supported                   	| Supported (exposes one endpoint using NAT) 	    |
| Supported use-cases  	| All use-cases                   	| All except direct database access               	|

## Implementing supported private connectivity methods

The diagram below shows a scenario where a secondary Azure virtual network co-exists alongside the virtual network hosting a customer's Mendix on Azure environment. The next two sections provide step-by-step instructions for interconnecting these networks using Azure Virtual Network Peering and Azure Private Link with Private Endpoints.

{{< figure src="/attachments/deployment/mx-azure/separate-access-groups.png" class="no-border" >}}

### Solution 1: Azure Virtual Network Peering {#network-peering}

The following diagram illustrates bi-directional [virtual network peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) connecting the Mendix on Azure virtual network with another virtual network.

{{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings.png" class="no-border" >}}

To enable virtual network peering between your Mendix on Azure virtual network and another virtual network:

1. In the Microsoft Azure portal, [set up a new bi-directional virtual peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal#create-a-virtual-network-peering) following Microsoft’s instructions. Locate the Mendix on Azure virtual network inside the [Managed Resource Group of your Mendix on Azure environment](../_index.md#the-mendix-on-azure-managed-resource-group-mrg).

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-add.png" class="no-border" >}}

2. Adjust routing tables and/or network security groups as needed to allow traffic between the Mendix on Azure cluster virtual network and your other networks.

#### DNS name resolution towards Mendix app cluster

Virtual network peering enables IP traffic flow but DNS resolution must also be configured so users in other networks can reach Mendix apps by name. For apps using the default URL format (xxx.azure.mendixapps.io), do the following:

1. Create an [Azure Private DNS zone](https://learn.microsoft.com/en-us/azure/dns/private-dns-privatednszone).
2. In the **Instance details**, enter the domain suffix of your Mendix app, e.g., *azure.mendixapps.io*.

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-name.png" class="no-border" >}}

3. Create a [DNS record](https://learn.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal) for your Mendix app:
   - **Name**: The app name (e.g., *myapp*), or use a wildcard (*) for automatic resolution of new apps.
   - **IP**: The IP address of the internal load balancer, found on the Load Balancer resource in the [Managed Resource Group](../_index.md#the-mendix-on-azure-managed-resource-group-mrg).
4. Create a [virtual network link](https://learn.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links) to link this DNS zone to the virtual network(s) hosting the users who need access.

Users in linked networks can now access Mendix apps using the usual URLs.

#### DNS name resolution towards resources in other networks {#name-resolution-dns-override}

To allow Mendix apps to resolve internal services in your network, configure DNS resolution by either:

1. Creating or using an existing Private DNS Zone for the internal service's FQDN and link it to the Mendix virtual network via a [virtual network link](https://learn.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links); or
2. Configuring the Mendix virtual network to use your own DNS server that resolves internal service names, following [Microsoft's DNS configuration instructions](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances). The Mendix virtual network is located in the [Managed Resource Group](../_index.md#the-mendix-on-azure-managed-resource-group-mrg).

### Solution 2: PrivateLink with Private Endpoints

Alternatively, connectivity can be established via [Azure Private Endpoints](https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-overview). The diagram below shows private endpoints added to each virtual network, enabling direct connections to Mendix apps or services in other networks.

{{< figure src="/attachments/deployment/mx-azure/private-endpoints.png" class="no-border" >}}

#### Creating a private endpoint to Mendix apps {#pls}

To create a Private Endpoint for accessing Mendix on Azure apps from another virtual network:

1. In the Azure portal, configure a new [Azure Private Link service](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview) targeting the Mendix AKS cluster load balancer in the [Managed Resource Group](../_index.md#the-mendix-on-azure-managed-resource-group-mrg). Follow the [Microsoft instructions](https://learn.microsoft.com/en-us/azure/private-link/create-private-link-service-portal?tabs=dynamic-ip#create-a-private-link-service).

    {{< figure src="/attachments/deployment/mx-azure/private-link.png" class="no-border" >}}

2. Create an Azure Private Endpoint in a virtual network accessible to your users:
3. Under the **Resource** tab, specify:
   - **Resource type**: **privateLinkServices**
   - **Resource**: The private link service created in step 1
4. Under the **Virtual Network** tab, specify:
   - **Virtual network**: The network reachable by your users
5. Configure additional settings as needed.
6. Create an [Azure Private DNS zone](https://learn.microsoft.com/en-us/azure/dns/private-dns-privatednszone) and enter the Mendix app domain suffix, e.g., *azure.mendixapps.io*, under **Instance details**.

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-name.png" class="no-border" >}}

7. Create a [DNS record](https://learn.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal) for your Mendix app:
   - **Name**: The app name (e.g., *myapp*), or use a wildcard (*) for automatic resolution of new apps.
   - **IP**: The private endpoint IP address.
8. Link the DNS zone with the virtual network(s) of users via a [virtual network link](https://learn.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links).

Users in these networks can now access Mendix apps via the standard URLs.

#### Creating a private endpoint to internal services {#pe-internal}

To enable Mendix apps to connect to internal services in another virtual network:

1. Ensure the internal service is exposed through an Azure Private Link service following Microsoft’s guidance.
2. Create an Azure Private Endpoint in the [Managed Resource Group](../_index.md#the-mendix-on-azure-managed-resource-group-mrg).
3. On the **Resource** tab, specify:
   - **Resource type**: **privateLinkServices**
   - **Resource**: The private link service exposing the internal service.
4. On the **Virtual Network** tab, specify:
   - **Virtual network**: The Mendix-managed virtual network.
5. Configure DNS resolution by creating a Private DNS zone pointing the service's FQDN to this Private Endpoint. Link this DNS zone to the Mendix virtual network using a virtual network link as detailed in the [Managed Resource Group](../_index.md#the-mendix-on-azure-managed-resource-group-mrg).