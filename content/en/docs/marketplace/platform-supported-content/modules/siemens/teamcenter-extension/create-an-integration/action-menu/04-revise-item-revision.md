---
title: "Revise Item Revision"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/action-menu/revise-item-revision/
description: "Provides step by step guide to use \"Revise Item Revision\" action in Teamcenter Extension."
weight: 4
---

## Introduction {#introduction}
The “Revise Item Revision” action allows you to configure and generate the domain model and microflow to revise an Item Revision or its specializations in Teamcenter. The resulting microflow implements the Revise Object and Update Properties actions from the Teamcenter Connector.  

This document takes you through a use case where we need to create a revision of a Requirement Revision object. The Requirements object in Teamcenter facilitates the capture, organization, and tracking of various product requirements throughout the product lifecycle. Requirements undergo changes during the product's lifecycle and as such new revisions of this object are created.  

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the settings tab before following these instructions. For more instructions on how to configure your settings, follow the steps [here].
2. Click on the Revise Item Revision button on the home page to start configuring your integration
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/revise-item-revision/icon.png">}}
3. You will land on the [import mapping page](https://docs.mendix.com/refguide/import-mappings/). This determines which Mendix entity will be used to create what type of revised Teamcenter Item Revision.
Click on one of the top placeholder entities to start the import mapping. 
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/revise-item-revision/object-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter objects (both out of the box and custom) retrieved from the Teamcenter instance. Since we are interested in revising Requirements Revisions, we select `Requirement Revision` found under parent `Item Revision`.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/revise-item-revision/object-mapping.png">}}
The right side shows the relevant Mendix entities which could be used to create the new Revision of the Item Revision in Teamcenter.. In our case, we want to have an entity specifically for Requirement Revisions so we can choose custom properties to be set when revising the Item Revision. Hence, we select `TcConnector.ItemRevision` and then click on the checkbox to *Create new Specialization of selected Entity*. The entity will automatically be named `RequirementRevision` after the Teamcenter Object name, but it can be renamed here if required. Now click on *OK*, to finish the object mapping and close the object mapping dialog.
5. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected Teamcenter Object the properties you want to have within your Mendix application. In this case, we’d like Name, Item ID and Description to be provided as attributes that can be modified when revising the item revision. Hence, we should make sure we check the write column for these attributes. Since they’re already checked, no further selections are required in the sidebar.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/revise-item-revision/property-mapping.png">}}
6. Click on the Generate button to generate the appropriate domain model and microflows
7. Once the generation is done, you will be redirected to the History tab which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/revise-item-revision/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* `ReviseRequirementRevision` - This is a helper entity that is used in the `RequirementRevision_ReviseItemRevision` microflow to revise a Requirement revision in Teamcenter.
* `RequirementRevision` - This is an entity representing Requirement Revisions from Teamcenter. It can be used to revise a new Requirement Revision in Teamcenter. 

### Microflows {#microflows}
The extension generated the following microflows in your project.
* `RequirementRevision_ReviseItemRevision` – This microflow implements the logic to revise an Item Revision in Teamcenter 