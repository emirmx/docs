---
title: "Relate Workspace Objects"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/integrations/relate-workspace-objects/
description: "Provides step by step guide to use \"Relate Workspace Objects\" integration in Teamcenter Extension."
weight: 9
---

## Introduction {#introduction}
The **Relate Workspace Objects** integration allows you to generate the domain model and microflow to relate two **Workspace Objects** or their specialization from Teamcenter. The resulting microflow implements the `Create relation` action from the TcConnector module.
This document takes you through a use case where we want to relate a **CAE 3D Analysis** object with an **Item Revision**. A **CAE 3D Analysis** object typically contains simulation files of a **CAE (Computer Aided Engineering) 3D Analysis**. It can also contain results after the simulation has completed. The **Item Revision** contains the **CAD Parts** on which the simulation is to be performed. Users would want to relate the two objects to trace for which parts a 3D simulation has been performed. The relation type that will be created between the two objects is `CAE Target`. 


## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the **Settings tab** before following these instructions. For more instructions on how to configure your settings, follow the steps [here](/appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/#settings-tab).
2.	Click on the **Search Item Revision** button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/relate-workspace-objects/icon.png">}}
3. You will be presented with the [import mapping page](/refguide/import-mappings/). Here we can determine what data is retrieved from Teamcenter and what type of objects should be created in Mendix.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/relate-workspace-objects/import-mapping.png">}}
4. When defining a relationship between two objects in Teamcenter, the object from which the relationship originates is called the **Primary Object** while the object to which it’s referenced is called the **Secondary Object**. For this reason, you’re seeing two placeholder entities on the import mapping page – **Primary entity** and **Secondary entity**. In our case, our primary object will be `Item Revision` while the secondary object will be `CAE 3D Analysis Revision`.
5. Click on the **Primary entity** placeholder.
6. In the object mapping dialog that opens, the left side shows Teamcenter objects (out of the box and custom) retrieved from your configured Teamcenter instance. Since we are interested in retrieving **Item Revision** objects, we select `Item Revision`.  
The right side shows the relevant Mendix entities that can be created when we retrieve **Workspace Objects** from Teamcenter. In our case, we are only relating two objects and won’t be making any change to it. Hence, we just select the marketplace entity `TcConnector.WorkspaceObject`.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/relate-workspace-objects/object-mapping.png">}}
7. Closing the object mapping dialog opens the relations sidebar. Here you can choose to select the relationship name between the two objects. If you choose a **Relation Type**, the extension will generate a microflow to relate the two objects with the selected relation type. If no relation name is selected, the relation type needs to be provided manually in the microflow that gets generated to relate the objects.
8. In our case, we will select the `CAE Target` relationship.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/relate-workspace-objects/property-mapping.png">}}
9. After this selection, click on the **close** button to close the side panel.
10. Click on the **Secondary entity** placeholder and repeat the previous steps. But this time select `CAE 3D Analysis Revision` on the left and `TcConnector.ModelObject` on the right.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/relate-workspace-objects/object-mapping-association.png">}}
11. Click on the **Generate** button to generate the appropriate Domain model and microflows.
12. Once this has been completed, you will be redirected to the **History tab** which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/relate-workspace-objects/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension has not generated any new entities since we are reusing the existing entities from the TcConnector Marketplace module. 

### Microflows {#microflows}
The extension generated the following microflow in your project. 
* **WorkspaceObject_RelateToModelObject** - This microflow can be used to relate Primary object and Secondary object (i.e. `Item Revision` and `CAE 3D Analysis Revision` in this case).  
The microflow receives four input parameters, a **ConfigName**, **Primary Workspace Object**, **Secondary Workspace Object** and **Relation Type**.
  1. The `configName` must be the **configName** of a **TcConnection.TeamcenterConfiguration** object and determines which Teamcenter configuration will be used to connect to Teamcenter. 
If only one Teamcenter Configuration is used, the parameter can be left empty. 
  2. The **Primary** and **Secondary Workspace Objects** are the objects between which the relation is to be created.
  3. The **Relation** is the **Relationship Type** between the two objects. In our case, this will be `CAE Target`.