---
title: "Date Creation"
url: /refguide10/date-creation/
weight: 90
---

## Introduction

Dates can be created with the `dateTime` and `dateTimeUTC` functions. The difference between them is that  `dateTime` uses the calendar of the session used in the function call, and `dateTimeUTC` uses the UTC calendar. The system session runs as UTC by default, except for scheduled events, which can be configured in the [Scheduled Event Time Zone](/refguide10/app-settings/#scheduled) section of **App Settings**.

{{% alert color="info" %}}
Do not use `dateTimeUTC` in client-side expressions (for example, in nanoflows) if you want to assign the output to (or compare the output with) an attribute of type **Date and time** where **Localize** is disabled. In the client, the localization functionality is built into the attribute type itself, and using UTC functions causes the time zone conversion to be handled twice.
{{% /alert %}}

This function does not accept variable or attribute parameters, only fixed values. To create a date using parameters, use the [parseDateTime](/refguide10/parse-and-format-date-function-calls/#parseDateTime) function.

## Values

These functions take between one and six input values in the following order:

1. years (type: integer, four digits and greater than 1799)
2. months (type: integer, between 1 and 12)
3. days (type: integer, between 1 and 31)
4. hours (type: integer, between 0 and 23)
5. minutes (type: integer, between 0 and 59)
6. seconds (type: integer, between 0 and 59)

## Examples

The examples below illustrate which value the expression returns:

* If you specify one value as an input: 

    ```java
    dateTime(2007)
    ```

    The expression will return the following output:

    ```java
    "Mon Jan 01 00:00:00 CET 2007"
    ```

* If you specify two values as an input: 

    ```java
    dateTime(2007, 1)
    ```

    The expression will return the following output:

    ```java
    "Mon Jan 01 00:00:00 CET 2007"
    ```

* If you specify three values as an input: 

    ```java
    dateTime(2007, 1, 1)
    ```

    The expression will return the following output:

    ```java
    "Mon Jan 01 00:00:00 CET 2007"
    ```

* If you specify four values as an input: 

    ```java
    dateTime(2007, 1, 1, 1)
    ```

    The expression will return the following output:

    ```java
    "Mon Jan 01 01:00:00 CET 2007"
    ```

* If you specify five values as an input: 

    ```java
    dateTime(2007, 1, 1, 1, 1)
    ```

    The expression will return the following output:

    ```java
    "Mon Jan 01 01:01:00 CET 2007"
    ```

* If you specify six values as an input: 

    ```java
    dateTime(2007, 1, 1, 1, 1, 1)
    ```

    The expression will return the following output:

    ```java
    "Mon Jan 01 01:01:01 CET 2007"
    ```
