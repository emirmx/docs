---
title: "OQL Clauses"
url: /refguide/oql-clauses/
weight: 10
aliases:
    - /refguide/oql-from-clause/
    - /refguide/oql-group-by-clause/
    - /refguide/oql-limit-clause/
    - /refguide/oql-order-by-clause/
    - /refguide/oql-select-clause/
    - /refguide/oql-subqueries/
    - /refguide/oql-where-clause/
---

## Introduction

OQL clauses are reserved words that structure an OQL query into sections. Each section, and therefore clause, is responsible for a different aspect of the query. Every query has two mandatory clauses: `SELECT` and `FROM`.

For example:

```sql
SELECT LastName, Address
FROM Sales.Customer
```

These basic clauses define the data that needs to be retrieved and which entity or entities should be used as a source. Other clauses are not mandatory. They are used to specify limitations and other rules on the data being retrieved.

Clauses must be presented in the following order, but can be left out if they are optional:

1. [`SELECT`](#select)
2. [`FROM`](#from)
3. [`WHERE`](#where)
4. [`GROUP BY`](#group-by)
5. [`HAVING`](#having) (allowed only in combination with `GROUP BY`)
6. [`ORDER BY`](#order-by)
7. [`LIMIT`](#limit-offset)
8. [`OFFSET`](#limit-offset)

The `UNION` clause defies the usual order presented above. It will be presented in a [Union Clause](#oql-union) section at the end.

## `SELECT` Clause {#select}

The `SELECT` clause specifies which entity attributes or other specified data must be retrieved. The clause returns all the requested values of objects which match the `SELECT` clause.

The `SELECT` clause consists of the term `SELECT` and one or more column definitions. Each column definition must be separated by a comma. Each column definition defines a column or a set of columns in the result. Each single value column definition can have an alias, which will be the name of the column in the result.

{{% alert color="info" %}}
We use terms "attributes" and "columns" for data in different contexts. Attributes are the [entity attributes](/refguide/attributes/). If data belongs to objects of an entity, we refer to that data as attributes of those objects. While columns contain the same data, they are not tied to a particular entity. In the general case, OQL does not result in an entity, and so results on an OQL query are columns. When it comes to [view entities](/refguide/view-entities/), OQL columns are mapped to attributes of a view entity, and then we can speak of attributes again.

Similarly, we use term "objects" when referring to objects of an entity, but for results of an OQL query, we use "rows" because the results are not necessarily mapped to objects of a particular entity.
{{% /alert %}}

### Syntax

The syntax is as follows:

```sql
SELECT [ DISTINCT ]
	{
			*
		| { entity_name | from_alias }.*
		| { expression [ [ AS ] column_alias ] } [ ,...n ]
	}
```

### Selecting Specific Attributes

A simple way to define what is to be retrieved is to specify particular attributes of an entity. For example:

```sql
SELECT FirstName AS FName, LastName AS LName
FROM Sales.Customer
```

It is possible to specify aliases for columns – in the example above: `FirstName AS FName`. The keyword `AS` is not mandatory, so the following query is equivalent to the one above:

```sql
SELECT FirstName FName, LastName LName
FROM Sales.Customer
```

An alias is an alternative name which replaces the column name in the result. For example, when the `name` attribute is retrieved, the result column is "Name". With an alias, you can call the result column something else, like "Customer_Name". An alias cannot contain spaces.

{{% alert color="info" %}}
Aliases are mandatory when defining a view entity.
{{% /alert %}}

### Selecting All Attributes Using `*`

Using `*` (asterisk) in the SELECT clause specifies that the values of all attributes from all entities in the `FROM` clause should be returned.

Specifying `entity_name/*` and `from_alias/*` specify that the values of all attributes of the specified entity or expression of the `FROM` clause should be returned.

`entity_name` can optionally be put in double quotes. If the entity name is a [reserved OQL word](/refguide/oql/#reserved-oql-words) (like `Order` or `Group`), double quotes are mandatory.

{{% alert color="info" %}}
Specifying all attributes will also return attributes which are normally hidden in the Domain Model, such as the `ID` of each object.
{{% /alert %}}

#### Examples

The following query returns the values of all the attributes of `Sales.Customer`

```sql
SELECT *
FROM Sales.Customer
```

The following query returns all attributes of objects of `Sales.Request` that are associated with `Sales.Customer` objects (see [Select from Multiple Tables using `JOIN`](/refguide/oql-clauses/#join))

```
SELECT Sales.Request/*
FROM Sales.Customer
JOIN Sales.Customer/Sales.Customer_Request/Sales.Request
```

The following query is equivalent to the previous one, but it uses table aliases

```
SELECT Req/*
FROM Sales.Customer Cust
JOIN Cust/Sales.Customer_Request/Sales.Request Req
```

### Selecting Distinct Values with `DISTINCT` {#distinct}

The keyword `DISTINCT` specifies that duplicate rows must not be included in the result. If used, `DISTINCT` should follow directly after `SELECT`. It is not possible to request only some attributes to be distinct. `Null` is treated as a separate value. If the query result contains multiple `Null` values, `DISTINCT` is applied to them the same way it would be applied to other values.

In this example, the `Sales.Customer` entity has 4 objects.

```sql
SELECT FirstName FName, LastName LName
FROM Sales.Customer
```

returns

| FName | LName |
| ----- | ----- |
| John  | Doe   |
| Jane  | Doe   |
| Jane  | Doe   |
| Jane  | Moose |

The following query, using `DISTINCT` will result only in unique last names

```sql
SELECT DISTINCT LastName LName
FROM Sales.Customer
```

returns

| LName |
| ----- |
| Doe   |
| Moose |

If multiple attributes are selected, the result is all the unique combinations that are present in the database:

```sql
SELECT DISTINCT FirstName FName, LastName LName
FROM Sales.Customer
```

returns

| FName | LName |
| ----- | ----- |
| John  | Doe   |
| Jane  | Doe   |
| Jane  | Moose |

`DISTINCT` can also be combined with `*`.

{{% alert color="info" %}}
If you specify an entity name in `FROM`, this will return all columns, including the unique column `ID`. This means that adding `DISTINCT` does not affect the result.

In more complex cases, `SELECT DISTINCT *` can become useful.
{{% /alert %}}

```sql
SELECT DISTINCT *
FROM Sales.Customer
```

returns

| ID              | FName | LName |
| --------------- | ----- | ----- |
| 562949953421521 | John  | Doe   |
| 562949953421683 | Jane  | Doe   |
| 562949953421777 | Jane  | Doe   |
| 562949953421923 | Jane  | Moose |

### Expressions

It is possible to use more complex expressions in `SELECT`. This is explained in detail in [OQL Expressions](/refguide/oql-expressions/).

It is also possible to use a subquery. See [Subquery in `SELECT`](/refguide/oql-clauses/#subquery-in-select) for more details.

### Selecting Attributes over Associations

A unique feature of OQL is the ability to access attributes of associated objects using paths. For example:

```sql
SELECT
	Number AS RequestNumber,
	Sales.Request/Sales.Request_Customer/Sales.Customer/LastName AS CustomerName
FROM Sales.Request
```

It is possible to build paths over multiple associations. Associated entities can be reached from both directions. System associations `System.owner` and `System.changedBy` can also be used in such association paths, assuming they are enabled for the entity. For example:

```sql
SELECT
	LastName AS CustomerName,
	Sales.Customer/Sales.Request_Customer/Sales.Request/System.owner/System.User/Name AS UserName
FROM Sales.Customer
```

In the case when the association [multiplicity type](/refguide/association-properties/#multiplicity) is one-to-many or many-to-many, every attribute over association may result in multiple results per object. If there are multiple attributes over association in the `FROM` clause, the result will be a cartesian product of associated objects meaning that there will be a row for every combination of associated objects. If you want to avoid that effect, consider rewriting the query using [`JOIN`](/refguide/oql-clauses/#join).

For example, when a `Sales.Customer` with `LastName` "Doe" has two associated  `Sales.Request` objects with a `Number` attribute with values 1 and 2, the following query will return 4 rows:

```sql
SELECT
	LastName AS CustomerName,
	Sales.Customer/Sales.Request_Customer/Sales.Request/Number AS RequestNumber,
	Sales.Customer/Sales.Request_Customer/Sales.Request/Number AS OrthogonalRequestNumber
FROM Sales.Customer
WHERE Sales.Customer/LastName = 'Doe'
```

| CustomerName | RequestNumber | OrthogonalRequestNumber |
| ------------ | ------------- | ----------------------- |
| Doe          | 1                | 1                       |
| Doe          | 1                | 2                       |
| Doe          | 2                | 1                       |
| Doe          | 2                | 2                       |

## `FROM` Clause {#from}

The `FROM` clause specifies the entities or other source or sources from which the data must be retrieved.

This clause starts with the `FROM` keyword, followed by an entity name. To select data from additional entities, add these entities via the `JOIN` keyword. In OQL, this syntax is a little more strict than that of the SQL `FROM` clause.

### Syntax

Below is the full syntax of the FROM clause:

```sql
FROM
	{
		entity_name | ( sub_oql_query )
	}
	[ [ AS ] from_alias ]

	{
		{ INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } JOIN
		entity_path | entity_name | ( sub_oql_query ) [ [ AS ] from_alias ]
		[ ON <constraint> ]
	} [ ,...n ]
```

### Select from One Entity

Below is an example of a simple select from one entity:

```sql
SELECT Sales.Customer/LastName
FROM Sales.Customer
```

You can use an alias to replace the entity name:

```sql
SELECT Cust/LastName
FROM Sales.Customer AS Cust
```

### Select from Multiple Entities

You can specify multiple tables in `FROM`.

{{% alert color="info" %}}
In these examples there are 2 entities, `Sales.Customer` and `Sales.Request`:

```sql
SELECT *
FROM Sales.Customer
```

| ID              | FirstName | LastName |
| --------------- | -----     | -----    |
| 562949953421521 | John      | Doe      |
| 562949953421923 | Jane      | Moose    |
| 562949953422131 | Jim       | Elk      |

```sql
SELECT *
FROM Sales.Request
```

| ID               | CustomerName | Number |
| ---------------- | ------------ | ------ |
| 1688849860264073 | Doe          | 1      |
| 1688849860264231 | Moose        | 2      |
| 1688849860264654 | Caribou      | -1     |

{{% /alert %}}
You can select data from both entities at once.

```sql
SELECT *
FROM Sales.Customer, Sales.Request
```

 This returns get all attributes of both entities. Every object of the first entity is combined with every object of the second entity, which results in a cartesian product of objects of the two entities.

| sales$customer.ID | FirstName | LastName | sales$request.ID | CustomerName | Number |
| ---------------   | -----     | -----    | ---------------- | ------------ | ------ |
| 562949953421521   | John      | Doe      | 1688849860264073 | Doe          | 1      |
| 562949953421923   | Jane      | Moose    | 1688849860264073 | Doe          | 1      |
| 562949953422131   | Jim       | Elk      | 1688849860264073 | Doe          | 1      |
| 562949953421521   | John      | Doe      | 1688849860264231 | Moose        | 2      |
| 562949953421923   | Jane      | Moose    | 1688849860264231 | Moose        | 2      |
| 562949953422131   | Jim       | Elk      | 1688849860264231 | Moose        | 2      |
| 562949953421521   | John      | Doe      | 1688849860264654 | Caribou      | -1     |
| 562949953421923   | Jane      | Moose    | 1688849860264654 | Caribou      | -1     |
| 562949953422131   | Jim       | Elk      | 1688849860264654 | Caribou      | -1     |

You can specify conditions to filter the results in a `WHERE` clause (see more details and examples in [`WHERE` Clause](#where), below)

```sql
SELECT *
FROM Sales.Customer Cust, Sales.Request Req
WHERE Cust.LastName = Req.CustomerName
```

returns

| sales$customer.ID | FirstName | LastName | sales$request.ID | CustomerName | Number |
| ---------------   | -----     | -----    | ---------------- | ------------ | ------ |
| 562949953421521   | John      | Doe      | 1688849860264073 | Doe          | 1      |
| 562949953421923   | Jane      | Moose    | 1688849860264231 | Moose        | 2      |

To avoid ambiguity in case of duplicate attribute names, you should use the entity name or alias when specifying columns to be selected. The format in that case is, respectively, `<module>.<entity>/<attribute>` or `<alias>/<attribute>`. You can also retrieve all attributes of a particular entity using `<alias>/*`. For example:

[//]: # (Specifying sql here causes issues as it doesn't understand the "*" and starts using italics!)

```
SELECT Cust/FirstName, Req/*
FROM Sales.Customer Cust, Sales.Request Req
WHERE Cust.LastName = Req.CustomerName
```

returns

| FirstName | sales$request.ID | CustomerName | Number |
| -----     | ---------------- | ------------ | ------ |
| John      | 1688849860264073 | Doe          | 1      |
| Jane      | 1688849860264231 | Moose        | 2      |

### Select from Multiple Tables using `JOIN` {#join}

OQL supports the `JOIN` syntax, which is similar to SQL. There are 4 main types of `JOIN`:

* `INNER JOIN` or simply `JOIN`
* `LEFT JOIN` or `LEFT OUTER JOIN`
* `RIGHT JOIN` or `RIGHT OUTER JOIN`
* `FULL JOIN` or `FULL OUTER JOIN`

In addition to standard SQL syntax, OQL supports joins over associations.

The syntax is as follows:

```sql
{ INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } JOIN
entity_path | entity_name | ( sub_oql_query ) [ [ AS ] from_alias ]
[ ON <constraint> ]
```

#### entity_path

`entity_path` specifies the entity to join and the path to this entity from an earlier defined entity in the `FROM` clause.

For example, the path `Crm.Customer/Crm.Customer_Address/Crm.Address` defines an association path from the entity **Crm.Customer** to an associated entity **Crm.Address**.

As with `entity_name`, double quotes can be used.

 If `entity_path` is specified, the `ON` condition is optional. If entities are joined by name without using an association path, the `ON` condition is mandatory.

### `JOIN` Types

#### `INNER JOIN`

An `INNER JOIN` is the most common join operation between entities and represents the default join type. The query compares each row of entity A with each row of entity B to find all the pairs of rows that have an association and/or satisfy the JOIN predicate. If the association exists and the JOIN predicate is satisfied, the column values for each matched pair of rows of A and B are combined into a resulting row.

Example of an `INNER JOIN` with predicate:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
JOIN Sales.Request Req ON Cust.LastName = Req.CustomerName
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |

If the model contains an association between `Sales.Request` and `Sales.Customer`, a more logical way to write this query would be to use a join on association:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
JOIN Cust/Sales.Request_Customer/Sales.Request Req
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |

##### `LEFT OUTER JOIN`

A `LEFT OUTER JOIN` query compares each row of entity A with each row of entity B to find all pairs of rows which have an association and thus satisfy the `JOIN` predicate. When the association exists and the `JOIN` predicate is satisfied, column values for each matched pair of rows of A and B are combined into a resulting row.

However, in contrast to the `INNER JOIN` construction, the query will also return rows of entity A which do not match entity B. When columns of entity B are specified, these columns contain a null value for these rows.

The syntax is as follows:

```sql
LEFT [ OUTER ] JOIN entity_path [ ON <constraint> ]
```

Example of a `LEFT OUTER JOIN` with predicate:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
LEFT OUTER JOIN Sales.Request Req ON Cust.LastName = Req.CustomerName
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |
| Elk          | Null   |

`LEFT OUTER JOIN` over association is also supported:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
LEFT OUTER JOIN Cust/Sales.Request_Customer/Sales.Request Req
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |
| Elk          | NULL   |

##### `RIGHT OUTER JOIN`

A `RIGHT OUTER JOIN` query compares each row of entity A with each row of entity B to find all pairs of rows which have an association and thus satisfy the `JOIN` predicate. If the association exists and the `JOIN` predicate is satisfied, the column values for each matched pair of rows of A and B are combined into a resulting row.

However, in contrast to the `INNER JOIN` construction, rows from entity B that do not match entity A will also be returned. When columns of entity A are specified, these columns contain a null value for these rows.

The syntax is as follows:

```sql
RIGHT [ OUTER ] JOIN entity_path [ ON <constraint> ]
```

Example of a `RIGHT OUTER JOIN` with predicate:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
RIGHT OUTER JOIN Sales.Request Req ON Cust.LastName = Req.CustomerName
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |
| NULL         | -1     |

`RIGHT OUTER JOIN` over association is also supported:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
RIGHT OUTER JOIN Cust/Sales.Request_Customer/Sales.Request Req
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |
| NULL         | -1     |

##### `FULL OUTER JOIN`

A `FULL OUTER JOIN` query compares each row of entity A with each row of entity B to find all pairs of rows which have an association and thus satisfy the `JOIN` predicate. When the association exists and the `JOIN` predicate is satisfied, column values for each matched pair of rows from A and B are combined into a result row.

However, in contrast to the `INNER JOIN` construction, data from entities that do *not* match will also be returned. For these rows, columns of missing entities will contain null values.

The syntax is as follows:

```sql
FULL [ OUTER ] JOIN entity_path [ ON <constraint> ]
```

Example of a `FULL OUTER JOIN` with predicate:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
FULL OUTER JOIN Sales.Request Req ON Cust.LastName = Req.CustomerName
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |
| Elk          | NULL   |
| NULL         | -1     |

`FULL OUTER JOIN` over association is also supported:

```sql
SELECT Cust/LastName, Req/Number
FROM Sales.Customer Cust
FULL OUTER JOIN Cust/Sales.Request_Customer/Sales.Request Req
```

| LastName     | Number |
| ------------ | ------ |
| Doe          | 1      |
| Moose        | 2      |
| Elk          | NULL   |
| NULL         | -1     |

### Select from a Subquery

See [Subquery in `FROM`](/refguide/oql-clauses/#subquery-in-from) for details.

## `WHERE` Clause {#where}

The `WHERE` clause specifies how the data being retrieved must be constrained.

### Syntax

The syntax is as follows:

```sql
WHERE <constraint>
```

`<constraint>` is an expression of type BOOLEAN. Expressions can consist of simple comparisons using operators, functions, keywords, parameters, and system variables. If the result of the expression is `True` for a particular row, that row is included in the result. Rows that do not match the expression are not included in the result.

For more information, see [OQL Expressions](/refguide/oql-expressions/).

### Examples

The following query retrieves all customers whose name is equal to "Doe":

```sql
SELECT FirstName, LastName
FROM Sales.Customer
WHERE LastName = 'Doe'
```

| FirstName | LastName |
| --------- | -------- |
| John      | Doe      |

It is possible to specify multiple conditions in a constraint using logical operators `AND` and `OR`.

{{% alert color="info" %}}
Note that `AND` has precedence over `OR`
{{% /alert %}}

For example:

```sql
SELECT CustomerName, Number
FROM Sales.Request
WHERE
	CustomerName = 'Doe'
	OR
	CustomerName != 'Doe'
	AND
	Number < 0
```

| CustomerName | Number |
| ------------ | ------ |
| Doe          | 1      |
| Caribou      | -1     |

You can modify the precedence of logical operators using parentheses:

```sql
SELECT CustomerName, Number
FROM Sales.Request
WHERE
	(
		CustomerName = 'Doe'
		OR
		CustomerName != 'Doe'
	)
	AND
	Number < 0
```

| CustomerName | Number |
| ------------ | ------ |
| Caribou      | -1     |

You can also use a subquery in `WHERE`. See [Subquery in `WHERE`](/refguide/oql-clauses/#subquery-in-where) for examples.

#### Entities over Associations in `WHERE` Clause

A feature that is specific to OQL and is not in the standard SQL syntax is using paths to other entities over associations.

For example, when selecting from entity `Sales.Customer`, we can access an attribute of the associated entity `Sales.Request`:

```sql
SELECT FirstName, LastName
FROM Sales.Customer
WHERE
	Sales.Customer/Sales.Request_Customer/Sales.Request/Number = 1
```

| FirstName | LastName |
| --------- | -------- |
| John      | Doe      |

#### `WHERE` Clause Returns `NULL`

In some cases, the expression in `WHERE` can result in `NULL`. In that case, the `WHERE` expression is equal to `FALSE`, and the query returns no rows.

```sql
SELECT FirstName, LastName
FROM Sales.Customer
WHERE NULL
```

| FirstName | LastName |
| --------- | -------- |

## `GROUP BY` Clause {#group-by}

The `GROUP BY` clause groups OQL query results into summary rows based on the values of one or more attributes. You can filter aggregated results further using a `HAVING` clause.

### Syntax

The syntax is as follows:

```sql
GROUP BY
	expression [ ,...n ]

[HAVING <constraint>]
```

{{% alert color="info" %}}
The `GROUP BY` clause is usually used in combination with [aggregations](/refguide/oql-expressions/#aggregates): `AVG`, `COUNT`, `MAX`, `MIN`, `SUM`.
{{% /alert %}}

### Using `GROUP BY`

When a query contains a `GROUP BY` clause, then its `SELECT` clause can contain only aggregate functions and attributes and other expressions used in `GROUP BY`. If an attribute is present in `SELECT`, but not present in `GROUP BY`, the query is invalid.

{{% alert color="info" %}}
The examples below assume that there is an entity `Sales.Location` with the following objects:

```sql
SELECT Brand, City, Stock, Address, LocationNumber
FROM Sales.Location
```

| Brand  | City      | Stock | Address   | LocationNumber |
| ------ | -----     | ----- | --------- | -----          |
| Cinco  | Rotterdam | 5     | Address 1 | 1              |
| Rekall | Utrecht   | 9     | Address 4 | 4              |
| Rekall | Zwolle    | 3     | Address 3 | 3              |
| Veidt  | Rotterdam | 23    | Address 2 | 2              |
| Veidt  | Rotterdam | 1     | Address 6 | 6              |
| Veidt  | Utrecht   | 2     | Address 5 | 5              |

{{% /alert %}}

#### `GROUP BY` with Single Aggregate

The following query retrieves total stock per brand:

```sql
SELECT Brand, SUM(Stock) AS SumStock
FROM Sales.Location
GROUP BY Brand
```

| Brand  | SumStock |
| ------ | -----    |
| Cinco  | 5        |
| Rekall | 12       |
| Veidt  | 26       |

#### `GROUP BY` with Multiple Aggregates

You can specify multiple aggregate functions in combination with `GROUP BY`:

```sql
SELECT
	Brand,
	SUM(Stock) AS SumStock,
	MIN(Stock) AS MinStock,
	MAX(Stock) AS MaxStock
FROM Sales.Location
GROUP BY Brand
```

| Brand  | SumStock | MinStock | MaxStock |
| ------ | -----    | -----    | -----    |
| Cinco  | 5        | 5        | 5        |
| Rekall | 12       | 3        | 9        |
| Veidt  | 26       | 1        | 23       |

#### `GROUP BY` Multiple Attributes

You can also group by multiple attributes:

```sql
SELECT Brand, City, SUM(Stock) AS SumStock
FROM Sales.Location
GROUP BY Brand, City
```

| Brand  | City      | SumStock |
| ------ | ------    | -----    |
| Cinco  | Rotterdam | 5        |
| Rekall | Utrecht   | 9        |
| Rekall | Zwolle    | 3        |
| Veidt  | Rotterdam | 24       |
| Veidt  | Utrecht   | 2        |

#### Using Functions with `GROUP BY`

You can use functions of attributes in the `SELECT` clause, but only if those attributes are present in the `GROUP BY` clause:

```sql
SELECT Brand, LENGTH(Brand) AS NameLen, SUM(Stock) AS SumStock
FROM Sales.Location
GROUP BY Brand
```

| Brand  | NameLen | SumStock |
| ------ | ------  | -----    |
| Cinco  | 5       | 5        |
| Rekall | 6       | 12       |
| Veidt  | 5       | 26       |

You can also use functions of attributes in `GROUP BY`. Please note that if `GROUP BY` contains a function of an attribute, the attribute itself cannot be present in the `SELECT` clause because that would lead to ambiguity.

```sql
SELECT LENGTH(Brand) AS NameLen, SUM(Stock) AS SumStock
FROM Sales.Location
GROUP BY LENGTH(Brand)
```

| NameLen | SumStock |
| ------  | -----    |
| 5       | 5        |
| 6       | 12       |
| 5       | 26       |

{{% alert color="info" %}}
`GROUP BY` behavior in OQL relies on the behavior of the underlying database. Some functionality is allowed by OQL syntax, but its implementation differs per vendor.

It is recommended not to use the following functionality to avoid potential migration problems.

1. Aliases in `GROUP BY`. [Supported](/refguide/system-requirements/#databases) database vendors that allow do allow aliases in `GROUP BY` are HSQLDB, PostgreSQL, MariaDB, and MySQL.
2. Subqueries in `GROUP BY`. Supported database vendors which do allow subqueries in `GROUP BY` are PostgreSQL, MariaDB, and MySQL.
{{% /alert %}}

### Using `GROUP BY` with `HAVING`{#having}

The `HAVING` clause is used to filter aggregated results of `GROUP BY`. The difference between `HAVING` and `WHERE` is that `WHERE` is applied to every object before the objects are grouped, and `HAVING` is applied only to the aggregate rows.

The following query returns only aggregate rows for brands with more than one location:

```sql
SELECT Brand, SUM(Stock) AS SumStock, COUNT(*) AS LocationCount
FROM Sales.Location
GROUP BY Brand
HAVING COUNT(*) > 1

```

| Brand  | SumStock | LocationCount |
| ------ | -----    | ----          |
| Rekall | 12       | 2             |
| Veidt  | 26       | 3             |

You can use aggregate functions that are not present in `SELECT`:

```sql
SELECT Brand
FROM Sales.Location
GROUP BY Brand
HAVING COUNT(*) > 1 AND SUM(Stock) < 20

```

| Brand  |
| ------ |
| Rekall |

You can also use [more complex expressions](/refguide/oql-expression-syntax/) such as subqueries and functions in `HAVING`:

```sql
SELECT Brand
FROM Sales.Location loc
GROUP BY Brand
HAVING
	COUNT(*) > 1
	AND
	EXISTS (
		SELECT *
		FROM Sales.Storage stor
		WHERE stor/AvailableStorage >= SUM(loc/Stock)
	)
```

## `ORDER BY` Clause {#order-by}

The `ORDER BY` clause specifies the sort order used on columns returned in a `SELECT` statement. Multiple columns can be specified. Columns are ordered in the sequence of the items in the `ORDER BY` clause.

{{% alert color="info" %}}
This clause can include items that do not appear in the `SELECT` clause, except when `SELECT DISTINCT` is specified or when a `GROUP BY` clause exists. When `UNION` is used, the column names or aliases must be those specified in the `SELECT` clause of the first part of the query. More information is presented in the [Union Clause](#oql-union) section.
{{% /alert %}}

{{% alert color="info" %}}
The `ORDER BY` clause cannot be used in view entities without a `LIMIT` or an `OFFSET` clause. See [Sorting of View Entity Results](/refguide/use-view-entities/#sorting) in *How To Use View Entities* for more details.

If OQL v2 is enabled, an `ORDER BY` clause cannot be used in subqueries without a `LIMIT` or an `OFFSET` clause because the order of the subquery results may not be retained in the outer query. See the [`ORDER BY` in Subquery](/refguide/oql-v2/#order-by-in-subquery) section of *OQL Version 2 Features* for more details.
{{% /alert %}}

### Syntax

The syntax is as follows:

```sql
ORDER BY
	{
		order_by_expression [ ASC | DESC ]
	} [ ,...n ]
```

### order_by_expression

`order_by_expression` specifies an attribute of an entity or an alias from the `FROM` clause to sort on.

For example:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
ORDER BY LastName
```

| FirstName | LastName |
| -----     | -----    |
| John      | Doe      |
| Amelia    | Doe      |
| Oliver    | Doe      |
| Oliver    | Moose    |
| Jane      | Moose    |

{{% alert color="info" %}}
For information on the default ordering behavior of NULL values, see the [NULL Values Order Behavior](/refguide/ordering-behavior/#null-ordering-behavior) section of *Order By Behavior*.
{{% /alert %}}

{{% alert color="info" %}}
It is not possible to use functions of attributes in an `ORDER BY` clause.
{{% /alert %}}

### ASC

`ASC` specifies that the results must be returned in ascending order, from the lowest to the highest value. This is the default sort type, so results are equivalent to not specifying `ASC`.

For example:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
ORDER BY LastName ASC
```

| FirstName | LastName |
| -----     | -----    |
| John      | Doe      |
| Amelia    | Doe      |
| Oliver    | Doe      |
| Oliver    | Moose    |
| Jane      | Moose    |

### DESC

`DESC` specifies that the results must be returned in descending order, from the highest to the lowest value.

For example:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
ORDER BY LastName DESC
```

| FirstName | LastName |
| -----     | -----    |
| Oliver    | Moose    |
| Jane      | Moose    |
| John      | Doe      |
| Amelia    | Doe      |
| Oliver    | Doe      |

### Multiple `ORDER BY` Criteria

Multiple criteria can be specified in `ORDER BY`. Separate each criterion with a comma.

For example:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
ORDER BY LastName, FirstName
```

| FirstName | LastName |
| -----     | -----    |
| Amelia    | Doe      |
| John      | Doe      |
| Oliver    | Doe      |
| Jane      | Moose    |
| Oliver    | Moose    |

You can apply `ASC` and `DESC` modifiers to each criterion separately:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
ORDER BY LastName DESC, FirstName ASC
```

| FirstName | LastName |
| -----     | -----    |
| Jane      | Moose    |
| Oliver    | Moose    |
| Amelia    | Doe      |
| John      | Doe      |
| Oliver    | Doe      |

### `ORDER BY` Associated Attribute

OQL allows you to specify paths to attributes of associated entities in the `ORDER BY` clause.

{{% alert color="info" %}}
In the example below `Sales.Customer` object for Jim Elk does not have any associated objects of the `Sales.Request` entity. Depending on the database, that object will end up either at the beginning or the end of the ordered result. See [NULL Values Order Behavior](/refguide/ordering-behavior/#null-ordering-behavior) for more information.
{{% /alert %}}

```sql
SELECT LastName
FROM Sales.Customer
ORDER BY Sales.Customer/Sales.Customer_Request/Sales.Request/Number
```

| LastName     |
| ------------ |
| Doe          |
| Moose        |
| Elk          |

## `LIMIT` and `OFFSET` Clauses {#limit-offset}

With the `LIMIT` and `OFFSET` clauses, you can specify that only a portion of the results of a query is returned.

{{% alert color="info" %}}
Mendix recommends combining `LIMIT` and `OFFSET` clauses with `ORDER BY` because the return order of rows without an `ORDER BY` clause is undefined and can lead to unpredictable output.
{{% /alert %}}

### Syntax

The syntax is as follows:

```sql
[ LIMIT number ] [ OFFSET number ]
```

### `LIMIT` Clause

`LIMIT` specifies the maximum amount of rows to return.

For example, the following query retrieves the first three records sorted by last name and first name:

```sql
SELECT Brand, City, LocationNumber
FROM Sales.Location
ORDER BY LocationNumber
LIMIT 3
```

| Brand  | City      | LocationNumber |
| ------ | -----     | -----          |
| Cinco  | Rotterdam | 1              |
| Veidt  | Rotterdam | 2              |
| Rekall | Zwolle    | 3              |

### `OFFSET` Clause

`OFFSET` specifies how many rows must be skipped before returning the result rows.

For example, the following query retrieves all records except the first two:

```sql
SELECT Brand, City, LocationNumber
FROM Sales.Location
ORDER BY LocationNumber
OFFSET 2
```

| Brand  | City      | LocationNumber |
| ------ | -----     | -----          |
| Rekall | Zwolle    | 3              |
| Rekall | Utrecht   | 4              |
| Veidt  | Utrecht   | 5              |
| Veidt  | Rotterdam | 6              |

`LIMIT` and `OFFSET` can be combined in one query.

For example, the following query skips 2 records and then returns 3 records:

```sql
SELECT Brand, City, LocationNumber
FROM Sales.Location
ORDER BY LocationNumber
LIMIT 3
OFFSET 2
```

| Brand  | City      | LocationNumber |
| ------ | -----     | -----          |
| Rekall | Zwolle    | 3              |
| Rekall | Utrecht   | 4              |
| Veidt  | Utrecht   | 5              |

## `UNION` Clause {#oql-union}

The clause takes multiple select queries and combines their results into a single result set.
The resulting set by default only includes distinct rows. The `ALL` keyword can be used to include all rows. Rows are considered distinct if the combination of the column values is distinct from all other rows. Comparison logic of values is the same as the `DISTINCT` keyword of a `SELECT` clause.

All select queries must define the same number of columns in the same order and their data types should be compatible. The resulting column names will be those used in the very first `SELECT` clause.

### Syntax

The syntax is as follows:

```sql
  select_query
    { 
       UNION [ALL] select_query
    } [ ,...n ]
    [ order_by_clause ]
    [ LIMIT number ]
    [ OFFSET number ]
```

### Result data type {#oql-union-type}

The data types used in `select_query` statements are considered when determining the final return type of the `UNION` clause. All data types used in `select_query` statements must be compatible. All data types are compatible with themselves. Differing types are only compatible in these cases:

* `UNION` of numeric types `INTEGER`, `LONG` and `DECIMAL`. The return type is the numeric type found in the `select_query` statements with the highest precedence (see [Type Precedence](/refguide/oql-expression-syntax/#type-coercion) for more information). 
* `UNION` of limited and unlimited `STRING`. The return type is determined by the largest length of `STRING` attributes in the `select_query` statements.
* `UNION` of any known type and a `NULL` literal. The result type is the known type.

{{% alert color="warning" %}}
Oracle Database and SAP HANA databases do not support `UNION` containing unlimited `STRING`
{{% /alert %}}

### Examples

The following query combines sales person and customer names into a single table:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
UNION
SELECT FirstName, LastName
FROM Sales.Customer
```

| FirstName | LastName |
|-----------|----------|
| John      | Doe      |
| Amelia    | Doe      |
| Oliver    | Doe      |
| Oliver    | Moose    |
| Jane      | Moose    |
| Jane      | Doe      |

Some names are duplicated across the tables, so only some entries are included in the result. The query below uses `UNION ALL` to include all names the result:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
UNION ALL
SELECT FirstName, LastName
FROM Sales.Customer
```

| FirstName | LastName |
|-----------|----------|
| John      | Doe      |
| Amelia    | Doe      |
| Oliver    | Doe      |
| Oliver    | Moose    |
| Jane      | Moose    |
| John      | Doe      |
| Jane      | Doe      |
| Jane      | Doe      |
| Jane      | Moose    |

The following query performs a self union of a table, returning fewer rows than the original table, as only distinct rows are included in the result.

```sql
SELECT FirstName FName, LastName LName
FROM Sales.Customer
UNION
SELECT FirstName FName, LastName LName
FROM Sales.Customer
```

| FName | LName |
| ----- | ----- |
| John  | Doe   |
| Jane  | Doe   |
| Jane  | Moose |

The result of the union can also be sorted and limited, similar to a normal `SELECT` clause. This query retrieves the first 4 names sorted on both first and last name:

```sql
SELECT FirstName, LastName
FROM Sales.SalesPerson
UNION
SELECT FirstName, LastName
FROM Sales.Customer
ORDER BY FirstName, LastName
LIMIT 4
```

| FirstName | LastName |
|-----------|----------|
| Amelia    | Doe      |
| Jane      | Doe      |
| Jane      | Moose    |
| John      | Doe      |

`UNION` can be chained to use more than 2 select queries as a source. This query collects all distinct first and last names into a single column in a single table:

```sql
SELECT FirstName AS Name FROM Sales.SalesPerson
UNION
SELECT FirstName AS Name FROM Sales.Customer
UNION
SELECT LastName AS Name FROM Sales.SalesPerson
UNION
SELECT LastName AS Name FROM Sales.Customer
```

| Name   |
|--------|
| John   |
| Amelia |
| Oliver |
| Jane   |
| Doe    |
| Moose  |

#### Union of different types

Presume two entities that have columns of types `INTEGER` and `DECIMAL`:

```sql
SELECT Sale
FROM Sales.BulkSales
```

| Sale |
|------|
| 350  |
| 200  |

```sql
SELECT Sale
FROM Sales.Sales
```

| Sale  |
|-------|
| 42.25 |
| 15.5  |

Performing a `UNION` of the entities will result in a column of type `DECIMAL`:

```sql
SELECT Sale as CombinedSale FROM Sales.BulkSales
UNION
SELECT Sale FROM Sales.Sales
```

| CombinedSale |
|--------------|
| 350          |
| 200          |
| 42.25        |
| 15.5         |

#### Union of associations

Performing a `UNION` with columns that are associations is possible, given the columns refer to the same entity for all select clauses. 

Presume an entity that has one-to-many association to `Sales.Customer`:

```sql
SELECT *
FROM Sales.ExtraInfo
```

| ID              | CustomerName |
|-----------------|--------------|
| 403600446162337 | Doe          |
| 403600446169432 | Elk          |

The query below combines associations to the `Sales.Customer` entity from 2 different source entities with duplicates:

```sql
SELECT Cust.LastName as CustomerName FROM (
    SELECT Sales.Request/Sales.Request_Customer as RequestAssoc
    FROM Sales.Request
    UNION ALL
    SELECT Sales.ExtraInfo/Sales.ExtraInfo_Customer as RequestAssoc
    FROM Sales.ExtraInfo
) AS Cust
```

| CustomerName |
|--------------|
| Doe          |
| Moose        |
| Doe          |
| Elk          |

## Subqueries

A subquery is an OQL query nested inside another query. A subquery can contain the same clauses as a regular OQL query. The entities from the outer query can be referred to in a subquery. A subquery can be used in different parts of the query.

### Subquery in `SELECT` {#subquery-in-select}

 A subquery can be used as a column in the `SELECT` clause. It can refer to other tables and expressions in `FROM`.

```sql
SELECT
	Req/Number AS RequestNumber,
	(
		SELECT COUNT(*)
		FROM Sales.Customer AS Cust
		WHERE Cust/LastName = Req/CustomerName
	) AS CustomerCount
FROM Sales.Request Req
```

| RequestNumber | CustomerCount |
| ------------- | ------------- |
| 1             | 1             |
| 2             | 1             |
| -1            | 0             |

{{% alert color="info" %}}
If you use a subquery as an expression in `SELECT`, then such subquery should always return a single row and column. If the subquery returns more than one row or column, that will lead to an exception during runtime.
{{% /alert %}}

### Subquery in `FROM` {#subquery-in-from}

It is possible to use a subquery in `FROM`. For example:

```sql
SELECT Cust/LastName
FROM (
		SELECT *
		FROM Sales.Customer
	) AS Cust
```

| LastName |
| -------- |
| Doe      |
| Moose    |
| Elk      |

Subqueries in `FROM` can be combined with other tables:

```sql
SELECT Cust/LastName, Req/Number
FROM
	Sales.Request AS Req,
	(
		SELECT *
		FROM Sales.Customer
	) AS Cust
WHERE
	Req.CustomerName = Cust.LastName
```

| LastName | Number |
| -------- | ------ |
| Doe      | 1      |
| Moose    | 2      |

It is possible to refer to other tables in the outer `FROM` clause from a subquery:

```sql
SELECT Cust/LastName, Req/MaxNumber
FROM
	Sales.Customer AS Cust,
	(
		SELECT MAX(Number) as MaxNumber
		FROM Sales.Request
		WHERE Req.CustomerName = Cust.LastName
	) AS Req
```

| LastName | MaxNumber |
| -------- | --------- |
| Doe      | 1         |
| Moose    | 2         |

`JOIN` is also supported:

```sql
SELECT Cust/LastName, Req/Number
FROM
	Sales.Request Req
	LEFT JOIN
	(
		SELECT *
		FROM Sales.Customer
	) AS Cust
	ON Req.CustomerName = Cust.LastName
```

| LastName | Number |
| -------- | ------ |
| Doe      | 1      |
| Moose    | 2      |
| NULL     | -1     |

### Subquery in `WHERE` {#subquery-in-where}

A subquery can be used in the `WHERE` clause. There are multiple ways to use subqueries

#### Subquery as a Value

An outcome of a subquery can be used as a value to be compared to another value or expression. In this case, the subquery should always return exactly one column and at most one row.

For example:

```sql
SELECT Brand, City
FROM Sales.Location AS Location
WHERE
	Location.Stock = (
		SELECT MAX(Stock)
		FROM Sales.Location AS MaxStockLocation
		WHERE Location.City = MaxStockLocation.City
	)
```

| Brand  | City      |
| ------ | -----     |
| Rekall | Utrecht   |
| Rekall | Zwolle    |
| Veidt  | Rotterdam |

#### Subquery with `IN`

A subquery can be combined with the `IN` keyword. In that case, the expression is true if a value in the outer query matches one of the results of the subquery. In this case, the subquery can return any number of rows, but it should always return exactly one column.

```sql
SELECT FirstName, LastName
FROM Sales.Customer Cust
WHERE
	Cust/LastName IN (
		SELECT CustomerName
		FROM Sales.Request Req
	)
```

| FirstName | LastName |
| --------- | -------- |
| John      | Doe      |
| Jane      | Moose    |

#### Subquery with `EXISTS`

A subquery can be combined with the `EXISTS` keyword. In that case, the expression is true if the subquery returns at least one row. In case of `EXISTS`, the subquery can return any number of rows and any number of columns.

```sql
SELECT FirstName, LastName
FROM Sales.Customer Cust
WHERE
	EXISTS (
		SELECT * FROM Sales.Request Req
		WHERE Req/CustomerName = Cust/LastName
	)
```

| FirstName | LastName |
| --------- | -------- |
| John      | Doe      |
| Jane      | Moose    |

### Subquery in `HAVING` {#subquery-in-having}

Subqueries can be used in a `HAVING` clause the in same way they are used in `WHERE`: as a value or combined with `IN` or `EXISTS` clauses. For other cases, the same limitations apply to the subquery outcome.

Example:

```sql
SELECT COUNT(*) AS LocationCount, SUM(Stock) as CityStock, City AS City
FROM Sales.Location AS Location
GROUP BY City
HAVING
	SUM(Stock) <= (
		SELECT COUNT(*)
		FROM Sales.Location
	)
```

| LocationCount | CityStock | City   |
| ------------- | --------- | ------ |
| 1             | 3         | Zwolle |

Example of `EXISTS`:

```sql
SELECT COUNT(*) AS LocationCount, City AS City
FROM Sales.Location AS Location
GROUP BY City
HAVING
	EXISTS (
		SELECT *
		FROM Sales.Location AS SubLocation
		WHERE
			Location/City = SubLocation/City
			AND SubLocation/Brand = 'Rekall'
	)
```

| LocationCount | City      |
| ------------- | --------- |
| 2             | Utrecht   |
| 1             | Zwolle    |

The same result can be achieved with `IN`:

```sql
SELECT COUNT(*) AS LocationCount, City AS City
FROM Sales.Location AS Location
GROUP BY City
HAVING
	Location/City IN (
		SELECT SubLocation/City
		FROM Sales.Location AS SubLocation
		WHERE SubLocation/Brand = 'Rekall'
	)
```

| LocationCount | City      |
| ------------- | --------- |
| 2             | Utrecht   |
| 1                | Zwolle    |
