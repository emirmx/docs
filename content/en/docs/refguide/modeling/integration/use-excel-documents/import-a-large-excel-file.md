---
title: "Import a Large Excel File"
url: /refguide/import-a-large-excel-file/
weight: 10
aliases: /howto/integration/import-a-large-excel-file/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## Introduction

Use XML-to-domain mapping to create a new import entity from an Excel sheet in a quick, semi-automated way. There is also a new & fully automated way available to create placeholder entity based off of Excel files, refer section **Using Data Importer Extension to create Entity using a Large Excel.**

This how-to teaches you how to do the following:

* Import a large Excel file with a lot of columns

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisite:

* Have an app with the [MxModel Reflection](/appstore/modules/model-reflection/) and [Excel Importer](/appstore/modules/excel-importer/) modules installed and configured from the Mendix Marketplace.

## Modifying Your Excel Sheet

The Excel sheet you receive in this scenario contains almost every country in the world, as well as some supporting data. This data has to be imported into your application.

You can find the Excel sheet here: [Countries](/attachments/refguide/modeling/integration/import-a-large-excel-file/Countries.xlsx).

You are going to make an XSD schema from the headers in the Excel sheet so you can import the data into the model.

To modify your Excel sheet, follow these steps:

1. Select the header row with all the country names.
2. Copy and paste them in a new sheet using the transpose function:

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/transpose.png" class="no-border" >}}

    Your headers should be listed vertically and your sheet should look like this: [Countries Transposed](/attachments/refguide/modeling/integration/import-a-large-excel-file/CountriesTransposed.xlsx).

    You are now ready to add some tags around the column.

3. Add one column to the left.
4. Place the following string in cell **A1**:

    ```text
    <xs:element type="xs:string" name="
    ```

5. Drag the string all the way down to the last country.

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/country-sheet.png" class="no-border" >}}

6. In cell **C1**, enter the following string:

    ```text
    "/>
    ```

7. Like you did with the previous string, drag it down to the last country. The Excel sheet should now look like this: [Countries with Tags](/attachments/refguide/modeling/integration/import-a-large-excel-file/CountriesWithTags.xlsx).

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/countries-with-tags.png" class="no-border" >}}

    Now, group the three different columns into one column. This is necessary to copy the whole column into an XSD file.

8. Select cell **D1** and type in the following into the formula box: 

    ```text
    =(A1&B1&C1)
    ```

9. Drag the cells down like you’ve done with column **A1** and **C1**. Now, column **D** should have columns **A**, **B**, and **C** combined into one, and your sheet should look like this: [Countries with Tags and Column D](/attachments/refguide/modeling/integration/import-a-large-excel-file/CountriesWithTagsAndColumnD.xlsx).

## Preparing Your XSD File

You have used some of Excel's basic functionalities to create the first part of the XSD structure. For a proper XSD file, it needs a header and a footer. To prepare your XSD file, follow these steps:

1. Open a new file and name it *CountriesImport.xsd*.
2. Place the following text as the header of your XSD file:

    ```xsd
    <?xml version="1.0"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" attributeFormDefault="unqualified" elementFormDefault="qualified">
    <xs:element name="CountriesImport">
    <xs:complexType>
    <xs:sequence>
    ```

3. Go back to your sheet, copy the content from column **D**, and paste it underneath the header.  

   {{% alert type="info" %}} Sometimes the content copied from Excel contains extra double quotes. To eliminate these, paste the Excel content into Word, and then copy it from Word and paste it into the XSD file. {{%/alert%}}

4. Place the following text as the footer:

    ```xsd
    </xs:sequence>
    </xs:complexType>
    </xs:element>
    </xs:schema>
    ```

    Your file should look like this: [Country Import](/attachments/refguide/modeling/integration/import-a-large-excel-file/CountryImport.xsd).

5. Click **Save**.

## Importing into Your Application Project

The XSD file is ready to be imported into your app. To import it, follow these steps:

1. Open your app and create a new XSD schema. Do this by right-clicking the module and selecting **Add other** > **XML schema**.

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/create-schema.png" class="no-border" width="600" >}}

2. With the new XSD schema, create the XML-to-domain mapping by right-clicking the module > **Add other** > **Import mapping**.

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/create-mapping.png" class="no-border" width="600" >}}

3. Check all the attributes listed. After clicking **OK**, you see a mapping entity with all your countries.

4. You will now generate a real entity from it that you can use as an import table for your Excel sheet. Click **Map automatically**:

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/map-automatically.png" class="no-border" width="400" >}}

    Your entity is created:

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/new-entity.png" class="no-border" >}}

5. Open your domain model and set the entity’s **Persistable** property to **Yes**. 

    {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/persistable-properties.png" class="no-border" >}}

The data is imported to the page, as seen in the image below:  

{{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/large-file.png" class="no-border" >}}

To keep your application clean, you can delete the XSD schema and XML-to-domain files from your app.

A video demonstrating this technique can be viewed below:  

{{< youtube 8qLyIoUqKEE >}}

## Using Data Importer Extension to create Entity using a Large Excel.

{{% alert color="info" %}}
Mendix Studio Pro 10.7 or above is required for this approach. You can also use these steps to create an entity from a CSV file.
{{% /alert %}}

The [Data Importer](/appstore/modules/data-importer/) extension can be used to automatically create an entity in your domain model. This example uses the same input Excel (*countries.xlsx*) to create an entity. 

To create entity in your domain model using an Excel sheet, follow these steps:

1. Right-click your module and navigate to **Add other** > **Data Importer**.
2. Provide a name for the Data Importer document. You will then have the ability to upload a sample file.
3. Drop the *Countries.xlsx* file or click **Select a local file** and navigate to the file.
4. Set the configuration in terms of **Sheet Name**, **Header Row No**, and **Read Data from**.
5. Click **Preview Source Data & Entity**
   * If the column names do not conform to Mendix naming conventions, they will automatically be corrected.    
   * The extension identifies correct data types of each column (such as string, boolean, or date).
6. After reviewing the preview, click **Create Entity** and a non-persistable entity (NPE) is created in your domain model.
   {{< figure src="/attachments/refguide/modeling/integration/import-a-large-excel-file/create-entity-using-excel-input.png" class="no-border" >}}

    You can change the name of the entity or change its persistence later, if necessary.
