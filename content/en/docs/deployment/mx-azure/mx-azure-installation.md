---
title: "Installing and Configuring Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/installation/
description: "Documents the initial configuration tasks for Mendix on Azure."
weight: 20
---

{{% alert color="info" %}} This feature is currently available to participating customers. For more information, contact your Customer Success Manager. {{% /alert %}}

## Introduction

To get started with your Mendix on Azure deployment, you must first register your Microsoft Azure cloud cluster in the Mendix Portal. This will provide you with the resources required to deploy the Mendix Operator and host your Mendix app in an Azure deployment.

### Prerequisites

Before starting the installation and implementation process, make sure that you have all the necessary prerequisites:

* Obtain and configure a Microsoft Azure account. For more information, refer to the the Microsoft Azure documentation.
* Purchase the Mendix on Azure offering in the [Azure Marketplace](https://azuremarketplace.microsoft.com/).
* Enable co-access permissions for the offering.

{{< figure src="/attachments/deployment/mx-azure/coadmin-permission.png" class="no-border" >}}

* Familiarize yourself with the [Private Cloud](https://docs.mendix.com/developerportal/deploy/private-cloud/) concepts.
* Ensure that your Mendix Studio Pro is in version 10.10 or above.
* As an optional best practice, add multiple cluster manager to your clusters.

## Creating an Azure Cluster

To create a cluster for your Mendix on Azure app, perform the following steps:

1. In the Mendix Portal, in Private Cloud Cluster Manager, click **Mendix on Azure**.
2. Connect to your Azure account by clicking **Connect Azure Account**, and then logging in with the same account that you used to purchase the Mendix on Azure offering. If required, you can also purchase an Azure offering after you log in.

    After you successfully connect the accounts, the Mendix Portal shows a list of available clusters (that is, any Azure clusters that you have already linked with Mendix), initializable clusters (that is, any clusters that you have not yet linked with Mendix), and clusters that failed to initialize for any reason. For initialized clusters, means that the all the required resources are provisioned on the cluster. For uninitialized clusters, no resources are provisioned yet.

    {{< figure src="/attachments/deployment/mx-azure/available-clusters.png" class="no-border" >}}

3. In the Microsoft Azure portal, add a new managed Mendix on Azure application with **Standard** as the plan.

    {{< figure src="/attachments/deployment/mx-azure/create-managed-app.png" class="no-border" >}}

4. Provide a name for the resource group. The resource group contains all the resources that must be initialized for your Mendix deployment.

    {{< figure src="/attachments/deployment/mx-azure/resource-group-name.png" class="no-border" >}}

5. Follow the **Create** wizard to create the managed application.

6. After the resource deployment finishes, click **Go to resource**, and then click **Mendix on Azure Portal**.

    The managed app that you created is now visible as a new initializable cluster.

    {{< figure src="/attachments/deployment/mx-azure/initializable-clusters.png" class="no-border" >}}

7. In the **Actions** column, click the icon, and then select **Initialize**. 

    The preflight check launches to verify that the required resources can be registered in the cluster. Mendix apps are hosted with virtual images, so the preflight check determines whether the cluster contains the required type of virtual image. To view a list of the required resource providers, hover your cursor over the **Information** icon. If required, you can register any missing providers in the **Resource providers** section of the Microsoft Azure portal.

    {{< figure src="/attachments/deployment/mx-azure/preflight-check.png" class="no-border" >}}

8. After the preflight check completes, click **Next**.

9. In the **Provision** screen, review the information in the **Advanced Settings** section, and adjust any settings as needed. Note that selecting higher service tiers will incur higher costs. The **Postgress Compute SKU** and **Postgress Storage Performance Tier for IOPS** cannot currently be adjusted.

    {{% alert color="info" %}}If you plan to use [virtual network peering](#network-peering), you must set the **Load Balancer Type** to **Private (Internal)**.{{% /alert %}}

10. In the **Review & Initialize** screen, review the information and click **Initialize**.

    The initialization process takes approximately 15 minutes. It creates a resource group in the managed app that you created in step 3 above. Once the cluster is initialized successfully, a corresponding cluster is created in the the Private Cloud portal. The namespace is also created and configured automatically, as described in [Standard Operator: Running the Tool](https://docs.mendix.com/developerportal/deploy/standard-operator/#running-the-tool). You cannot create additional namespaces for a Mendix on Azure cluster. You also cannot use APIs to create or modify the cluster.
    
    The cluster cannot be deleted from the Private Cloud portal or the Mendix on Azure portal. If you want to remove it, you must delete it in the Microsoft Azure portal.

## Rerunning Failed Clusters

If a cluster fails for any reason, its status in the Mendix Portal changes to **Failed**. To view more information about the issue, click the icon in the **Actions** column, and then select **Details**.

{{< figure src="/attachments/deployment/mx-azure/failed-cluster.png" class="no-border" >}}

To fix the issue, you can click **Rerun** to manually re-run the cluster. If a cluster still fails after a manual rerun, a support ticket is automatically opened with Mendix Support. For more information, see [Support Policy for Mendix on Azure: Automatic Support Tickets](/developerportal/deploy/mendix-on-azure/support/#tickets-automatic).

{{% alert color="warning" %}}Do not initialize a cluster while it is in a Failed status.{{% /alert %}}
    
## Editing the Cluster in the Mendix on Azure Portal

If required, you can change the following options for your cluster:

* AKS service tier
* AKS node size
* VM type
* Load balancer type
* Postgres compute SKU
* Postgres performance tier for storage IOPS
* Custom tags

## Enabling Connections Between Different Azure Resource Groups

If your Mendix managed app is in a different Azure resource group than the user machines which must connect to it, you may need to perform additional steps to enable connections between these resource groups.

### Example Situation

The following diagram shows two managed resource groups. One of them contains the Mendix managed app, and the other - a user machine that must access Mendix, along with a backend virtual machine that the Mendix app must access. Connections between the two resource groups are not enabled, resulting in access issues.

{{< figure src="/attachments/deployment/mx-azure/separate-access-groups.png" class="no-border" >}}

#### Potential Solution 1: Virtual Network Peering {#network-peering}

The following diagram shows one potential solution to the access issue. Bi-directional [virtual network peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) has been configured between the two resource groups.

{{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings.png" class="no-border" >}}

To enable virtual network peering for your Mendix on Azure app, perform the following steps:

1. In the Microsoft Azure portal, add a new bi-directional virtual peering.

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-add.png" class="no-border" >}}

2. Create an [Azure Private DNS zone](https://learn.microsoft.com/en-us/azure/dns/private-dns-privatednszone).
3. In the **Instance details** section, in the **Name** field, enter the domain of your Mendix app, for example, *azure.mendixapps.io*.

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-name.png" class="no-border" >}}

4. Create a [DNS record](https://learn.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal) for the Mendix application.
5. In the **Name** field, enter the name of our Mendix app, for example, *myapp*.
6. In the **IP** field, enter the IP address of the internal load balancer.
7. Create a [virtual network link](https://learn.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links) and link it with your custom virtual network.

    Users in the other virtual network can now connect to your Mendix app.

8. If you want to enable connections from your Mendix app to a virtual back-end machine in the other network, perform the following additional steps:
    
    1. In the Azure portal, create another private DNS zone for the virtual machine, with auto-registration enabled.
    2. In the Mendix portal, in the **Environment Details** page, go to [Model Options](/developerportal/deploy/environments-details/#model-options).
    3. In the [Constants](/developerportal/deploy/environments-details/#constants) section, find and edit the **RestClient.RestServiceUrl** constant.
    4. In the **New value** field, enter the URL and port of your back-end machine, and then click **Save and Apply**.
    5. In the Azure portal, configure the virtual network link to link the private DNS zone with the virtual network of your managed Mendix application.

    Your Mendix app can now connect to a back-end server in the other virtual network.

#### Potential Solution 2: Private Endpoints

Another possible solution can be achieved by using [private endpoints](https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-overview). In the following diagram, a private endpoint has been added to each resource group. The private endpoint connects to a private link in the other resource group, which in turn connects to an internal load balancer.

{{< figure src="/attachments/deployment/mx-azure/private-endpoints.png" class="no-border" >}}

To enable private endpoints for your Mendix on Azure app, perform the following steps:

1. In the Microsoft Azure portal, add a new [private link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview) for the Mendix AKS load balancer.

    {{< figure src="/attachments/deployment/mx-azure/private-link.png" class="no-border" >}}

2. Create a private endpoint in your custom resource group.
3. In the **Resource** tab, specify the following settings:

    * **Resource type** - **privateLinkServices**
    * **Resource** - the private link that you created in step 1 above

4. In the **Virtual Network** tab, specify the following settings:

    * **Virtual network** - the virtual network where the user's machine is located

5. Configure other settings as needed.
6. Create an [Azure Private DNS zone](https://learn.microsoft.com/en-us/azure/dns/private-dns-privatednszone).
7. In the **Instance details** section, in the **Name** field, enter the domain of your Mendix app, for example, *azure.mendixapps.io*.

    {{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings-name.png" class="no-border" >}}

8. Create a [DNS record](https://learn.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal) for the Mendix application.
9. In the **Name** field, enter the name of our Mendix app, for example, *myapp*.
10. In the **IP** field, enter the IP address of the private endpoint.
11. Create a [virtual network link](https://learn.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links) and link it with your Mendix managed virtual network.

    Users in the other virtual network can now connect to your Mendix app.

12. If you want to enable connections from your Mendix app to a virtual back-end machine in the other network, perform the following additional steps:

    1. Create a load balancer that points to the back-end machine.
    2. Add a new [private link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview) for the back-end load balancer.

        Make sure to select the back-end load balancer in the **Load balancer** field of the **Outbound settings** tab.

    3. Configure other settings as needed.
    4. Create a private endpoint in your Mendix managed resource group.
    5. In the **Resource** tab, specify the following settings:

        * **Resource type** - **privateLinkServices**
        * **Resource** - the private link that you created in step 12-b above.

    6. In the **Virtual Network** tab, specify the following settings:

        * **Virtual network** - the virtual network managed by Mendix

    7. Create a [DNS record](https://learn.microsoft.com/en-us/azure/dns/dns-operations-recordsets-portal) for the IP address of the back-end service's private endpoint.
    8. In the Mendix portal, in the **Environment Details** page, go to [Model Options](/developerportal/deploy/environments-details/#model-options).
    9. In the [Constants](/developerportal/deploy/environments-details/#constants) section, find and edit the **RestClient.RestServiceUrl** constant.
    10. In the **New value** field, enter the URL and port of your back-end machine, and then click **Save and Apply**.

    Your Mendix app can now connect to a back-end server in the other virtual network.

## Deploying an App to an Azure Cluster

After creating your cluster in Microsoft Azure, you can deploy now deploy your applications to the cluster. The deployment process is the same as with Mendix for Private Cloud. For more information, see [Deploying a Mendix App to a Private Cloud Cluster](/developerportal/deploy/private-cloud-deploy/).
