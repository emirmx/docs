---
title: "Parse and Format Date Function Calls"
url: /refguide/parse-and-format-date-function-calls/
weight: 160
description: "Describes the functions for parsing Date and time values from strings using a specified pattern or producing a string from a Date and time value in Mendix."
---

## Introduction 

This document describes functions that are used to parse Date and time values from strings using a specified pattern, or to produce a string from a Date and time value.

The following pattern letters can be used to parse and format Date and time values:

| Letter | Date or Time Component                           | Examples                  |
| ------ | ------------------------------------------------ | ------------------------- |
| `M`      | Month in year, digit                             | 1                         |
| `MM`     | Month in year, digit with leading zero           | 01                        |
| `MMM`    | Month in year, abbreviated (context sensitive)   | Nov                       |
| `MMMM`   | Month in year (context sensitive)                | November                  |
| `L`      | Month in year, digit (standalone), digit         | 1                         |
| `LL`     | Month in year, digit with leading zero           | 01                        |
| `LLL`    | Month in year, abbreviated (standalone)          | Nov                       |
| `LLLL`   | Month in year (standalone)                       | November                  |
| `yy`     | Year, two digits                                 | 01                        |
| `yyyy`   | Year, four digits                                | 2001                      |
| `G`      | Era designator                                   | AD                        |
| `E`      | Day name in week, abbreviated                    | Tue                       |
| `EEEE`   | Day name in week                                 | Tuesday                   |
| `u`      | Day of week (1 = Monday, ..., 7 = Sunday)        | 5                         |
| `w`      | Week in year                                     | 11                        |
| `W`      | Week in month                                    | 2                         |
| `D`      | Day in year                                      | 133                       |
| `d`      | Day in month                                     | 7                         |
| `F`      | Day of week in month                             | 1                         |
| `a`      | Am/pm marker                                     | PM                        |
| `H`      | Hour in day (0-23)                               | 0                         |
| `k`      | Hour in day (1-24)                               | 24                        |
| `K`      | Hour in am/pm (0-11)                             | 0                         |
| `h`      | Hour in am/pm (1-12)                             | 12                        |
| `m`      | Minute in hour                                   | 24                        |
| `s`      | Second in minute                                 | 50                        |
| `S`      | Millisecond                                      | 201                       |

{{% alert color="warning" %}}
Prior to Mendix 11, the `MMM` and `MMMM` tokens were not properly supported in nanoflows for some languages.
{{% /alert %}}

{{% alert color="info" %}}
Here are some examples of using `LLLL`, `MMMM`, `LLL`, and `MMM` in languages that support the genitive case:

* Ukrainian:
    * `LLLL` returns `квітень`
    * `MMMM` returns `квітня`
    * `LLL` returns `квіт.`
    * `MMM` returns `квіт.`
* Polish:
    * `LLLL` returns `kwiecień`
    * `MMMM` returns `kwietnia`
    * `LLL` returns `kwi`
    * `MMM` returns `kwi`
{{% /alert %}}

The following pattern letters are only available for microflows:

| Letter | Date or Time Component                    | Examples                              |
| ------ | ----------------------------------------- | ------------------------------------- |
| `z`      | Time zone                                 | Pacific Standard Time; PST; GMT-08:00 |
| `Z`      | Time zone                                 | -0800                                 |
| `X`      | Time zone                                 | -08; -0800; -08:00                    |

{{% alert color="info" %}}
For some parse and format functions, there are UTC variants. Do not use these UTC variants (for example, `parseDateTimeUTC`) in client-side expressions if you want to assign the output to (or compare the output with) an attribute of type **Date and time** where **Localize** is disabled. In the client, the localization functionality is built into the attribute type itself, and using UTC functions causes the time zone conversion to be handled twice.
{{% /alert %}}

## `parseDateTime[UTC]` {#parseDateTime}

Takes a string and parses it. If it fails and a default value is specified, it returns the default value. Otherwise, an error occurs. The function `parseDateTime` uses the user's time zone and `parseDateTimeUTC` uses the UTC calendar.

{{% alert color="info" %}}
When using `yy` date format in microflows, the century guessing by proximity follows the rule of **80/20**. Specifically, it adjusts dates to be within 80 years before and 20 years after the time the date format instance is created:

* `25` {{< icon name="arrow-narrow-right" >}} `2025`
* `68` {{< icon name="arrow-narrow-right" >}} `1968`
  
When using it in nanoflows, it follows the rule of **50/50**:

* `25` {{< icon name="arrow-narrow-right" >}} `2025`
* `88` {{< icon name="arrow-narrow-right" >}} `1988`
  
{{% /alert %}}

### Input Parameters

The input parameters are described in the table below:

| Value                        | Type                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| Date                         | A string which contains the textual representation of a date — for example `dd/MM/yyyy` or `MM/dd/yyyy` |
| Format                       | String                                                       |
| Default value (**optional**) | Date and time                                                |

