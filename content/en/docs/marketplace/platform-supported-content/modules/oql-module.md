---
title: "OQL Module"
url: /appstore/modules/oql-module/
description: "Describes the configuration and usage of the OQL module, which is available in the Mendix Marketplace."
---

## Introduction

The [OQL Module](https://marketplace.mendix.com/link/component/66876) executes [OQL](https://docs.mendix.com/refguide/oql/) queries and returns the results as a list of Mendix objects, a count of results in the database or a CSV file.

### Typical Use Cases

This module allows users to run arbitrary OQL queries without having to write Java code.
It is also an alternative to [View Entities](/refguide/view-entities/) with support for running dynamic OQL queries and using OQL parameters.
Unless these features are necessary, it is recommended to use View Entities.

Alternatively, this module can be used to interactively investigate data problems in a running app.

{{% alert color="warning" %}}
The OQL Module does not validate the input query and does not apply user role access controls when executing queries.
Care should be taken to guarantee that the query is valid and does not expose app data.
{{% /alert %}}

### Features

* Execute an OQL query directly or load a query from a dataset
* Supports running queries with limit and offset
* Supports OQL named query parameters
* Option to return data directly or generate a CSV file
* Option to compress the generated CSV file in a Zip file
* Customize CSV files to use different characters for quoting strings, separating values, and escaping special characters
* Customize CSV files to remove new line sequences from string values

## Available OQL Actions

All actions support defining [Named Parameters](#named-parameters) for queries.
Only `SELECT` queries are supported and all queries are executed without access rules.

### ExecuteOQLStatement {#executeoqlstatement}

This action executes an OQL query, either directly or by loading one from a dataset. If you wish to use a dataset, you need to provide its fully qualified name (example "MyFirstModule.Dataset", instead of just "Dataset").

Returns: A list of Mendix objects of type `returnEntity` with each query column stored in an attribute or association of the same name. Note:
- Attributes and associations of the `returnEntity` that are not present in the query are ignored
- Only associations owned by the `returnEntity` are considered when setting associations

This action takes the following parameters:
* `statement`: OQL Query or fully qualified name of a Dataset (module and Dataset name) to be executed
* `returnEntity`: Entity type to be used to store results
* `amount`: If not `empty` or 0, will limit the number of results to at most this amount. It is recommended to use an explicit sorting order in the OQL query when using amount
* `offset`: If not `empty` or 0, will offset the results of the query by this quantity. It is recommended to use an explicit sorting order in the OQL query when using offset
* `preserveParameters`: If false, all parameters defined for this query will be cleared

### CountRowsOQLStatement {#countrowsoqlstatement}

Similar to `ExecuteOQLStatement` above, but returns only the number of results. Useful to obtain information from the database without the overhead of generating Mendix objects.

Returns: The number of results of executing `statement`

This action does not support datasets

[Named parameters](#named-parameters) are always reset on completion.

This action takes the following parameters:
* `statement`: OQL Query to be executed
* `amount`: Limits the number of results to at most this amount. If there are more results in the query than this limit, this amount is returned instead. If set to 0, no limit is applied
* `offset`: This parameter is ignored and might be removed in a future version

### ExportOQLToCSV {#exportoqltocsv}

This action executes an OQL query and saves the result in a CSV file.

Returns: An object of type `FileDocument` (or a specialization of it) containing the CSV file with the results of executing `statement`

[Named parameters](#named-parameters) are always reset on completion.

This action takes the following parameters:
* `statement`: OQL Query or fully qualified name of a Dataset (module and Dataset name) to be executed
* `returnEntity`: A FileDocument or one of its specializations where the resulting file will be stored
* `removeNewLinesFromValues`: Indicates if new lines inside string values should be replaced with spaces
* `zipResult`: Indicates if the resulting file should be compressed inside a ZIP file
* `exportHeaders`: Indicates if the first line of the result should contain a header with the names of each column
* `separatorChar`: Indicates what character should be used to separate columns in the result
* `quoteChar`: Indicates what character should be used to quote string values. May be left empty if `escapeChar` is defined
* `escapeChar`: Indicates what character should be used to escape spaces and other special characters if string values are unquoted. Only applicable if `escapeChar` is not defined

## Named Parameters {#named-parameters}

If you wish to use named parameters inside an OQL query, you must call the following actions to set their values before calling the actions above:
* `AddBooleanParameter`: For Boolean parameters
* `AddDateTimeParameter`: For Date and Time parameters
* `AddDecimalParameter`: For parameters of type Decimal
* `AddIntegerLongValue`: For parameters of type Integer/Long
* `AddObjectParameter`: For parameters that reference a Mendix Object
* `AddStringParameter`: For String parameters

If you wish to keep the same parameters across multiple calls to the [ExecuteOQLStatement](#executeoqlstatement) action, you must set `preserveParameters` to `true`. All other OQL actions above will clear all defined parameters after every call.
