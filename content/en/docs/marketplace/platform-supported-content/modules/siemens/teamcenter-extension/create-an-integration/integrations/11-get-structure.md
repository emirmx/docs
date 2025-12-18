---
title: "Get Structure"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/integrations/get-structure/
description: "Provides step by step guide to use \"Get Structure\" integration in Teamcenter Extension."
weight: 11
---

## Introduction {#introduction}
The **Get Structure** integration allows you to generate the domain model and microflows to configure a Bill-of-Material (BOM) window and retrieve **BOM structures** from Teamcenter. A **BOM window** is what displays a **BOM structure** inside a Mendix application. 

This document takes you through a use case where we want create logic using the Teamcenter Extension to display a simple **BOM structure** with options to configure it according to our needs. 

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the **Settings tab** before following these instructions. For more instructions on how to configure your settings, follow the steps [here](/appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/#settings-tab).
2. Click on the **Get Structure** button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/icon.png">}}
3. When you click on this integration for the very first time, you’ll see a popup warning indicating that Mendix should be used to display small **BOM structures**. For larger **BOM structures**, it is recommended to use **Teamcenter Active Workspace**. Click **OK** to proceed further. This warning shows only the first time you click the integration.
4. You will land on the [import mapping page](/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix. You’ll also notice the **Configure structure** side bar opens by default. This gives you the ability to configure the **BOM structure** at design time or have the Extension generate microflows where the BOM configurations are input parameters.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/import-mapping.png">}}
   1. Turn on **Revision Rule**. This will generate a microflow to retrieve **Structure** with **Revision Rule** as the input parameter and a microflow to retrieve **Revision Rules**. In other words, this lets you build a feature in your app where you can give the end-users the ability to select a **Revision Rule** at run-time and configure the BOM with the selected **Revision Rule**.  

      If the option is turned off, the Extension will generate a microflow to retrieve **Structures** using the default revision rule (typically **LatestWorking**).

   2. Turn on **Variant rules** option. Just like with **Revision Rule**, this will generate a microflow to retrieve **Structures** with **Variant Rules** as an input parameter and a microflow to retrieve **Variant Rules**. Just like with **Revision Rule** option, this also lets you build a feature in your app where you can give the end users the ability to select a **Revision Rule** at run-time and configure the BOM with selected **Variant Rule**.  
 
      If the option is turned off, the extension will generate a microflow to retrieve structures without applying any variant rules.
 
   3. Turn on the **BOM window properties** flag. This will allow you to configure the **BOM window properties** (see below) at design time. The Extension will generate a microflow to retrieve **Structures** with the selected properties. 

      If the option is turned off, the Extension will generate a microflow to retrieve structures with `TcConnector.BOMWindowPropFlagMap` object as input parameter. This is a **HashMap** used in the Teamcenter Java SOA API when creating BOM windows and controls various display and behavior settings for BOM windows.  
For more details on the **BOM Window properties**, please refer to Teamcenter Documentation. 
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/import-mapping-2.png">}}
  
5. **Close** the configure structure panel.
6. In the import mapping page, click on `BOMLine` entity. This opens the Object Mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/import-mapping-empty.png">}}
7. In the object mapping dialog, the left side of the panel displays **BOMLine** and its sub-types. In Teamcenter, a **BOMLine** is a runtime object that represents a single entry/row in a product structure. The **BOM Lines** define the hierarcy of the structure and attached to each **BOMLine** is an **Item Revision**. The **BOMLine** is not a persistent item in the database. Instead, **BOMLines** are generated dynamically when a **BOMWindow** is created.
8. Select the parent **BOMLine** object.
9. The right side shows the relevant Mendix entities that can serve as input parameters to the microflow generated. Select the marketplace entity `TeamcenterToolkit.BOMLine` and click on the checkbox to **Create new Specialization of selected Entity**. The entity will be automatically named `BOMLine` after the Teamcenter Object name, but it can be renamed here if required. Now click on **OK**, to finish the object mapping and close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/object-mapping.png">}}
10. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected object. For our use case, we will not be adding any additional properties so you can close the panel by clicking on the **close** button.
11. Click on the other entity placeholder (**ItemRevision**) to re-open the object mapping dialog.
12. For our use case, the aim is to display **Item Revisions** in the **BOM tree**. Hence, select `ItemRevision` on the left and click on the checkbox to **Create a specialization of TcConnector.ItemRevision** on the right. Click **OK** to close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/object-mapping-2.png">}}
13. This will re-open the **Attributes and Associations** sidebar on the right. Select the following attributes
    * Last Saved Date (lsd)
    * Maturity
    
    **Close** the panel.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/object-mapping-result.png">}}
14. Click on the **Generate** button to generate the appropriate domain model and microflows.
15. Once the generation is done, you will be redirected to the **History tab** which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-structure/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* **BOMLine** – This is an entity to represent the BOMLine object in Teamcenter. As stated before, the BOMLines define the hierarchy in a structure. When navigating the BOM, the BOMLine serves as an input to microflow that expands the BOM structure
* **ItemRevision** – This is an entity to represent the Item Revision object in Teamcenter. The ItemRevision also serves as an input to microflows that create the BOM window and retrieve variant rules. 

### Microflows {#microflows}
The following microflows are generated.
* **ItemRevision_CreateBOMWindow** – This microflow implements the logic to create a BOM window with input item revision as its top line.
* **BOMWindow_GetTopBOMLine** – This microflow implements the logic to retrieve the top line or root of the BOM structure.
* **BOMLine_ExpandAllLevels_GetAll** – This microflow implements the logic to expand all levels of the BOM structure.
* **BOMLine_ExpandOneLevel_GetChildren** – This microflow implements the logic to expand just one level of the BOM structure.
* **BOMWindow_Close** – This microflow implements the logic to close the BOM Window.
* **GetRevisionRules** – This microflow implements the logic to retrieve revision rules from your Teamcenter instance.
* **ItemRevision_VariantRules** – This microflow implements the logic to retrieve variant rules from your Teamcenter instance. 