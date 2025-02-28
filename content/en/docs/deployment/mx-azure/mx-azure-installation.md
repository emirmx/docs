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
* Familiarize yourself with the [Private Cloud](https://docs.mendix.com/developerportal/deploy/private-cloud/) concepts.
* Ensure that your Mendix Studio Pro is in version 10.10 or newer.
* As an optional best practice, add multiple cluster manager to your clusters.

## Creating an Azure Cluster

To create a cluster for your Mendix on Azure app, perform the following steps:

1. In the Mendix Portal, in Private Cloud Cluster Manager, click **Mendix on Azure**.
2. Connect to your Azure account by clicking **Connect and Initialize**, and then logging in with the same account that you used to purchase the Mendix on Azure offering.

    After you successfully connect the accounts, the Mendix Portal shows a list of available clusters (that is, any Azure clusters that you have already linked with Mendix) and initializable clusters (that is, any clusters that you have not yet linked with Mendix). For initialized clusters, means that the all the required resources are provisioned on the cluster. For uninitialized clusters, no resources are provisioned yet.

    {{< figure src="/attachments/deployment/mx-azure/available-clusters.png" class="no-border" >}}

3. In the Microsoft Azure portal, add a new managed Mendix on Azure application with **Standard** as the plan.

    {{< figure src="/attachments/deployment/mx-azure/create-managed-app.png" class="no-border" >}}

4. Provide a name for the resource group. The resource group contains all the resources that must be initialized for your Mendix deployment.

    {{< figure src="/attachments/deployment/mx-azure/resource-group-name.png" class="no-border" >}}

5. Follow the **Create** wizard to create the managed application.

6. After the resource deployment finishes, click **Go to resource**, and then click **Mendix on Azure Portal**.

    The managed app that you created is now visible as a new initializable cluster.

    {{< figure src="/attachments/deployment/mx-azure/initializable-clusters.png" class="no-border" >}}   

7. Click **Initialize**. 

    The preflight check launches to verify that the required resources can be registered in the cluster. Mendix apps are hosted with virtual images, so the preflight check determines whether the cluster contains the required type of virtual image. To view a list of the required resource providers, hover your cursor over the **Information** icon. If required, you can register any missing providers in the **Resource providers** section of the Microsoft Azure portal.

8. After the preflight check completes, click **Next**.

9. Select the **AKS Service Tier**.

    You can choose any tier that suits your requirements. Higher tiers will incur higher costs.

10. Click **Initialize**.

    The initialization process takes ca. 15 minutes. It creates a resource group in the managed app that you created in step 3 above. Once the cluster is initialized successfully, a corresponding cluster and namespace are created in the the Private Cloud portal. The namespace is also configured automatically, as described in [Standard Operator: Running the Tool](https://docs.mendix.com/developerportal/deploy/standard-operator/#running-the-tool). The cluster cannot be deleted from the Private Cloud portal. If you want to remove it, you must delete it in the Microsoft Azure portal.

## Deploying an App to an Azure Cluster

After creating your cluster in Microsoft Azure, you can deploy now deploy your applications to the cluster. The deployment process is the same as with Mendix for Private Cloud. For more information, see [Deploying a Mendix App to a Private Cloud Cluster](/developerportal/deploy/private-cloud-deploy/).