---
title: "Export XML Documents"
url: /refguide/export-xml-documents/
weight: 4
description: "Describes how to add an XML schema, create domain-to-XML mapping, and export logic."
aliases: /howto/integration/export-xml-documents/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

In enterprise software, it is not likely that you work in a [greenfield](https://en.wikipedia.org/wiki/Greenfield_project). In almost every situation, you will need to integrate with existing systems. Mendix supports many ways of integration, but this how-to focuses on how you can export XML documents.

This how-to teaches you how to do the following:

* Add an XML schema
* Create domain-to-XML mapping and export logic

## Prerequisites

Before you can start exporting XML documents, you need data in your application that is used during the export. You first need to set up the data structure and GUI to maintain the customer data. Then, you'll create the actual export logic and the corresponding export mapping. To do this, you need to know how to do the following:

* Create a domain model (for more information, see [Configuring a Domain Model](/refguide/configuring-a-domain-model/))
* Create a custom file document (for more information, see [File Manager](/refguide/file-manager/))
* Create overview and detail pages (for more information, see [How to Create Your First Two Overview and Detail Pages](/howto/front-end/create-your-first-two-overview-and-detail-pages/))
* Create menu items, (for more information, see [Setting Up Navigation](/refguide/setting-up-the-navigation-structure/))

Before starting this how-to, make sure you have completed the following prerequisites:

1. Create the following **Customer** entity in your domain model:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/customer-entity.png" class="no-border" >}}

2. Create overview and detail pages to manage the Customer objects.
3. Create a menu item to access the customer overview page.
4. Create the **XMLDocument** entity that inherits all the properties from *System.FileDocument*:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/xmldocument-entity.png" class="no-border" >}}

5. Create a reference set (multiplicity **[*-*]**) between XMLDocument and Customer:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/reference-set.png" class="no-border" >}}

## Adding an XML Schema (XSD)

Whether you plan to import documents or export documents, working with XML means that your application must contain an XML schema (also called XSD). An XSD describes the possible contents of an XML file. Based on this XSD, your application knows how to read or write an XML file. If you don't have an XSD file, there are a couple of online XSD generators that accept an XML document as input. For this how-to, you can use [Customers.xsd](/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/customer-schema.xsd).

1. Right-click your module in the **App Explorer** and select **Add other** > **XML schema**.
2. Enter *CustomersXSD* for the **Name** and click **OK**:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/name-schema.png" class="no-border" >}}

3. In the **XML Schema** editor, click **Select** for **XML Schema** and select the XSD file that you downloaded earlier:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/schema-editor.png" class="no-border" >}}

4. Click **OK** to save the XML Schema. We'll be using this schema in the following steps.

## Creating Domain-to-XML mapping

The XML schema describes what the contents of an XML document should be. We need to create domain-to-XML mapping to define how the data in our application is transformed into a XML document.

1. Right-click your module in the **App Explorer** and select **Add other** > **Export mapping**.
2. Enter *ExportCustomersMapping* for the **Name**:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/name-mapping.png" class="no-border" >}}

3. Click **OK**, and the **Select schema elements for export mapping** dialog box will automatically open. Now do the following:<br />
    1. For **Schema source**, select **XML schema**.<br />
    1. For the schema, select the previously added **CustomersXSD**.<br />
    1. In the **Schema elements** section of the dialog box, click the **Expand all** and **Check all** buttons. This automatically selects the **Customer** element and its child elements. Your screen should now look like this:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/select-schema-elements.png" class="no-border" >}}

4. Click **OK**. You should now see the first part of the import mappings:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/im-part-1.png" class="no-border" >}}

5. Open the **Connector** pane and drag the **XMLDocument** entity from the **Connector** into the placeholder:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/connector-pane.png" class="no-border" >}}

    The mapping editor for this element will pop up which can be closed by clicking **OK**.

6. Drag the **Customer** entity from the **Connector** into the placeholder:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/drag-entity.png" class="no-border" >}}

    The mapping editor for this element will open up:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/mapping-editor.png" class="no-border" >}}

7. In the mapping editor, verify the following:<br />
    * **Method** is set to **By association**<br />
    * **Association to parent** is set to **XMLDocument_Customer**<br />
8. Select attributes for all five **Attribute to value element mapping** instances (or click **Map attributes by name**). You should have the following mapping:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/select-attributes.png" class="no-border" >}}

9. Click **OK** to save the mapping.

## Creating the Export Logic

This section explains how you can create logic to export the customers stored in your application to an XML document.

To create the export logic, follow these steps:

1. Open the **Customer** overview page, right-click the toolbar of the data grid widget, and select **Add button** > **Action** to add a new Action button:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/add-action-button.png" class="no-border" >}}

2. Double-click the new button to open the properties editor and do the following:
    * For **Caption**, enter *Export as XML*
    * For **On click**, select **Call a microflow**
    * In the **Select Microflow** dialog box, click **New** to create a new microflow and enter *Customers_Export* for its **Name**

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/action-button-editor.png" class="no-border" >}}

3. Click **OK** to save the button properties.
4. Right-click the new action button and click **Go to microflow** in the context menu. You should see an empty microflow with one input parameter:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/parameter.png" class="no-border" >}}

5. Select the input parameter and delete it.
6. Open the **Toolbox**, which should be on the lower-right side of Studio Pro (you can also open it from the **View** menu).
7. Drag a **Retrieve** activity from the **Toolbox** to the line between the start event and end event.
8. Double-click the activity to open the **Retrieve Objects** properties editor and do the following:
    * For **Source**, select **From database**
    * For **Entity**, click **Select...** and select the customer entity

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/retrieve-objects.png" class="no-border" >}}

9. Click **OK**. The microflow should now look like this:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/microflow.png" class="no-border" >}}

10. Drag a **Create object** activity from the **Toolbox** to the line between the start event and end event.
11. Double-click the activity to open the **Create Object** editor and do the following:
    * For **Entity**, select **XMLDocument**
    * Click **New** to add a change item

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/create-object-editor.png" class="no-border" >}}

12. In the **Edit Change Item** editor, do the following:
    * For the **Member**, for the change item, select the **XMLDocument_Customer** reference:
    * For the **Value**, enter `$CustomerList`

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/edit-changed-item.png" class="no-border" >}}

13. Click **OK** to save the change item.
14. Create a change item to set the **Name** attribute to *'customers.xml'* (including the single quotation marks [']). The **Create Object** dialog box should now look like this:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/create-object.png" class="no-border" >}}

15. Click **OK** to save the action properties. The microflow should look like this:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/microflow-final.png" class="no-border" >}}

16. Drag an **Export with mapping** activity from the **Toolbox** to the line between the start event and end event. This inserts a new export XML activity.
17. Double-click the new activity to open the properties editor and do the following:
    * For **Mapping**, select the previously created **ExportCustomersMapping** XML-to-domain mapping
    * For **Parameter type**, verify that the entity **XMLDocument** is automatically selected
    * For the **Parameter**, select the created **NewXMLDocument**
    * For the output **Name**, select the created **NewXMLDocument**

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/export-with-mapping-dialog.png" class="no-border" >}}

18. Click **OK** to save the properties. The microflow should look like this:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/save-microflow.png" class="no-border" >}}

19. Drag a **Download file** activity from the **Toolbox** to the line between the start event and end event.
20. Double-click the activity to open the **Download File** dialog box and select **NewXMLDocument** as the **File document**:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/download-file.png" class="no-border" >}}

21. Click **OK**. The microflow should now look like this:

    {{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-xml-documents/edited-microflow.png" class="no-border" >}}

22. Deploy the application and open the customer overview page.
23. Click the **Export as XML** button and download the generated XML document.

## Read More

* [Consume a Complex Web Service](/howto/integration/consume-a-complex-web-service/)
* [Consume a Simple Web Service](/howto/integration/consume-a-simple-web-service/)
* [Import Excel Documents](/howto/integration/importing-excel-documents/)
* [Expose a Web Service](/howto/integration/expose-a-web-service/)
* [Enable Selenium Support](/howto/integration/selenium-support/)
* [Import XML Documents](/howto/integration/importing-xml-documents/)
* [Consume a REST Service](/howto/integration/consume-a-rest-service/)
* [Expose Data to BI Tools Using OData](/howto/integration/exposing-data-to-bi-tools-using-odata/)
