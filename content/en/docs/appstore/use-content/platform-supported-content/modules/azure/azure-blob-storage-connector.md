---
title: "Azure Blob Storage"
url: /appstore/modules/azure/azure-blob-storage/
description: "Describes the configuration and usage of the Azure Blob Storage connector, which is available in the Mendix Marketplace. Azure Blob Storage is an object storage service offering industry-leading scalability, data availability, security, and performance."
weight: 20
aliases:
    - /appstore/connectors/azure-blob-storage-connector/
    - /appstore/connectors/azure/azure-blob-storage/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## Introduction

The [Azure Blob Storage](https://marketplace.mendix.com/link/component/<insert when published>) connector enables you to connect your app to [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs) and easily store objects.

### Typical Use Cases

The Azure Blob Storage service is an object storage service offering industry-leading scalability, data availability, security, and performance. Customers of all sizes and industries can store and protect any amount of data for virtually any use case. With cost-effective storage types and easy-to-use management features, you can optimize costs, organize data, and configure fine-tuned access controls to meet specific business, organizational, and compliance requirements. Some typical use cases of Azure Blob Storage are:

* Back up and restore critical data - Meet Recovery Time Objectives (RTO), Recovery Point Objectives (RPO), and compliance requirements with Bolb storage's robust replication features.
* Archive data at the lowest cost - Move data archives to the Azure Blob Storage to eliminate operational complexities, and gain new insights.

### Prerequisites {#prerequisites}

The Azure Blob Storage Connector requires Mendix Studio Pro version 9.24.2 or above.

### Licensing and Cost

This connector is available as a free download from the Mendix Marketplace, but the Azure service to which it connects may incur a usage cost. For more information, refer to Azure documentation.

Depending on your use case, your deployment environment, and the type of app that you want to build, you may also need a license for your Mendix app. For more information, refer to [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/).

## Installation

Follow the instructions in [How to Use Marketplace Content](/appstore/use-content/) to import the Azure Blob Storage connector into your app.

## Configuration

After you install the connector, you can find it in the **App Explorer**, in the **AzureBlobStorageConnector** section. The connector provides a domain model and operations that you can use to connect your app to Azure Blob Storage. Each operation can be implemented using it in a microflow or nanoflow.

### Configuring Authentication {#authentication}

In order to use the Azure Blob Storage service, you must authenticate using a Shared Access Signature (SAS) or an Azure Entra ID Access Token.

#### SAS authorization
You or your admin needs to create a SAS for the container or blob you want to perform operations on. This SAS should then be added to a `SASCredentials` object on the `SASToken` attribute. Feed the `SASCredentials` object to the `AbstractCredentials` input parameter of the operation microflow you want to use.

#### Azure Entra ID Access Token
Set up SSO using the OIDC SSO marketplace module. When this is set up for your application you can use the `GetCurrentToken` microflow to get the access token needed for authenticationg the call. Create an `EntraCredentials` object and add the access token to the `BearerToken` attribute. Feed the `EntraCredentials` object to the `AbstractCredentials` input parameter of the operation microflow you want to use.

### Configuring a Microflow for an AWS Service

You can implement the operations of the connector by using them in microflows. For example, to upload a Blob to the Azure Blob Storage, implement the **PUT_v1_Azure_PutBlob** operation by performing the following steps:

1. In the **App Explorer**, right-click on the name of your module, and then click **Add microflow**.
2. Enter a name for your microflow, for example, *ACT_PutBlob*, and then click **OK**.
3. In the **App Explorer**, in the **AzureBlobStorageConnector** section, find the **PUT_v1_Azure_PutBlob** operation microflow.
4. Create a **SASCredentials** or **EntrCredentials** object and add the SAS or access token to the **SASToken** or **BearerToken** attributes respectively. 
5. Drag the **PUT_v1_Azure_PutBlob** microflow in to your microflow.
6. Double-click the **PUT_v1_Azure_PutBlob** operation to configure the required parameters. 
    
    For the `PUT_v1_Azure_PutBlob` operation, retrieve the `System.FileDocument` you want to store and provide a configured `SASCredentials` or `EntrCredentials` object. You must then create a `PutBlobRequest` object in your microflow as the last parameter. This entity requires the following parameters:

    * `BlobName` - The BlobName attribute holds the name the blob will get in the Blob storage.
    * `ContainerName` - The ContainerName attribute holds the target container name where the blob will be stored.
    * `BlobType` - The BlobType attribute holds the type of blob that will be created. For now we only support the BlockBlob option.

    The following parameters are optional:
    * `ContentType` - Optional. The ContentType attribute can be used to specify the MIME content type of the blob. The default type is application/octet-stream.
    * `StorageType` - Optional. The StorageType attribute specifies the storage tier to be set on the blob. For page blobs on a Premium Storage account only with version 2017-04-17 and later. For a full list of page blob-supported tiers, see https://learn.microsoft.com/en-us/azure/virtual-machines/disks-types#premium-ssd. For block blobs, supported on blob storage or general purpose v2 accounts only with version 2018-11-09 and later. Valid values for block blob tiers are Hot, Cool, Cold, and Archive. Note: Cold tier is supported for version 2021-12-02 and later. For detailed information about block blob tiering, see https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview.
    
9. Configure a method to trigger the `ACT_PutBlob` microflow. 
    For example, you can call the microflow with a custom button on a page in your app. For an example of how this can be implemented, see [Creating a Custom Save Button with a Microflow](/refguide/creating-a-custom-save-button/).
    
## Technical Reference {#technical-reference}

The module includes technical reference documentation for the available entities, enumerations, activities, and other items that you can use in your application. You can view the information about each object in context by using the **Documentation** pane in Studio Pro.

The **Documentation** pane displays the documentation for the currently selected element. To view it, perform the following steps:

1. In the [View menu](/refguide/view-menu/) of Studio Pro, select **Documentation**.
2. Click on the element for which you want to view the documentation.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/technical-reference/doc-pane.png" class="no-border" >}}

For additional reference, the available activities are listed below.

### Operations

[Operations](/refguide/Operations/) define the operations that are executed in a microflow or a nanoflow.

The Azure Blob Storage connector contains the following activities:

* `PutBlob` - Allows you to upload, as a Blob, a file of any type, to Azure Blob Storage. For more information, see [Put Blob to Azure Blob Storage](https://learn.microsoft.com/en-us/rest/api/storageservices/put-blob).
