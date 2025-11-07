---
title: "Integrating Mendix with Altair Graph Studio"
linktitle: "Altair Graph Studio"
url: /partners/altair/integrate-mendix-altair-graph-studio
weight: 10
description: "Describes how to use graph data from Altair Graph Studio in your Mendix application."
---

## Introduction

Altair Graph Studio provides a highly scalable graph database suitable for analytical queries, offering access to data from different backend systems. It is an extremely powerful tool to unify data from multiple backend systems and link data from different sources into one unified data model. Graph Studio provides both virtualized and replicated access to data, with powerful data ingestion facilities to ingest, transform, clean, and correlate data from different sources. 

This guide will walk you through the process of creating a graphmart in Altair Graph Studio, loading data, and querying that data from a Mendix application using OData REST endpoints and SPARQL queries.

## Prerequisites

Before you begin, make sure you have:

* Access to an Altair Graph Studio instance
* A Mendix Studio Pro development environment (version 9.x or 10.x recommended)
* Sample JSON data to import (or use the example data provided)
* Basic understanding of REST APIs and microflows in Mendix
* Graph Studio credentials (username and password for API authentication)

## Setup Graph Studio with an Ontology and Data

### Create a New Graphmart

A graphmart in Altair Graph Studio is a logical container for your data and ontologies. It provides a unified view of data from one or more sources.
   
   {{< figure src="/attachments/howto/integration/howto-ags/ags-1.png" class="no-border" >}}

### Add a Source Layer

A source layer allows you to import data from various sources or start from a JSON document. Source layers can:

* Import data from databases, APIs, or files
* Transform and map data to your ontology
* Keep data synchronized with source systems

{{< figure src="/attachments/howto/integration/howto-ags/ags-2.png" class="no-border" >}}

### Upload and Validate JSON Data

We will start by uploading an example test dataset in JSON format. You can ask an LLM like ChatGPT to generate some sample data. This how-to uses a test set with PLM (Product Lifecycle Management) and BOM (Bill of Materials) data.

Upload the JSON source data into the source layer.

{{< figure src="/attachments/howto/integration/howto-ags/ags-4.png" class="no-border" >}}

You can review the JSON structure and the data in the file you just uploaded in Graph Studio. It will show you a tree structure of your JSON file, data types, and sample values.

{{< figure src="/attachments/howto/integration/howto-ags/ags-5.png" class="no-border" >}}

### Review the Generated Ontology

When you save the source layer, an ontology will be automatically created based on the JSON structure. You can review it in:

* The **Graph** tab for visual representation
* The **Ontology Models** section for detailed class and property definitions

{{< figure src="/attachments/howto/integration/howto-ags/ags-3.png" class="no-border" >}}

**Tip:** You can customize the generated ontology by adding descriptions, constraints, and relationships between classes to better model your domain.

The **Ontologies** section will contain a definition based on the graph of the uploaded JSON data. The ontology defines the structure and main concepts of your data model.

### Create and Test a SPARQL Query

Now that you have data and an ontology in Graph Studio, create a SPARQL query to retrieve data from the graph. While Graph Studio also provides OData and SQL access, SPARQL is the most flexible way to work with graph data.

Create a new query in the Query Playground:

{{< figure src="/attachments/howto/integration/howto-ags/ags-6.png" class="no-border" >}}

Enter the SPARQL query and test the result.

{{< figure src="/attachments/howto/integration/howto-ags/ags-7.png" class="no-border" >}}

**Example SPARQL query for customers:**

```sparql
PREFIX dt: <http://cambridgesemantics.com/SourceLayer/YOUR_SOURCE_ID/Model#>

SELECT ?customerId ?customerName ?email ?phone ?city ?country
FROM <http://cambridgesemantics.com/SourceLayer/YOUR_SOURCE_ID/Model>
WHERE {
  ?customer a dt:Customer ;
            dt:customerId ?customerId ;
            dt:customerName ?customerName .
  OPTIONAL { ?customer dt:email ?email }
  OPTIONAL { ?customer dt:phone ?phone }
  OPTIONAL { ?customer dt:address ?address .
             ?address dt:city ?city ;
                      dt:country ?country }
}
ORDER BY ?customerName
```

**Note:** Replace `YOUR_SOURCE_ID` with your actual source layer ID. You can find this ID in Graph Studio by navigating to your source layer and copying the ID from the URL or source layer details.

You now have a graph ontology in Altair Graph Studio with data and a SPARQL query that returns customers.

Altair Graph Studio allows you to do much more, such as ingesting data from data lakes or APIs and linking siloed data into one unified ontology, but for now we'll focus on this basic graph.

## Expose the Data via an OData REST Endpoint

The simplest way to use Graph data in a Mendix application is by using an OData endpoint. In Altair Graph Studio, you can export an entire graph via OData. This will create an OData endpoint allowing flexible OData queries on the data in your graph database.

### Create an OData API in Graph Studio

Create a new OData API in your graphmart. Go to the **Exports** section in Graph Studio and select **Create OData REST Endpoint**:

{{< figure src="/attachments/howto/integration/howto-ags/ags-17.png" class="no-border" >}}

Choose **"auto generated endpoint"**. This is a one-click method to expose your entire graph via an OData API.

{{< figure src="/attachments/howto/integration/howto-ags/ags-18.png" class="no-border" >}}

Provide a name and optionally a description for the OData endpoint.

{{< figure src="/attachments/howto/integration/howto-ags/ags-19.png" class="no-border" >}}

The **ODBC (SQL)** endpoint listed on this screen can be used to access the endpoint. You should now be able to open this endpoint in a browser and see the OData response.

{{< figure src="/attachments/howto/integration/howto-ags/ags-20.png" class="no-border" >}}

