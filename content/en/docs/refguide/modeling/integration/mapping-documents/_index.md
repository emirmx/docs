---
title: "Mapping Documents"
url: /refguide/mapping-documents/
weight: 20
description: "Overview of how to use import and export mapping documents in Studio Pro."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Mapping documents are used to define how Mendix objects can be converted to and from XML or JSON, according to a specific XML schema or JSON structure. There are two types of mappings: [Import Mappings](/refguide/import-mappings/) and [Export Mappings](/refguide/export-mappings/).

## Import Mappings {#import-mappings}

Import mappings are used to define how incoming XML or JSON is converted into Mendix objects, according to a specific XML schema or JSON structure. This is needed to be able to interpret data received from other systems. In Mendix, there are three basic scenarios for creating Mendix objects from XML or JSON:

* Receiving XML from a web service (defined in an [imported web service](/refguide/consumed-web-service/)) in a [call web service](/refguide/call-web-service-action/) activity.
* Receiving XML or JSON from a call action [call REST service](/refguide/call-rest-action/).
* Importing XML or JSON directly in an [import from mapping](/refguide/import-mapping-action/) action. 

This is an example of an import mapping document that maps an **Order** from a web service to a **ReceivedOrder** entity:

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/import-mapping.png" class="no-border" >}}

For more details, see [Import Mappings](/refguide/import-mappings/).

## Export Mappings

Export mappings are used to define how Mendix objects can be converted to XML according to a specific XML schema. This is needed to be able to send data to other systems in a format the other system can process. In Mendix, there are two basic scenarios for converting Mendix objects to XML:

* Sending XML to a web service (defined in an [imported web service](/refguide/consumed-web-service/)) in a [call web service](/refguide/call-web-service-action/) activity.
* Exporting XML directly in an [export with mapping](/refguide/export-mapping-action/) action. 

This is an example of an export mapping document:

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mapping.png" class="no-border" >}}

In this example, a **Cheesecake** entity will be passed when the export mapping is called. Subsequently, the **Topping** entities will be fetched by following the **Topping_Cheesecake** association from the passed Cheesecake Mendix object. The result is passed to an XML document or sent to a web service.

For more details, see [Export Mappings](/refguide/export-mappings/).

## Creating a New Mapping Document

To specify an import or export mapping, the user must:

1. Create a new **Import Mapping** or **Export Mapping** document.
2. Click **Select elements...** to select an XML schema, imported web service document, or JSON structure document as the source schema for this mapping.

    If the schema is large, a subset of elements can be selected so you don't have to map all of them. This is explained in more detail in the [Select Elements](/refguide/select--elements/) section.

3. Click **OK**. A structure is created with placeholders for entities on the left, and the selected XSD elements on the right.

    It is possible to include a parameter entity in mapping documents. Mapping documents with a parameter entity need to be invoked (in a [call web service](/refguide/call-web-service-action/) or [export with mapping](/refguide/export-mapping-action/) activity) by passing a parameter of the configured type. Mapping documents without a parameter entity can be invoked without passing a parameter. For import mappings, a parameter entity can be included by dragging one onto the placeholder in the created structure using the Connector tool. Export mappings always need to have a parameter entity (the object that is being exported) and the mapped root element is used for this. In both cases, the parameter entity is depicted as a yellow arrow parameter symbol.

4. Map the child elements of the schema. Entities can be obtained in four ways:

    * From a mapping parameter
    * By associations
    * From a custom microflow
    * By entity specializations in the case of choice or inheritance XML elements

5. Configure how entity attributes should be transformed into the XML or JSON structure.

## Convenience Functions

* Map automatically: Automatically look for entities and associations in the domain model that can be used in the mapping. If a matching entity or association is not found, it is created in the domain model. This function is explained in more detail in the [Map Automatically](/refguide/map-automatically/) section.
* Clear mappings: Disconnects all mapping elements in the document from entities and associations. They are not deleted in the domain model.

## Tip: Important Windows

Mapping documents rely heavily on two windows. If they are not visible in Studio Pro, they can be enabled from the **View menu**.

* **Properties window**. Details of individual mapping elements are shown here.
* **Connector window**. Entities from the connector window are dragged into the mapping document.

## User Interactions

* Dragging entities. Drag an entity from the Connector window and drop it on an entity placeholder (dashed box) to relate it to an XML element. 
* Double-clicking on a mapping element (on either the entity side or the XML side). Opens a dialog that allows you to specify details for the individual mapping. If the element is not yet related to an entity, a dialog appears that lets you select the entity first.
* Right-click > **Select entity**. Change the entity that is coupled to an XML element.
* Right-click > **Go to entity**. Open the domain model that contains the entity and jumps to the entity.
* Right-click > **Collapse All**. Hides all the children of the mapping element so you can keep overview in large mappings.
* Right-click > **Expand All**. Expands all children of the mapping element; all underlying elements become visible.
* Left-click > "-" icon (below mapping element). Makes underlying elements invisible.
* Left-click > "+" icon (below mapping element). Makes underlying elements visible.

## Properties

| Property | Description |
| --- | --- |
| Name | The name of the mapping. |
| Documentation | You can add information to explain what the mapping does here. |
| Web Service Operation Properties | This category is only filled in if the mapping is for a web service call and not for an XML Schema. |
| Web Service | The name of the imported web service that this mapping is for. |
| Service name | The actual name of the service, as it is defined in the WSDL of the imported service. |
| Operation name | The name of the specific operation in the service that this mapping is meant for. |
| Request part | Only applicable for Export Mappings. The name of the header or body parameter that this mapping is for. If the mapping is for all body parameters, the value is **Body**. |
| XML Schema Properties | This category is only filled in if the mapping is for an XML schema and not for a web service call. |
| XML Schema | The name of the XML Schema that this mapping is for. |
| Start at | Determines which part of the XML structure this mapping defines. |
| Send empty values | Only applicable for export mappings. If a mapping element is optional and nillable, you need to select whether or not to send the empty values. The default is **Do not send the empty values**. |
