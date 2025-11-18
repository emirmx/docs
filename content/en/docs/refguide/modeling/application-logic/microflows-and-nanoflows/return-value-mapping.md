---
title: "Return Value Mapping"
url: /refguide/return-value-mapping/
weight: 60
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Pages and snippets can use [return values](/refguide/end-event/#return-value) when calling microflows or nanoflows by specifying a return value mapping. This feature enhances microflow and nanoflow reusability by allowing pages to update local values based on flow return values without needing to pass objects containing those attributes to the flow. 

Return values can be mapped to available variables on the page or snippet. Both primitive and object return value types are supported. Using an expression, the return value can be transformed as needed before assignment.

{{< figure src="/attachments/refguide/modeling/application-logic/microflows-and-nanoflows/parameter/return-value-mapping-list.png" width="500px" class="no-border" >}}

## Primitive Return Values

For primitive return values (such as strings, booleans, integers, decimals, or enumerations), the value can be assigned to any matching attribute of available objects on the page, or to variables and parameters of pages or snippets. The target must have a compatible data type with the return value.

## Object Return Values

When a microflow or nanoflow returns an object, you can specify a return value mapping using one or more attributes of that object. This capability makes it possible to dynamically update multiple attributes, variables, and parameters with the return value of a single flow.

To use multiple attributes from a returned object, add more than one return value mapping to the same flow call. This is particularly useful when assigning multiple computed values to different variables from the same return value.

## Mapping Return Values from a Page

When calling a microflow or nanoflow action, you can map return values in two primary ways.

### Variable Return Value Mappings 

Variable return value mappings are used to map values returned by the microflow or nanoflow directly. When the flow returns an object, you can select specific attributes from that object to assign to variables on the page or snippet.

{{< figure src="/attachments/refguide/modeling/application-logic/microflows-and-nanoflows/parameter/variable-return-value-mapping.png" width="500px" class="no-border" >}}

### Expression-Based Return Value Mappings 

Both primitive values and object attributes can be mapped using expressions. This method allows you to use functions within the expression to modify the return value before it is assigned.

{{< figure src="/attachments/refguide/modeling/application-logic/microflows-and-nanoflows/parameter/expression-return-value-mapping.png" width="500px" class="no-border" >}}