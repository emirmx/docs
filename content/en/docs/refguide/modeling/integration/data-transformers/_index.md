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

## Use Cases

Use the Data Transformer document to do the following:

* [Filtering out unused fields](/refguide/data-transformer-how-tos/#filtering-unused-fields)
* [Simplifying nested structures](/refguide/data-transformer-how-tos/#simplifying-nested)
* [Normalising objects to arrays (working with dynamic keys)](/refguide/data-transformer-how-tos/#normalising-objects)
* [Zipping metadata with data](/refguide/data-transformer-how-tos/#zipping-metadata)
* [Flattening Bill of Materials (BOM)](/refguide/data-transformer-how-tos/#flattening-bom)
* [Extracting information from a string](/refguide/data-transformer-how-tos/#extracting-string)

For detailed examples, see [Data Transformer How-Tos](/refguide/data-transformer-how-tos/).

## Limitations

At the moment we only support JSON-to-JSON transformation with JSLT, a JSON transformation language.

## Prerequisites

* Studio Pro [11.11](/releasenotes/studio-pro/11.11/) and above
* Familiarity with [JSLT](https://github.com/schibsted/jslt)

## Adding the Data Transformer Document

Download Studio Pro and add the Data Transformer document to your app. To do this, follow these steps:

1. Right-click the module you want to add the Data Transformer document to.
2. Select **Add other** > **Data Transformer**.
3. Name the data transformer.

## Defining a Transformation

Follow these steps to define a transformation:

1. In the **Input JSON** editor you can paste a JSON snippet that you would like to transform.
2. Define the JSLT transformation in the middle text editor.
3. Click the **Test Transformation** button below the JSLT transformation editor to see the transformation result in the **Output JSON**.
4. You can then click the **Copy** button below the Output JSON and you can use the simplified snippet for creating a new JSON structure.

### Specifying Input JSON

Paste an example JSON snippet that you would like to transform.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/input-json-editor.png" alt="Input JSON editor" >}}

### Defining the JSLT

Define the JSLT transformation in the middle text editor. Currently only high-code, manual transformation with JSLT is supported.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/jslt-editor.png" alt="JSLT transformation editor" >}}

Here you can find the documentation for JSLT:

* A short [introduction and tutorial](https://github.com/schibsted/jslt) on how to use JSLT
* A complete list of [functions available in JSLT](https://github.com/schibsted/jslt/blob/master/functions.md)
* [Transformation examples](/refguide/data-transformer-how-tos/)

### Testing the Transformation and Using the Output

Click the **Test Transformation** button below the JSLT editor to see the transformation result.

{{< figure src="/attachments/refguide/modeling/integration/data-transformers/test-transformation.png" alt="Test transformation output" >}}

You can use your transformed snippet in the following ways:

* Create a new JSON structure for Import Mapping
* Pass the transformed JSON to downstream systems
* Use it as input for further processing in your microflow

### Using the Data Transformer in a Microflow

To perform a transformation in a microflow, complete the following steps:

1. Drag the **Transform JSON** activity into a microflow, preferably after a REST call or anything that provides input for the transformation.

    {{< figure src="/attachments/refguide/modeling/integration/data-transformers/transform-json-activity.png" alt="Transform JSON activity in microflow" >}}

2. Double-click the activity and click **Select** to choose an existing Data Transformer document or create a new one.

    {{< figure src="/attachments/refguide/modeling/integration/data-transformers/select-data-transformer.png" alt="Select Data Transformer document" >}}

3. Click on the dropdown **Variable (string)** and select the input string variable from the list.

    {{< figure src="/attachments/refguide/modeling/integration/data-transformers/select-input-variable.png" alt="Select input variable" >}}

4. Specify the name of the output in the **Variable Name** text field.

    {{< figure src="/attachments/refguide/modeling/integration/data-transformers/specify-output-name.png" alt="Specify output variable name" >}}

5. Click **OK**.

    {{< figure src="/attachments/refguide/modeling/integration/data-transformers/transform-json-dialog.png" alt="Transform JSON dialog" >}}

## Read More

For practical, example-driven walkthroughs of common JSLT transformation patterns, see [Data Transformer How-Tos](/refguide/data-transformer-how-tos/).
