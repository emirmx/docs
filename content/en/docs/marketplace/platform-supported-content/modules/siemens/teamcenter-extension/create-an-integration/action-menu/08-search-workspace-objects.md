---
title: "Search Workspace Objects"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/action-menu/search-workspace-objects/
description: "Provides step by step guide to use \"Search Workspace Objects\" action in Teamcenter Extension."
weight: 8
---

## Introduction {#introduction}
The “Search Workspace Objects" action allows you to generate the domain model and microflow to search for Workspace Objects or its specialization. For those familiar with Teamcenter, the resulting microflow implements the out of the box saved query `General...` from Teamcenter.  

This document takes you through a use case where we want to retrieve FMEA (Failure Modes and Effects Analysis) object types from Teamcenter. The purpose of FMEA is to proactively identify risks involved in product design /process and mitigate/eliminate them by defining prevention and detection actions.  

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the settings tab before following these instructions. For more instructions on how to configure your settings, follow the steps [here].
2. Click on the Search Item Revision button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-workspace-objects/icon.png">}}
3. You will land on the [import mapping page](https://docs.mendix.com/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix.  
Click on one of the placeholder entities to start the import mapping.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-workspace-objects/import-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter objects (out of the box and custom) retrieved from the Teamcenter instance. Since we are interested in Retrieving FMEA objects, we select `FMEA`.  
The right side shows the relevant Mendix entities for which object could be created when we retrieve `Workspace` objects from Teamcenter. In our case, we want to have an entity specifically for FMEA so we can make changes and add new attributes to it. Hence, we select `TcConnector.WorkspaceObject` and then click on the checkbox to *Create new Specialization of selected Entity*. The entity will automatically be named after the Teamcenter Object name, but it can be renamed here if required. Now click on *OK*, to finish the object mapping and close the object mapping dialog. 
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-workspace-objects/object-mapping.png">}}
5. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected Teamcenter Object the properties you want to have within your Mendix application. In this example, we want to create an integration where we retrieve additional FMEA Object type attributes. Select in the side panel the following attributes to add to the `FMEA` entity.
   * FMEA Category
   * Last Saved Date
   * Version Information 

   {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-workspace-objects/property-mapping.png">}}
6. After this selection, click  on the close button to close the side panel.
7. Click on the Generate button to generate the appropriate Domain model and microflows so that you can start retrieving `FMEA` objects in your application
8. Once the generation is done, you will be redirected to the History tab which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-workspace-objects/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* `FMEA` – This is an entity to represent a FMEA Object from Teamcenter. The objects can be retrieved via `FMEA_PerformGeneralSearch`. 

### Microflows {#microflows}
The extension generated the following microflow in your project. 
* `FMEA_PerformGeneralSearch` - This microflow can be used to retrieve a list of `FMEA` Object types from Teamcenter.  
The microflow receives three input parameters, a `ConfigName`, `GeneralQuery` and `SearchInput` 
  1. The `configName` must be the *configName* on a `TcConnection.TeamcenterConfiguration` object and determines which Teamcenter configuration will be used to connect to Teamcenter. 
If only one Teamcenter Configuration is used, the parameter can be left empty. 
  2. The `GeneralQuery` is required and used as input arguments (entered by user as input) while searching for `FMEA`.  
  3. The `SearchInput` parameter is used to provide additional input that’s required by the Teamcenter SOA that is called upon. 

The microflow will return a list of `FMEA` Object types that matches the `SearchCriteria` provided in the `GeneralQuery`. 