To see the contract of the endpoint, use the endpoint URL displayed in the endpoint configuration page under **ODBC (SQL)** and append `/$metadata` to this URL. You will need it when using the endpoint in Mendix.

### Use the OData Endpoint in a Mendix Project

Add a consumed OData service in your project. Here you need to provide the location of the endpoint `$metadata` contract and username and password credentials. Mendix will read the contract to determine all the entities and actions provided by this endpoint.

{{< figure src="/attachments/howto/integration/howto-ags/ags-21.png" class="no-border" >}}

In the consumed OData service document, you can provide configuration for the endpoint location and credentials. The **Location** is the root endpoint URL you copied from Graph Studio (without the `$metadata` suffix).

{{< figure src="/attachments/howto/integration/howto-ags/ags-22.png" class="no-border" >}}

### Create External Entities for Datasets from the OData Endpoint

The integration pane on the right shows all the graph classes exposed via the OData REST API. Here you can see the attributes (properties) of these classes and the associations. The integration pane also shows the capabilities of the data provided by the endpoint. In this example, all the data is read-only, as the Altair Graph Database only provides read-only access to the data.

Select which classes you need for your application and drag and drop them into a domain model. Here they will be displayed as purple external entities. This indicates that the data is available to your application but is retrieved from an external service—in this case, the Altair Graph Database.

{{< figure src="/attachments/howto/integration/howto-ags/ags-23.png" class="no-border" >}}

### Build Pages Using the External Entities

You can use these external entities in your pages and microflows similarly to regular persistent entities. In this example, we generate overview pages for these external entities. 

{{< figure src="/attachments/howto/integration/howto-ags/ags-24.png" class="no-border" >}}

### Run the Application and Test

You can now start the application and test the results. Whenever you open a page using external entities from the integration pane, the data will automatically be retrieved from Altair Graph Studio via an OData REST call.

{{< figure src="/attachments/howto/integration/howto-ags/ags-25.png" class="no-border" >}}

Filtering, sorting, and pagination are automatically done server-side by your graph database. This is essential for large datasets to avoid overloading the frontend with too many objects.

### Lazy-Loaded Tree Widget

Altair Graph Studio automatically provides associations for linked entities in your graph. These graph associations are part of the OData endpoint and part of your external entities. When you drag your OData datasets into your domain model, the resulting external entities will show associations from the OData endpoint.

{{< figure src="/attachments/howto/integration/howto-ags/ags-26.png" class="no-border" >}}

This example page shows a tree that retrieves BOMs from the graph database. It will only load required data—first the top-level BOMs, then when you open one, its components will be retrieved over the association, and only when you open a component will it load the subcomponents.

This is done completely automatically when you set the external entities as the data source for the widgets in your tree.

{{< figure src="/attachments/howto/integration/howto-ags/ags-27.png" class="no-border" >}}

## Query SPARQL in Mendix

As just demonstrated, external entities leveraging the OData export facility of Altair Graph Studio are the easiest way to use your graph data in Mendix. If you need full control over your graph queries, you can use Mendix consumed REST services to execute SPARQL queries in your graph database.

### Configuring the REST Call

You can execute SPARQL queries from Mendix by using the SPARQL API in Altair Graph Studio.
Create a new REST client document and paste the query in the request body.

{{< figure src="/attachments/howto/integration/howto-ags/ags-9.png" class="no-border" >}}

The endpoint contains two main parts: the endpoint of the SPARQL API and the graphmart you want to work with. You can define these in the endpoint.

Define the content types of the request and response:

* Set the **Content-Type** to `application/sparql-query`
* Set **Accept** to `application/sparql-results+json`

{{< figure src="/attachments/howto/integration/howto-ags/ags-10.png" class="no-border" >}}

Now you can execute the API call and validate the resulting response message. The right-hand panel will show you the response JSON payload provided by your graph database.

{{< figure src="/attachments/howto/integration/howto-ags/ags-11.png" class="no-border" >}}

If the result is what you need, you can generate entities to capture the data in Mendix. Here you may want to rename the entities to a more descriptive name.

{{< figure src="/attachments/howto/integration/howto-ags/ags-12.png" class="no-border" >}}

#### Resulting Domain Model

The resulting domain model should look like this:

{{< figure src="/attachments/howto/integration/howto-ags/ags-8.png" class="no-border" >}}

### Microflow to Execute the SPARQL Query

Next, define a microflow that will execute the API call with the SPARQL query and return the resulting data as an entity list. In this example, you see that the endpoint is defined using two constants: one for the graph database API and one for the graphmart. By using constants, you can override these on deployment to match the environment you're deploying to.

{{< figure src="/attachments/howto/integration/howto-ags/ags-13.png" class="no-border" >}}

### Customers Overview Page

Finally, we need a page that will show the retrieved data. This example uses a Data Grid 2 with a microflow data source. The microflow data source is configured to use the microflow that was created in the previous step.

{{< figure src="/attachments/howto/integration/howto-ags/ags-14.png" class="no-border" >}}

### Test the Result

You can now run your application to validate the result. It should look like this—we have a data grid showing all objects and a detail form showing the details of one specific object.

{{< figure src="/attachments/howto/integration/howto-ags/ags-15.png" class="no-border" >}}

Detail page showing data from a single concept in your graph database:

{{< figure src="/attachments/howto/integration/howto-ags/ags-16.png" class="no-border" >}}

## Summary

As illustrated, Altair Graph Studio (AGS) allows you to store and retrieve graph data structures. Using either OData REST endpoints or SPARQL queries via the SPARQL API endpoint, you can retrieve this data into your Mendix application.

The OData approach provides the easiest integration with automatic entity mapping and association handling, while the SPARQL approach offers more control for complex graph queries. Both methods enable you to build powerful applications that leverage the scalability and flexibility of graph databases. 
