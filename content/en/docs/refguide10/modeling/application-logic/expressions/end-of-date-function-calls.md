---
title: "End-of Date Function Calls"
url: /refguide10/end-of-date-function-calls/
weight: 98
description: Describes end-of date function calls in Studio Pro expressions.
---

## Introduction

End-of date function calls calculate the end of the day, week, month, or year and return the value.

The first parameter can be an attribute of an entity of type **Date and time**, a variable of type **Date and time**, or a **Date and time** value created using a [Date Creation](/refguide10/date-creation/) function.

You can also calculate the end of a time period from the specified date. For more information, see [Begin-of Date Function Calls](/refguide10/begin-of-date-function-calls/).

## `endOfDay` {#endOfDay}

The `endOfDay` function calculates the end of the day compared to the initial date.

### Input Parameters

The input parameters are described in the table below:

| Value                                  | Type          |
| -------------------------------------- | ------------- |
| Initial date                           | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type          |
| ------------------------------------------------------------ | ------------- |
| A Date and time value that is the end of the day relative to the *initial date*. | Date and time |

### Example

```java
endOfDay(dateTime(2007, 2, 7, 1, 1))
```

The output is:

```java
"Wed Feb 07 23:59 CET 2007"
```

## `endOfWeek` {#endOfWeek}

The `endOfWeek` function calculates the end of the week compared to the initial date. The beginning and the end of the week are based on the user's locale. In the case of an anonymous user, the browser's locale is used instead.

### Input Parameters

The input parameters are described in the table below:

| Value                                  | Type          |
| -------------------------------------- | ------------- |
| Initial date                           | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type          |
| ------------------------------------------------------------ | ------------- |
| A Date and time value that is the end of the week relative to the *initial date*. | Date and time |

### Example

```java
endOfWeek(dateTime(2007, 2, 7, 1, 1, 1))
```

The output is:

```java
"Sat Feb 10 23:59 CET 2007"
```

## `endOfMonth` {#endOfMonth}

The `endOfMonth` function calculates the end of the month compared to the initial date.

### Input Parameters

The input parameters are described in the table below:

| Value                                  | Type          |
| -------------------------------------- | ------------- |
| Initial date                           | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type          |
| ------------------------------------------------------------ | ------------- |
| A Date and time value that is the end of the month relative to the *initial date*. | Date and time |

### Example

```java
endOfMonth(dateTime(2007, 2, 7, 1, 1, 1))
```

The output is:

```java
"Wed Feb 28 23:59 CET 2007"
```

## `endOfYear` {#endOfYear}

The `endOfYear` function calculates the end of the year compared to the initial date.

### Input Parameters

The input parameters are described in the table below:

| Value                                  | Type          |
| -------------------------------------- | ------------- |
| Initial date                           | Date and time |

### Output

The output is described in the table below:

| Value                                                        | Type          |
| ------------------------------------------------------------ | ------------- |
| A Date and time value that is the end of the year relative to the *initial date*. | Date and time |

### Example

```java
endOfYear(dateTime(2007, 2, 7, 1, 1, 1))
```

The output is:

```java
"Mon Dec 31 23:59 CET 2007"
```

## Read More

* [Date Creation](/refguide10/date-creation/)
* [Begin-of Date Function Calls](/refguide10/begin-of-date-function-calls/)
