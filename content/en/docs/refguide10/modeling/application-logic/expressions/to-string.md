---
title: "To String"
url: /refguide10/to-string/
weight: 130
---

## Introduction

Basic functions to convert values of various data types to string.

## `toString`

Converts the specified value to a string representation.

If you need full control over the output format, consider using the data type specific format functions. For example, for decimal, use [formatDecimal](/refguide10/parse-and-format-decimal-function-calls/).

### Input Parameters

The input parameters are described in the table below:

| Value                                         | Type                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| A value that should be converted to a string. | Integer/Long, Decimal, Date and time, Boolean and Enumeration.<br />In case of enumeration, the expression returns the key of the enumeration value, not the caption. More information, see [Enumerations in expressions](/refguide10/enumerations-in-expressions/). |

{{% alert color="info" %}}
If you pass an empty object or attribute to this function, the output will be an empty string.
{{% /alert %}}

### Example

If you use the following input:

```java
toString(1.4)
```

The output is:

```java
'1.4'
```

If you type in an input with a Date and time type:

```java
toString(dateTime(2007))
```

The output is:

```java
'Mon Jan 01 00:00:00 CET 2007'
```

If you type in an input with a Boolean:

```java
toString(true)
```

The output is:

```java
'true'
```
