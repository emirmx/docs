---
title: "Search Datasets"
url: /appstore/modules/siemens-plm/teamcenter-extension/action-menu/search-datasets/
description: "Provides step by step guide to use \"Search Datasets\" action in Teamcenter Extension."
weight: 5
---

## Introduction {#introduction}
The Search Datasets action allows you to generate the domain model and microflow to search for datasets or its specialization in Teamcenter. The resulting microflow implements the Teamcenter out of the box saved query “Datasets”.  

This document takes you through a use case where we want to create the logic to search datasets with the option of choosing different dataset types e.g. PDF, MS Word, etc.) 

## Step-by-step Guide {#step-by-step-guide}
1. Make sure you have set up your credentials in the settings tab before following these instructions. For more instructions on how to configure your settings, follow the steps [here].
2. Click on the Search Datasets icon on the home page to start configuring your integration.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-datasets/search-datasets.png">}}
3. You will land on the [import mapping page](https://docs.mendix.com/refguide/import-mappings/). This determines what data is retrieved from Teamcenter and what type of objects are created in Mendix.  
Click on one of the placeholder entities to start the import mapping.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-datasets/import-mapping.png">}}
4. In the object mapping dialog that opens, the left side shows Teamcenter datasets (out of the box and custom) retrieved from the Teamcenter instance. Since we are interested in searching datasets (including all different types), select Dataset.  
The right side shows the relevant Mendix entities for which object could be created when we retrieve Datasets from Teamcenter. In our case, we want to have an entity specifically for Dataset. Hence, we select *TcConnector.Dataset* and then click on the checkbox to *Create new Specialization of selected Entity*. The entity will automatically be named Dataset after the Teamcenter Object name, but it can be renamed here if required. Now click on OK, to finish the object mapping and close the object mapping dialog
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-datasets/object-mapping.png">}}
5. Closing the object mapping dialog opens the attributes and associations sidebar. Here you can select from all properties available on the selected Teamcenter Object the properties you want to retrieve within your Mendix application. In this example, we won’t add any additional properties, hence no selections are necessary.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-datasets/property-mapping.png">}}
6. Click on the Generate button to generate the appropriate Domain model and microflows.
7. Once the generation is done, you will be redirected to the History tab which shows a summary of what has been generated.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-extension/search-datasets/history.png">}}

## Result {#result}
### Domain Model {#domain-model}
The extension generated the following entities in your domain model. 
* Dataset – This is an entity to represent a Dataset object from Teamcenter. This serves as an input to microflow that attaches datasets.  

### Microflows {#microflows}
The extension generated the following microflows in your project. 
* Datatset_SavedQueryDataset – This microflow implements the Teamcenter saved query “Dataset”. The microflow receives two input parameters, a ConfigName and DatasetSearchCriteria. 
   1. The configName must be the *configName* on a *TcConnection.TeamcenterConfiguration* object and determines which Teamcenter configuration will be used to connect to Teamcenter. 
If only one Teamcenter Configuration is used, the parameter can be left empty.
   2. The DatsetSearchCriteria can be used as input arguments while searching for Datasets. 
* Dataset_RetrieveFilesorImages – This microflow implements the logic to retrieve files stored inside the dataset. This is a helper microflow that can be used to retrieve files on the returned dataset   
* GetFileTypesForDatasetType – This microflow implements the logic to retrieve all file types associated to a particular dataset type. 
* GetAvailableDatasetTypes – This microflow implements the logic to retrieve dataset types for your given Teamcenter instance 
* Dataset_GetImanFiles – This microflow implements the logic to retrieve the ImanFile object associated with an input dataset object 