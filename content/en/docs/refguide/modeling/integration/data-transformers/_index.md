---
title: "Data Transformers"
url: /refguide/data-transformers/
weight: 50
description: "Describes Data Transformers in Mendix Studio Pro."
---

## Introduction

Data Transformer can be used for transforming data of certain structure into another structure, basically a message-to-message transformation within Mendix Studio Pro. With this feature, you can pre-process an incoming message (for example, from an API response, MQTT message, etc.) before an Import Mapping. Additionally, you can also use it to transform a message before passing it on to a downstream system that expects the data in a certain structure.

{{% alert color="info" %}}
This feature is in beta and at the moment we only support JSON-to-JSON transformation with JSLT, a JSON transformation language.
{{% /alert %}}

### Example

Consider an API that returns customer data with many fields, but you only need a few specific fields for your Mendix app:

**Input JSON from API:**

```json
{
  "customer_id": "12345",
  "first_name": "John",
  "last_name": "Smith",
  "email": "john.smith@example.com",
  "phone": "+1-555-0123",
  "address": "123 Main St",
  "city": "New York",
  "country": "USA",
  "account_status": "active",
  "credit_limit": 5000,
  "internal_notes": "VIP customer",
  "created_date": "2024-01-15"
}
```

**JSLT Transformation:**

```jslt
{
  "id": .customer_id,
  "fullName": .first_name + " " + .last_name,
  "email": .email,
  "location": .city + ", " + .country
}
```

**Output JSON:**

```json
{
  "id": "12345",
  "fullName": "John Smith",
  "email": "john.smith@example.com",
  "location": "New York, USA"
}
```

In this example, the Data Transformer extracts only the needed fields, combines the first and last name into a single field, and creates a location string from the city and country. This simplified output can then be easily mapped to your Mendix entities.

## Limitations

At the moment we only support JSON-to-JSON transformation with JSLT, a JSON transformation language.

## Prerequisites

* [Studio Pro 11.11](https://marketplace.mendix.com/link/studiopro/11.11.0) and above

## Add the Data Transformer Document

You can add the Data Transformer document to your app, by following these steps:

1. Right-click the module you want to add the Data Transformer document to.
2. Select **Add other** > **Data Transformer**.
3. Name the data transformer.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/add-data-transformer.png" alt="Add Data Transformer dialog" >}}

4. In the **Input JSON** editor you can paste a JSON snippet that you would like to transform.
5. Define the transformation in **JSLT transformation** editor.
6. Click the **Test Transformation** button below the JSLT transformation editor to preview the transformation result in the **Output JSON**.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/define-transformation.png" alt="Data Transformer interface showing Input JSON, JSLT transformation, and Output JSON editors" >}}

## Use the Data Transformer in a Microflow

To perform a transformation in a microflow, complete the following steps:

1. Drag the **Transform JSON** activity into a microflow, preferably after a REST call or anything that provides input for the transformation.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/transform-json-dialog.png" alt="Add Transform JSON activity" >}}

2. Double-click the activity and click **Select** to choose an existing Data Transformer document or create a new one.
3. Click on the dropdown **Variable (String)** and select the input string variable from the list.
4. Specify the name of the output in the **Variable name** text field.
5. Click **OK**.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/configure-transformer-activity.png" alt="Configure Transform JSON activity dialog" >}}

## Using the output of Data Transformer

You can use your transformed JSON snippet in the following ways:

* Create a new JSON structure for Import Mapping
* Pass the transformed JSON to downstream systems
* Use it as input for further processing in your microflow


## Use Cases

Use the Data Transformer document to do the following:

* [Filtering out unused fields](/refguide/data-transformer-how-tos/#filtering-out-unused-fields)
* [Simplifying nested structures](/refguide/data-transformer-how-tos/#simplifying-nested-structures)
* [Normalising objects to arrays (working with dynamic keys)](/refguide/data-transformer-how-tos/#normalising-objects-to-arrays-working-with-dynamic-keys)
* [Zipping metadata with data](/refguide/data-transformer-how-tos/#zipping-metadata-with-data)
* [Flattening Bill of Materials (BOM)](/refguide/data-transformer-how-tos/#flattening-bill-of-materials-bom)
* [Extracting information from a string](/refguide/data-transformer-how-tos/#extracting-information-from-a-string)
* [Working with SPARQL query results](/refguide/data-transformer-how-tos/#working-with-sparql-query-results)

For detailed examples, see [Data Transformer How-Tos](/refguide/data-transformer-how-tos/).

## JSLT

You can find more information about JSLT below:

* A short [introduction](https://github.com/schibsted/jslt/blob/master/README.md#jslt) and [tutorial](https://github.com/schibsted/jslt/blob/master/tutorial.md#jslt-tutorial) on how to use JSLT
* A complete list of [functions available in JSLT](https://github.com/schibsted/jslt/blob/master/functions.md#jslt-functions)