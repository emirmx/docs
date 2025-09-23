---
title: "Read Replica for Postgres Database"
url: /developerportal/deploy/mendix-on-azure/read-replica-database-access/
description: "Provides details about the read replica for Postgres database."
weight: 30
---

{{% alert color="info" %}} This feature is currently available to participating customers. For more information, contact your Customer Success Manager. {{% /alert %}}

## Introduction

This document describes the documentation on how to enable read replica for Postgres database and examples on how to read data from the read replica database.

What is Read Replica of Database?

Read replicas are copies of your database that are kept in sync with the primary database. They are used to serve read-only queries and can be used to offload read traffic from the primary database.

With the above set up:
 1. Use the read replica to serve all the read-only queries
 2. Keep the main DB focused on write operations

{{% alert color="info" %}}Before you begin, familiarize yourself with some concepts on read replica for Postgres database and vnet peering.{{% /alert %}}

## Enable Read Replica in the Mendix on Azure Portal

{{% alert color="info" %}}By default, the read replica for Postgres database is disabled.{{% /alert %}}

1. Under the Provision section of Initialize Cluster page, under **Database Settings** select **Yes** for **Enable Read Replica** option. Click **Next**. and initialize the cluster.

{{< figure src="/attachments/deployment/mx-azure/enableReadReplica.png" class="no-border" >}}

 {{% alert color="info" %}}If you plan to use read replica, only **General Purpose** or **Memory Optimized** compute tier settings are supported for Postgres db. {{% /alert %}}

2. Once the cluster is initialized, the read replica for Postgres database is enabled and a read replica for the postgres database is created in the managed cluster. 

{{< figure src="/attachments/deployment/mx-azure/readReplicaEnabled.png" class="no-border" >}}

3. After successful cluster initialization, copy the address value from the record set within the Private DNS zone created for your Postgres database.

{{< figure src="/attachments/deployment/mx-azure/copyAddressValue.png" class="no-border" >}}

4. Add the user who wanted to access the replica database by going to the replica database created under managed app. Go to Security -> Authentication and add the user.

{{< figure src="/attachments/deployment/mx-azure/adduser.png" class="no-border" >}}  


5. If your Mendix managed app is in a different Azure resource group than the user machines which must connect to it, you may need to perform additional steps to enable connections between these resource groups.One of the potential solution to access the read replica is using the vnet peering. This allows to connect two virtual networks (VNets) so resources can talk to each other using private IPs. 

The following diagram shows one potential solution to the access issue. Bi-directional [virtual network peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) has been configured between the two resource groups.

{{< figure src="/attachments/deployment/mx-azure/vnetpeeringreadReplicaEnabled.png" class="no-border" >}}

To enable virtual network peering for your Mendix on Azure app, perform the following steps:

    5.1. In the Microsoft Azure portal, [add a new bi-directional virtual peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal) in the resource group where your Mendix app is deployed.

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-add.png" class="no-border" >}}

    5.2. Create a [Azure Private DNS zone](https://learn.microsoft.com/en-us/azure/dns/private-dns-privatednszone) in another resource group from where you need to connect to the replica database.  Private DNS zone resolves domain names in a managed app to a private IP addresses.

    5.3. In the **Instance details** section, in the **Name** field, enter the domain of your Mendix app, for example, *azure.mendixapps.io*. 

    5.4. Create a [DNS record](https://learn.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal) for the Mendix application. Record set maps a hostname to a private IP.

    5.4.1. In the **Name** field, enter the name of our Mendix app, for example, *myapp*.

    5.4.2. In the **IP** field, enter the IP address of the read replica. Check the value of the record set in Step 3

    5.5. Create a [virtual network link](https://learn.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links) to connect the Postgres database's Private DNS zone with your custom virtual network. This enables seamless name resolution within your VNet.

    Users in the other virtual network can now connect to your Mendix app.
