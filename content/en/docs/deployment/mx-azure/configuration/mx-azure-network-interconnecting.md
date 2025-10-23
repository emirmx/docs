---
title: "Interconnecting"
url: /developerportal/deploy/mendix-on-azure/advanced-configuration/interconnecting-networks
description: "Interconnecting Mendix on Azure with private networks"
weight: 10
---
## Enabling Connections Between Different Azure Resource Groups

If your Mendix managed app is in a different Azure resource group than the user machines which must connect to it, you may need to perform additional steps to enable connections between these resource groups.

### Example Situation

The following diagram shows two managed resource groups. One of them contains the Mendix managed app, and the other - a user machine that must access Mendix, along with a backend virtual machine that the Mendix app must access. Connections between the two resource groups are not enabled, resulting in access issues.

{{< figure src="/attachments/deployment/mx-azure/separate-access-groups.png" class="no-border" >}}

#### Potential Solution 1: Virtual Network Peering {#network-peering}

The following diagram shows one potential solution to the access issue. Bi-directional [virtual network peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) has been configured between the two resource groups.

{{< figure src="/attachments/deployment/mx-azure/virtual-network-peerings.png" class="no-border" >}}

To enable virtual network peering for your Mendix on Azure app, perform the following steps:

1. In the Microsoft Azure portal, [add a new bi-directional virtual peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal).

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

