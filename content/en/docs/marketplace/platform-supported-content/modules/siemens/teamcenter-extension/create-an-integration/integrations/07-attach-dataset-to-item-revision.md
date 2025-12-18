---
title: "Attach Dataset to Item Revision"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/integrations/attach-dataset-to-item-revision/
description: "Provides step by step guide to use \"Attach Dataset to Item Revision\" integration in Teamcenter Extension."
weight: 7
---

## Introduction {#introduction}
The **Attach Dataset to Item Revision** integration allows you to generate the domain model and microflows to create and attach Teamcenter **Datasets** (or its specializations) containing file documents to an **Item Revision** in Teamcenter. For those not familiar with Teamcenter, a **Dataset** essentially is a container for files and related metadata. If you want to attach a document to a Teamcenter object, you do this using datasets. 

This document takes you through a use case where we want to attach a PDF document to an **Item Revision** with a **Specification** relation. 

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the **Settings tab** before following these instructions. For more instructions on how to configure your settings, follow the steps [here](/appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/#settings-tab). 
2. Click on the **Attach Dataset to Item Revision** button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/icon.png">}}
3. You will land on the [import mapping page](/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix.  
Click on one of the placeholder entities to start the import mapping.
 {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/import-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter **Dataset** objects (out of the box and custom) retrieved from the Teamcenter instance. For this use case, select **Dataset** 
The right side shows the relevant Mendix entities that can serve as input parameters for the microflow to attach datasets. In our case, we want to have an entity specifically for **Datasets**. Hence, we select `TcConnector.Dataset` and then click on the checkbox to **Create new Specialization of selected Entity**. The entity will automatically be named **Dataset** after the Teamcenter Object name, but it can be renamed here if required. Now click on **OK**, to finish the object mapping and close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/object-mapping.png">}}
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/object-mapping-result.png">}}
5. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected object. For our use case, we will not be adding any additional properties so you can close the panel by clicking on the backdrop.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/property-mapping.png">}}
6. Double click on the annotation **Attach any dataset** to open the configure panel. 
This panel lets you choose if you want to configure settings such as **Dataset type**, **File type** and **Relation name**, needed to upload **Datasets**, inside the Extension or have them provided as input parameters to the generated microflows.
Since we are particularly interested in building the logic to attach a PDF document to item revision, make the following selections by turning on the switches. 
   * Dataset Type &rarr; PDF
   * File Type &rarr; PDF_Reference (*.pdf)
   * Relation Name &rarr; Specifications
     
   {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/configure-dataset-attachment.png">}}
**Close** the panel.
7. Click on the **Generate** button to generate the appropriate domain model and microflows.
8. Once the generation is done, you will be redirected to the **History tab** which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/attach-dataset-to-item-revision/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* **Dataset** – This is an entity to represent a `Dataset` object from Teamcenter. This serves as an input to microflow that attaches datasets.  

### Microflows {#microflows}
Since we chose specific options in Step 6, the following microflow is generated.
* **Dataset_AttachDataset** – This microflow implements the logic to create and attach PDF dataset type with `PDF_Reference` file type and `Specification` relation to an **Item Revision**.

Depending on what options are chosen in the configure panel (step 7), additional microflows are generated.  
* **GetAvailableDatasetTypes** – This microflow implements the logic to retrieve all available dataset types.
* **GetFileTypesForDatasetTypes** – This microflow implements the logic to retrieve all available files types for a specific dataset type.
* As we configured the dataset type and file type in the extension, the extension did not generate these additional microflows for this use case. 