### Output

The output is described in the table below:

| Value                                                        | Type          |
| ------------------------------------------------------------ | ------------- |
| The parsed date or the default value if a date could not be parsed. | Date and time |

{{% alert color="info" %}}
If the `Date` string is date-like, but not a valid date, the function will be able to parse it and will return a valid `Date and time` value.

For example `parseDateTime('35-11-2015', 'dd-MM-yyyy', dateTime(2015))` will return `05 December 2015 12:00 AM`.
{{% /alert %}}

### Example

The examples below illustrate which value the expression returns:

* If you use the following input:

    ```java
    parseDateTime('2022-04-30T22:00:00.000', 'yyyy-MM-dd''T''HH:mm:ss.SSS')
    ```

    the output is:

    ```java
    Apr 30 2022 22:00:00
    ```

    The time will be 00:00, if it is not specified.
    
* If you use the following input:

    ```java
    parseDateTime('noDateTime', 'dd-MM-yyyy', dateTime(2007))
    ```

    the output is:

    ```java
    Mon Jan 01 00:00:00 CET 2007
    ```

## `formatDateTime[UTC]` {#formatDateTime}

Converts the Date and time value to a string, formatted according to the format parameter. Without the format parameter, a standard format is used, which depends on the [Java version](/refguide/java-version-migration/#date-locale-dutch) and user locale. The function `formatDateTime` uses the users calendar and `formatDateTimeUTC` uses the UTC calendar.

### Input Parameters

The input parameters are described in the table below:

| Value                 | Type          |
| --------------------- | ------------- |
| Date                  | Date and time |
| Format (**optional**) | String        |

### Output

The output is described in the table below:

| Value                                       | Type   |
| ------------------------------------------- | ------ |
| A formatted representation of the Date and time value. | String |

### Example

If you use the following input:

```java
formatDateTime($object/Date1,'EEE, d MMM yyyy HH:mm:ss Z')
```

the output is:

```java
'Sun, 8 Jun 2008 10:12:01 +0200'
```

To get a format like `'2008-06-08T10:12:01'`, you need to concatenate two formatDateTime[UTC] functions:

```java
formatDateTime($object/Date1,'yyyy-MM-dd') + 'T' + formatDateTime($object/Date1,'HH:mm:ss')
```

## `formatTime[UTC]` {#formatTime}

Converts the time part of Date and time value to a string in a standard format, which depends on the Java version and user locale. `formatTime` uses the users calendar and `formatTimeUTC` uses the UTC calendar.

### Input Parameters

The input parameters are described in the table below:

| Value | Type          |
| ----- | ------------- |
| Date  | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type   |
| ------------------------------------------------------------ | ------ |
| A formatted representation of the time part of the Date and time value. | String |

### Example

If you use the following input:

```java
formatTime(dateTime(1974, 7, 2, 9, 50, 10))
```

the output is:

```java
'9:50 AM'
```

## `formatDate[UTC]` {#formatDate}

Converts the date part of Date and time value to a string in a standard format, which depends on the [Java version](/refguide/java-version-migration/#date-locale-dutch) and user locale. `formatDate` uses the users calendar and `formatDateUTC` uses the UTC calendar.

### Input Parameters

The input parameters are described in the table below:

| Value | Type          |
| ----- | ------------- |
| Date  | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type   |
| ------------------------------------------------------------ | ------ |
| A formatted representation of the date part of the Date and time value. | String |

### Example

If you use the following input:

```java
formatDate(dateTime(1974, 7, 2, 9, 50, 10))
```

the output is:

```java
'7/2/74'
```

## `dateTimeToEpoch` {#dateTimeToEpoch}

Returns the number of milliseconds since January 1, 1970, 00:00:00 GMT to the date.

### Input Parameters

The input parameters are described in the table below:

| Value | Type          |
| ----- | ------------- |
| Date  | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type   |
| ------------------------------------------------------------ | ------ |
| The number of milliseconds since January 1, 1970, 00:00:00 GMT to the date. | Integer/Long |

### Example

If you use the following input:

```java
dateTimeToEpoch(dateTime(1974, 7, 2, 9, 50, 10))
```

The output is:

```java
141990610000
```

## `epochToDateTime` {#epochToDateTime}

Creates a Date and time that represents the specified number of milliseconds since January 1, 1970, 00:00:00 GMT.

### Input Parameters

The input parameters are described in the table below:

| Value | Type          |
| ----- | ------------- |
| Epoch | Integer/Long |

### Output

The output is described in the table below:

| Value                                                        | Type   |
| ------------------------------------------------------------ | ------ |
| A Date and time that represents the specified number of milliseconds since January 1, 1970, 00:00:00 GMT. | Date and time |

### Example

If you use the following input:

```java
epochToDateTime(141990610000)
```

The output is:

```java
dateTime(1974, 7, 2, 9, 50, 10)
```
