---
title: "Get Datasets for Item Revision"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/action-menu/get-datasets-for-item-revision/
description: "Provides step by step guide to use \"Get Datasets for Item Revision\" action in Teamcenter Extension."
weight: 6
---

## Introduction {#introduction}
The **Get Dataset for Item Revision** action allows you to generate domain model and microflow to retrieve **Datasets** for an **Item Revision** and subsequently download files present inside the dataset. For those not familiar with Teamcenter, a **Dataset** is a container for files and related metadata. They are typically attached to a business object such as **Item Revision**. 

This document takes you through a use case where we want to retrieve PDF, MS Word and Excel documents stored inside a dataset that is attached to an **Item Revision**.

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the **Settings tab** before following these instructions. For more instructions on how to configure your settings, follow the steps [here](/appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/#settings-tab).
2. Click on the **Get Dataset for Item Revision** button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/icon.png">}}
3. You will land on the [import mapping page](/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix.  
There is a parameter entity placeholder on the top, which lets you select the object from which the **Dataset** should be retrieved
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/import-mapping.png">}}
Click on the parameter entity placeholder to open the object mapping panel. The left side shows Teamcenter **Item Revision** objects (out of the box and custom) retrieved from the Teamcenter instance. Select the `Item Revision` object.
4. The Mendix entity on the right is selected by default. The entity will be used as an input parameter to get **Datasets** for any type of **Item Revision**.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/object-mapping.png">}}
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/object-mapping-result.png">}}
After the object mapping panel is closed, a sidebar opens that shows the set of **8 default relationships** selected by default. These are the 8 most commonly used relationships in Teamcenter from which datasets are retrieved. You can modify the selections but in this case, we will keep the 8 relationships selected by default. You can always come back to it by **double clicking** on the annotation.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/property-mapping.png">}}
5. Now select the entity placeholder (below the parameter entity placeholder) to open the object mapping panel. Select the **Dataset** object on the left. The right side shows the relevant Mendix entities that can be mapped to the **Dataset** object and will be used to create objects when a **Dataset** is retrieved for an **Item Revision**. In our case, we want to have an entity specifically for **Datasets**. Hence, we select `TcConnector.Dataset` and then click on the checkbox to C**reate new Specialization of selected Entity**. The entity will be automatically named `Dataset` after the Teamcenter Object name, but it can be renamed here if required. Now click on **OK**, to finish the object mapping and close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/object-mapping-association.png">}}
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/property-mapping-association.png">}}
6. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected object. For our use case, we will not be adding any additional properties so you can close the panel by clicking on the close button. 
7. Double click on the annotation **Retrieve all dataset types** to open the configure panel. 
8. This panel lets you choose if you want to retrieve certain dataset types only (such as PDF, MS Word, etc.).  
9. Since we are particularly interested in retrieving only documents (PDF, Word and Excel) attached to the item revision, we will select only the following **Dataset** types:
   * PDF
   * MS Word
   * MS Excel

   {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/object-mapping-association-result.png">}}
Close the panel
10. Click on the **Generate** button to generate the appropriate domain model and microflow.
11. Once the generation is done, you will be redirected to the **History tab** which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-datasets-for-item-revision/history.png">}}


## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* **Dataset** – This is an entity to represent a Dataset object from Teamcenter. This serves as an input to microflow that attaches datasets.  

### Microflows {#microflows}
The following microflows are generated 
* **GetDatasetForItemRevision** – This microflow implements the logic to retrieve specific datasets types (selected in step 8) from `Item Revision` based on the 8 relationships chosen from step 3. 
* **Dataset_RetrieveFilesorImages** – This microflow implements the logic to retrieve files or images stored within the dataset.