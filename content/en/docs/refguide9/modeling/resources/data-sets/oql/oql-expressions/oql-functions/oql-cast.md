---
title: "OQL CAST"
url: /refguide9/oql-cast/
---

## Description

The `CAST` function converts an expression to a specific data type.

## Syntax

The syntax is as follows:

```sql
CAST ( expression AS data_type )
```

### expression

`expression` specifies the expression to convert.

### data_type

`data_type` specifies the data type to convert the expression to. The data type can be one of the following:

* `BOOLEAN`
* `DATETIME`
* `DECIMAL`
* `INTEGER`
* `LONG`
* `STRING`

## Supported Conversions

The table below describes which `CAST` conversions are supported:

* ✔ – the conversion is supported
* ✔* – the conversion is supported, but the behavior differs per database
* ✘ – the conversion is not supported

| From \ To | BOOLEAN | DATETIME | DECIMAL | INTEGER | LONG | STRING (unlimited) | STRING (limited) |
|------| :------: | :------: | :------: | :------: | :------: | :------: | :------: |
| BOOLEAN | ✔ | ✘ | ✘ | ✘ | ✘ | ✔* | ✔*¹ |
| DATETIME | ✘ | ✔ | ✘ | ✘ | ✘ | ✔* | ✔*² |
| DECIMAL | ✘ | ✘ | ✔* | ✔* | ✔* | ✔* | ✔*² |
| INTEGER | ✘ | ✘ | ✔ | ✔ | ✔ | ✔ | ✔ |
| LONG | ✘ | ✘ | ✔ | ✔ | ✔ | ✔ | ✔ |
| STRING | ✘ | ✘ | ✔ | ✔ | ✔ | ✔ | ✔ |

<small>[1] BOOLEAN to STRING (limited) is supported only if the resulting string length is ≥ 5. <br />[2] The conversion of DATETIME and DECIMAL to STRING (limited) is supported only if the value fully fits into the string length. The conversion can fail if the resulting string length is < 20.</small>

## Examples

A frequent use case for `CAST` is to convert your date from the `DATETIME` data type to a more readable `STRING` type: 

```sql
CAST ( your_datetime_variable AS string )
```
