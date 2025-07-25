---
title: "Export Mappings"
url: /refguide/export-mappings/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

For an introduction to export mappings, refer to [Mapping Documents](/refguide/mapping-documents/).

## Obtaining Objects in Export Mappings

Figure 1 shows an example of an Export Mapping document in which two elements from a schema have been selected using the [Select Elements](/refguide/select--elements/) dialog. The entity Cheesecake (on the left) was dragged into the mapping to map to the Cheesecake element (on the right). The entity Topping was mapped to the Topping element.

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mappings/example-mapping-document.png" class="no-border" >}}

**Figure 1**

Having defined what entities map to which schema elements, you need to configure how the actual Mendix objects that are to be exported, should be obtained when the Export Mapping is invoked. The root level element (in this case Cheesecake) is the parameter for an Export Mapping and is therefore passed directly to the Export Mapping when it is invoked. How the other Mendix objects in the mapping should be obtained needs to be configured.

### Getting Objects from Parameter

When you have an entity at the top of the mapping, that entity becomes a parameter to the mapping. When you use the mapping, you have to pass an object of that type to it.

When the top element in the mapping is [optional](#optional), you can specify a different element to be the parameter to the mapping by selecting **From parameter** as the method to get the Mendix object.

### Getting Objects by Association

For child objects, it is possible to get the objects via an association with the parent object, as shown in Figure 1. In the example, the **Topping** objects that need to be exported will be fetched at runtime using the **Topping_Cheesecake** association. It is possible to edit the mapping element by double-clicking the **Topping** entity (left) or the **Topping** schema element (right). This window will be shown:

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mappings/topping-object.png" class="no-border" >}}

**Figure 2**

### Getting Objects by Using a Microflow

In this window, you can choose to either get the object by association with the parent (Figure 3) or by microflow (for details, see [Mapping Attributes in Export Mappings](#mapping-attributes)). If you choose to get the object by microflow, you can pass any of the parent objects to that microflow as arguments to help determine what object you should return. This is the window in which this is configured:

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mappings/object-by-microflow.png" class="no-border" >}}

**Figure 3**

When you choose to get an object by microflow, this is shown in the Export Mapping document:

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mappings/object-by-microflow-example.png" class="no-border" >}}

**Figure 4**

The user can also define what should be done when the chosen method to get the Mendix object (from parameter, by association, or by microflow) fails. The first option is to throw an error and abort the mapping. The microflow that called this mapping should then handle this error. Alternatively, if the minimum occurrence of the schema element that is being mapped to is zero, it is possible to skip the creation of the element. The export mapping will continue for the remainder of the elements.

## Mapping Attributes in Export Mappings {#mapping-attributes}

For each value element that the complex schema element encompasses, an attribute needs to be mapped from the entity. These properties are not applicable for choice or inheritance elements because they do not contain value elements. Configuring how to map the attributes is done in the window depicted in Figure 5, which is shown after double-clicking a specific mapping element.

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mappings/mapping-attributes.png" class="no-border" >}}

**Figure 5**

### Entity Mapping Properties

| Property | Description |
| --- | --- |
| Entity attribute | The attribute in the domain entity that should be mapped to the element. |
| Schema value element | The element that will be filled. |
| Occurrence | Displays how often the element may occur. This can be "0..1" or "1", depending on if it is required or not. If the value is empty and the minimum required occurrence of the element is 0 (as specified by the schema) the creation of the element will be skipped. In the case you want to never map a value to an optional element, disable it in the "Select elements..." dialog. |
| Convert Using (optional) | A microflow to convert the value before performing export. |
| Map attributes by name | When this button is clicked, an effort is made to match attributes by name. A dialog appears reporting what has been changed. |

## Optional Mapping Elements {#optional}

For some selected schemas, elements defining an entity is optional. This is the case when the schema element:

* does not contain any attributes
* has a maximum occurrence of 1 (maxOccurs="1")
* is not a choice element or contained by a choice element 
* is not an inheritance element or contained by an inheritance element. 

An example of this is shown in Figure 6.

{{< figure src="/attachments/refguide/modeling/integration/mapping-documents/export-mappings/optional-mapping.png" class="no-border" >}} 

**Figure 6**

When no object is defined for the optional mapping, the element will always be created.
