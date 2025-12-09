---
title: "Parameter"
url: /refguide/page-parameter/
weight: 70
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Parameters are the means by which you pass data to your logic. When a page is loaded, the parameters are filled with the current values.

If you want to use an object or primitive in your page, use the parameter. In the image below, the parameter name is **CustomerName** is of **Type** `string`, is not **Required** and has a **Default value** shown in string empty.

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter.png" class="no-border" >}}

## Common Section

The **Documentation** property can be used to store developer documentation. This can be used to explain to other developers about the parameter. End-users will never see this documentation.

## Output Section

### Data Type

The data type of a parameter defines the type of the value that it expects. See [Data Types](/refguide/data-types/) for the possible data types.

Default: *Object*

### Argument {#argument}

Determines whether the argument is optional or required. When the argument is set to be **Optional**, it can be omitted when opening the page. When the argument is set to be **Required**, an argument must be passed.

#### Default Value

When an argument is set to **optional**, a default value can be set:

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-default-value.png" max-width=70% >}}

{{% alert color="info" %}}
 The default value is used when the argument is omitted, not when the argument value is `empty`.
{{% /alert %}}

### Name

**Name** refers to the name of the parameter.

### Variable Arguments

Variable arguments are used to pass parameters from the context to the page. This is done by selecting the desired `arguments` from the dropdown. **Optional** can be omitted by selecting `(None)` .

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-argument-variable.png" max-width=70% >}}

### Expression-Based Arguments (All Data Types)

Primitive values, such as strings, booleans, and enumerations, can be passed as expressions. This method allows users to use functions and follow associations within the expression to set the argument values. Using expressions for arguments provides flexibility in setting values and improves the functionality of pages. In the example below, the page has an parameter **AnimalName** which is populated by the **Name** member of the provided **Animal** object. 

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-expression.png" width="500px" >}}
