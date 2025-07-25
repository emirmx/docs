---
title: "Relational Expressions"
url: /refguide10/relational-expressions/
weight: 30
description: "Describes relational expressions in Mendix."
---

## Introduction

Relational expressions allow users to compare values and to make changes and perform actions based upon that information. The return type of such expressions is always Boolean.

## Less Than ( `<` ) {#less-than}

This expression determines whether the first value is less than the second value.

### Input Parameters

Possible values are described in the table below: 

| Value      | Type                            |
| ---------- | ------------------------------- |
| Two values | String                          |
| Two values | Numeric (Integer/Long, Decimal) |
| Two values | Date and time                   |

{{% alert color="info" %}}
The two values should be in the same category (for example, if one is numeric, the other should be numeric too).
{{% /alert %}}

### Output

The output is described in the table below:

| Value              | Type    |
| ------------------ | ------- |
| `True` or `False`. | Boolean |

### Example

If you use the following input:

```java
4<3
```

the output is:

```java
False
```

## Greater Than ( `>` ) {#greater-than}

This expression determines whether the first value is greater than the second value.

### Input Parameters

Possible values are described in the table below: 

| Value      | Type                            |
| ---------- | ------------------------------- |
| Two values | String                          |
| Two values | Numeric (Integer/Long, Decimal) |
| Two values | Date and time                   |

{{% alert color="info" %}}
The two values should be in the same category (for example, if one is numeric, the other should be numeric too).
{{% /alert %}}
{{% alert color="warning" %}}
An error is raised if at least one value is `empty`.
{{% /alert %}}

### Output

The output is described in the table below:

| Value              | Type    |
| ------------------ | ------- |
| `True` or `False`. | Boolean |

### Example

If you use the following input:

```java
4>3
```

the output is:

```java
True
```

## Less Than or Equal To ( `<=` ) {#less-than-or-equal-to}

This expression determines whether the first value is less than or equal to the second value.

### Input Parameters

Possible values are described in the table below: 

| Value      | Type                            |
| ---------- | ------------------------------- |
| Two values | String                          |
| Two values | Numeric (Integer/Long, Decimal) |
| Two values | Date and time                   |

{{% alert color="info" %}}
The two values should be in the same category (for example, if one is numeric, the other should be numeric too).
{{% /alert %}}
{{% alert color="warning" %}}
An error is raised if at least one value is `empty`.
{{% /alert %}}

### Output

The output is described in the table below:

| Value              | Type    |
| ------------------ | ------- |
| `True` or `False`. | Boolean |

### Examples

The examples below illustrate which value the expression returns:

* If you use the following input:

    ```java
    6<=3
    ```

    the output is:

    ```java
    False
    ```

* If you use the following input:

    ```java
    3<=3
    ```

    the output is:

    ```java
    True
    ```

## Greater Than or Equal To ( `>=` ) {#greater-than-or-equal-to}

Determines whether the first value is greater than or equal to the second.

### Input Parameters

Possible values are described in the table below: 

| Value      | Type                            |
| ---------- | ------------------------------- |
| Two values | String                          |
| Two values | Numeric (Integer/Long, Decimal) |
| Two values | Date and time                   |

{{% alert color="info" %}}
The two values should be in the same category (for example, if one is numeric, the other should be numeric too).
{{% /alert %}}
{{% alert color="warning" %}}
An error is raised if at least one value is `empty`.
{{% /alert %}}

### Output

The output is described in the table below:

| Value              | Type    |
| ------------------ | ------- |
| `True` or `False`. | Boolean |

### Example

If you use the following input:

```java
4>=3
```

the output is:

```java
True
```

## Equal To ( `=` ) {#equal-to}

This expression determines whether the two values are equal.

### Input Parameters

Possible values are described in the table below: 

| Value      | Type                                                         |
| ---------- | ------------------------------------------------------------ |
| Two values | String                                                       |
| Two values | Numeric (Integer/Long, Decimal)                              |
| Two values | Date and time                                                |
| Two values | Domain entity; equality is checked based on the ID of the object |

{{% alert color="info" %}}
The two values should be in the same category (for example, if one is numeric, the other should be numeric too).
{{% /alert %}}
{{% alert color="info" %}}
A comparison with an empty value evaluates as `true` if and only if the other value is `empty` or is an empty list.
{{% /alert %}}

### Output

The output is described in the table below:

| Value              | Type    |
| ------------------ | ------- |
| `True` or `False`. | Boolean |

### Examples

The examples below illustrate which value the expression returns:

* If you use the following input:

    ```java
    "mystring" = "myotherstring"
    ```

    the output is:

    ```java
    False
    ```

* If you use the following input:

    ```java
    dateTime(2007) = dateTime(2007)
    ```

    the output is:

    ```java
    True
    ```

## Not Equal To ( `!=` ) {#not-equal-to}

Determines whether the two values are not equal.

### Input Parameters

The values be any of the following types, but the two values should be in the same category (for example, both numbers):

Possible values are described in the table below: 

| Value      | Type                                                      |
| ---------- | --------------------------------------------------------- |
| Two values | String                                                    |
| Two values | Numeric (Integer/Long, Decimal)                           |
| Two values | Date and time                                             |
| Two values | Object; equality is checked based on the ID of the object |

{{% alert color="info" %}}
The two values should be in the same category (for example, if one is numeric, the other should be numeric too).
{{% /alert %}}
{{% alert color="info" %}}
A comparison with an empty value evaluates as `true` if and only if the other value is `empty` or is an empty list.
{{% /alert %}}

### Output 

The output is described in the table below:

| Value              | Type    |
| ------------------ | ------- |
| `True` or `False`. | Boolean |

### Example

If you use the following input:

```java
"mystring" != "mystring"
```

the output is:

```java
False
```
