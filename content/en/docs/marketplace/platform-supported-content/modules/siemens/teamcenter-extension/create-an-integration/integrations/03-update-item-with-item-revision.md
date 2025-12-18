---
title: "Update Item with Item Revision"
url: /appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/integrations/update-item-with-item-revision/
description: "Provides step by step guide to use \"Update Item with Item Revision\" integration in Teamcenter Extension."
weight: 3
---

## Introduction {#introduction}
The **Update Item and Item Revision** integration allows you to generate the domain model and microflow to update an Item with ItemRevision or their specializations in Teamcenter. The resulting microflows implement the Update Properties action from the Teamcenter Connector. 

This document takes you through a use-case where you want to update the properties (e.g. `description`) of **CAE 3D Analysis Revision Reports** in Teamcenter. **CAE 3D Analysis Revision** business object are utilized to manage files associated from a 3D simulation study (CFD, Structural, etc.) 

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the **Settings tab** before following these instructions. For more instructions on how to configure your settings, follow the steps [here](/appstore/modules/siemens-plm/teamcenter-extension/create-an-integration/#settings-tab).
2. Click on the **Update Item and Item Revision** button on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/icon.png">}}
3. You will land on the [import mapping page](/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix. Click on the one of placeholder entities to start import mapping.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/import-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter objects (both out of the box and custom) retrieved from the Teamcenter instance. Since we are interested in updating **CAE 3D Analysis Revision**, look for `CAE 3D Analysis`. 
The right side shows the Mendix entities that can serve as input parameters for the microflow to update Item with Item revision. Notably, if we create our own entity here, we can ensure that additional attributes are set in Teamcenter upon update. Hence, we select `TcConnector.Item` and then click on the checkbox to **Create new Specialization of selected Entity**. The entity will automatically be named `CAEAnalysis` after the Teamcenter Object name, but it can be renamed here if required. Now click on **OK**, to finish the object mapping and close the object mapping dialog.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/object-mapping.png">}}
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/object-mapping-result.png">}}
5. Since we also want to update **CAE 3D Analysis Revision**, repeat this process to select `CAE 3D Analysis Revision` with another specialized entity.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/object-mapping-association.png">}}
6. Double clicking on any of the boxes opens up the **Properties** window. Since we are interested in updating the `description` attribute and it is already selected, we won’t select any additional attribute.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/property-mapping.png">}}
7. Click **Generate** to generate the domain model and microflow.
8. Once the generation is done, you will be redirected to the **History tab** which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* **CAE3DAnalysis** – This is an entity representing a CAE 3D Analysis from Teamcenter. 
* **CAE3DAnalysisRevision** - This is an entity representing a CAE 3D Analysis Revision from Teamcenter.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/update-item-with-item-revision/domain-model.png">}}

### Microflows {#microflows}
The extension generated the following microflows in your project. 
* **CAE3DAnalysis_UpdateItem** – This microflow implements the logic to update properties of CAE 3D Analysis. The microflow takes two input parameters: `CAE 3D Analysis` and `ConfigName`. It returns one output variable named `CAE3DAnalysis_Updated_Cast`.
* **CAE3DAnalysisRevision_UpdateItemRevision** – Its similar microflow but for updating properties of CAE 3D Analysis Revision. 