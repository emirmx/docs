---
title: "Parameter"
url: /refguide/page-parameter/
weight: 70
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Parameters are the means by which you pass data to your page. When a page is loaded, the parameters are filled with the current values.

If you want to use an object or primitive in your page, use a parameter. In the image below, the parameter's name is **CustomerName** is of **Type** `string`, is not **Required** and has a **Default value** shown in string empty.

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter.png" class="no-border" >}}

## Output Section

### Name

**Name** refers to the name of the parameter.

### Data Type

The data type of a parameter defines the type of the value that it expects. Possible data types are **objects**, and **primitives** such as `Boolean`, `Date and time`, `Decimal`, `Enumeration`, `Integer/Long`, and `String`.

Default: *Object*

### Argument {#argument}

The page has a parameter, which can be made required or optional. If it is required it is mandatory to supply an argument for that parameter when opening a page.

#### Default Value

When an argument is set to **optional**, a default value can be set. When a default value is set if the parameter is omitted, the default value will be used. In the example below, if the parameter is omitted by the user an empty string `''` will be used for the parameter. 

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-default-value.png" max-width=70% >}}

{{% alert color="info" %}}
 The default value is used when the argument is omitted, not when the argument value is `empty`.
{{% /alert %}}

## Passing Arguments from a caller

When using an **Show page** action form a microflow, nanoflow or button, you can pass arguments in two primary ways, depending on the data type of the argument.

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-mappings.png" width="500px" >}}

### Variable Arguments

Variable arguments are used to pass parameters from the context to the page. This is done by selecting the desired `arguments` from the dropdown. **Optional** can be omitted by selecting `(None)` .

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-argument-variable.png" max-width=70% >}}

### Expression-Based Arguments (All Data Types)

Primitive values, such as strings, booleans, and enumerations, can be passed as expressions. This method allows users to use functions and follow associations within the expression to set the argument values. Using expressions for arguments provides flexibility in setting values and improves the functionality of pages. In the example below, the page has an parameter **AnimalName** which is populated by the **Name** member of the provided **Animal** object. 

{{< figure src="/attachments/refguide/modeling/pages/page/page-parameter-expression.png" width="500px" >}}
