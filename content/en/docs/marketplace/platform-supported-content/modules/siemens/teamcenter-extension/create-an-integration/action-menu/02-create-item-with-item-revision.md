---
title: "Create Item with Item Revision"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/action-menu/create-item-with-item-revision/
description: "Provides step by step guide to use \"Create Item with Item Revision\" action in Teamcenter Extension."
weight: 2
---

## Introduction {#introduction}
The **Create Item with Item Revision** action allows you to configure and generate the domain model and microflow to create an **Item** with **ItemRevision** or its specializations in Teamcenter. The resulting microflow implements the **Create Object** and U**pdate Properties** actions from the Teamcenter Connector.  

This document takes you through a use-case where you want to create **Problem Reports** in Teamcenter. **Problem Reports**, represented by the **Problem Report Revision** business object, are utilized to manage the documentation or reproduction of an issue. They can also be used to determine the severity and priority of the issue. 

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the **Settings tab** before following these instructions. For more instructions on how to configure your settings, follow the steps [here](/appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/#settings-tab). 
2. Click on the **Create Item w/ Item Revision** button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/icon.png">}}
3. You will land on the [import mapping page](/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix.
When creating a **Problem Report** object in Teamcenter, two objects are created: **Problem Report** (sub-class of **Item**) and its associated **Problem Report Revision** (sub-class of **Item Revision**). Hence, in the import mapping page, we have placeholders to map both the objects with Mendix entities.
Click on one of the top placeholder entities to start the import mapping.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/import-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter objects (both out of the box and custom) retrieved from the Teamcenter instance. Since we are interested in creating **Problem Reports**, we select `Problem Report` found under **Item -> Change Item -> Generic Problem Report**.
The right side show the Mendix entities that can serve as input parameters for the microflow to create **Item** with **Item revision**. Notably, if we create our own entity here, we can ensure that additional attributes are set in Teamcenter upon creation. Hence, we select `TcConnector.Item` and then click on the checkbox to **Create new Specialization of selected Entity**. The entity will automatically be named `ProblemReport` after the Teamcenter Object name, but it can be renamed here if required. Now click on **OK**, to finish the object mapping and close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/object-mapping.png">}}
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/object-mapping-result.png">}}
5. Since we also want to create **Problem Report Revision**, repeat this process to select `Problem Report Revision` with another specialized entity.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/property-mapping.png">}}
6. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected Teamcenter Object the properties you want to have within your Mendix application. In this example, we want to create an integration where we retrieve additional **Problem Report Revision** details. Select in the side panel the following attribute to add to the Problem Report Revision entity:
   * Severity Rating

   After this selection, you can click on the backdrop or on the close button to close the side panel.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/attribute-mapping.png">}}
7. Click on the **Generate** button to generate the appropriate domain model and microflows.
8. Once the generation is done, you will be redirected to the **History tab** which shows a summary of what has been generated.
   {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/create-item-with-item-revision/history.png">}}
   
## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* **ProblemReport** – This is an entity representing a Problem Report from Teamcenter. It can be used to create a new Item in Teamcenter. 
* **ProblemReportRevision** – This is an entity representing Problem Report Revision from Teamcenter. It can be used to create a new Item Revision in Teamcenter.
* **ProblemReportCreateInput** – This is a helper entity that is used in the `ProblemReport_CreateItemAndItemRevision` microflow to Create the Item in Teamcenter.
* **ProblemReportRevisionCompoundCreateInput** – This is a helper entity that is used in the `ProblemReport_CreateItemAndItemRevision` microflow to Create the `ItemRevision` in Teamcenter. 
### Microflows {#microflows}
The extension generated the following microflows in your project. 
* **ProblemReport_CreateItemAndItemRevision** – This microflow implements the logic to create an item w/ item revision with only the necessary properties and then updates the objects with additional properties.
* **ProblemReportRevision_RetrieveProblemReport** – This microflow implements the logic to retrieve problem reports revision. It’s primarily available if you are interested in retrieving problem report revisions that were created.