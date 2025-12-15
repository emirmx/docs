---
title: "Search Item Revisions"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/action-menu/search-item-revisions/
description: "Provides step by step guide to use \"Search Item Revisions\" action in Teamcenter Extension."
weight: 1
---

## Introduction {#introduction}
The  “Search Item Revisions” action allows you to generate the domain model and microflow to search for Item Revisions or its specialization. For those familiar with Teamcenter, the resulting microflow implements the out of the box saved query “Item Revision...” from Teamcenter.  

This document takes you through a use case where we want to retrieve Part Revisions from Teamcenter. Part Revisions are specific iterations of a part of a product that is managed using Teamcenter. A Part Revision contains essential information like the schematics of the Part. As a Part Revision is a sub-type of an Item Revision, it can be revised.

## Step-by-step guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the settings tab before following these instructions. For more instructions on how to configure your settings, follow the steps [here](https://marketplace.mendix.com/link/component/111627).
2. Click on the Search Item Revision button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-item-revisions/icon.png">}}
3. You will land on the [import mapping page](https://docs.mendix.com/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix.
Click on one of the placeholder entities to start the import mapping.  
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-item-revisions/import-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter objects (out of the box and custom) retrieved from the Teamcenter instance. Since we are interested in Retrieving Part Revisions, we select `Part Revision (Part Revision)`, which can be found under `Item Revision`.  
The right side shows the relevant Mendix entities for which object could be created when we retrieve Item revisions from Teamcenter. In our case, we want to have an entity specifically for Part Revisions so we can make changes and add new attributes to it. Hence, we select `TcConnector.ItemRevision` and then click on the checkbox to *Create new Specialization of selected Entity*. The entity will automatically be named `PartRevision` after the Teamcenter Object name, but it can be renamed here if required. Now click on *OK*, to finish the object mapping and close the object mapping dialog. 
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-item-revisions/object-mapping.png">}}
5. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected Teamcenter Object the properties you want to have within your Mendix application. In this example, we want to create an integration where we retrieve additional Part Revision details. Select in the side panel the following attributes to add to the Part Revision entity.
   * Finish type
   * Multi body
   * Safety Part
   * Standard
   * Spare Part
   * Technology intent

   After this selection, click on the backdrop or on the close button to close the side panel. 
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-item-revisions/property-mapping.png">}} 
6. Click on the Generate button to generate the appropriate Domain model and microflows so that you can start retrieving Part Revisions in your application 
7. Once the generation is done, you will be redirected to the History tab which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-item-revisions/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
- `PartRevision` – This is an entity to represent a `PartRevision` from Teamcenter. The objects can be retrieved via `PartRevision_SavedQueryItemRevision`. 

### Microflows {#microflows}
The extension generated the following microflows in your project. 
* `PartRevision_SavedQueryItemRevision` - This microflow can be used to retrieve a list of Part Revisions from Teamcenter.  
The microflow receives two input parameters, a `ConfigName` and `ItemRevisionSearchCriteria`.
  1. The `configName` must be the *configName* on a *TcConnection.TeamcenterConfiguration* object and determines which Teamcenter configuration will be used to connect to Teamcenter. 
If only one Teamcenter Configuration is used, the parameter can be left empty.
  2. The `ItemRevisionSearchCriteria` is required and must be used as input arguments while searching for the ItemRevisions. [For hints on efficient use of this parameter see section here] 

The microflow will return a list of `PartRevision` that matches the `SearchCriteria` provided in the `ItemRevisionSearchCriteria`. 

## Tips and Tricks {#tips-and-tricks}
### Item Revision Search Criteria {#item-revision-search-criteria}
* It is highly recommended to provide the `_Type` field as it significantly optimizes the `ItemRevision_SavedQueryItemRevision` microflow. The `_type` field specifies which types of `ItemRevisions` will be requested. The field should be set to `PartRevision` to retrieve only `PartRevisions and specializations of it. 
* The fields `Name` and `ItemID` on `ItemRevisionSearchCriteria` support wildcards. For example, if `Name` is set to `a*`, all revisions will be returned that have a name that start with an `a`. 