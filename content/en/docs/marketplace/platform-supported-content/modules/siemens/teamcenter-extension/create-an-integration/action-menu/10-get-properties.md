---
title: "Get Properties"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/action-menu/get-properties/
description: "Provides step by step guide to use \"Get Properties\" action in Teamcenter Extension."
weight: 10
---

## Introduction {#introduction}
The “Get Properties” action allows you to generate a microflow to retrieve additional properties of a Teamcenter workspace object or specializations thereof and the corresponding domain model. The resulting microflow implements the Get Properties action from Teamcenter Connector 

This document takes you through a use case where we want to retrieve additional properties of a Part Revision object. Part Revisions are specific iterations of a part of a product that is managed using Teamcenter. A Part Revision contains essential information like the schematics of the Part. As a Part Revision is a sub-type of an Item Revision, it can be revised. 

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the settings tab before following these instructions. For more instructions on how to configure your settings, follow the steps [here].
2. Click on the Get Properties button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-properties/get-properties.png">}}
3. You will land on the [import mapping page](https://docs.mendix.com/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix. You’ll also notice the “Get Properties Configuration” side bar opens by default.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-properties/import-mapping.png">}}
4. By default, this option is turned on. This means it will generate a microflow to create and populate Mendix Model Objects for a given list of Teamcenter product `UIDs`. If the option is turned off, the extension will generate a microflow to update and overwrite a list of existing Mendix Model Objects based on the selections made in the import mapping page.
5. Keep the option turned on and close the configuration panel
6. Click on one of the placeholder entities to open the object mapping panel. The left side of the panel displays workspace objects and its sub-types (out of the box and custom). Since we are interested in retrieving Part Revision object properties, we select the `Part Revision` object.
7. The right side shows the relevant Mendix entities that can serve as input parameters for the microflow to retrieve additional properties. In our case, we want to have an entity specifically for `Part Revision`. Hence, we select `TcConnector.WorkspaceObject` and then click on the checkbox to *Create new Specialization of selected Entity*. The entity will be automatically named `PartRevision` after the Teamcenter Object name, but it can be renamed here if required. Now click on *OK*, to finish the object mapping and close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-properties/object-mapping.png">}}
8. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected object. In this case, we will not be selecting any additional properties. Close the sidebar by clicking on the import mapping page.
9. Click on “Generate” button to generate the appropriate domain model and microflows
10. Once the generation is done, you will be redirected to the History tab which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/get-properties/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* `PartRevision` – This is an entity representing Part Revision in Teamcenter and serves as an input to the Java action that retrieve list of part revisions from the Teamcenter product `UIDs`. 

### Microflows {#microflows}
The following microflows are generated 
* `PartRevision_GetPropertiesByUIDs` – This microflow implements the logic to retrieve additional properties of part revisions based on the provided `UID` and Teamcenter configuration `Name` 
* `ModelObjectList_ToUIDList` – This microflow implements the logic to retrieve `UIDs` for a list of model objects. 