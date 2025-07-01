---
title: "OQL DELETE Statement"
url: /refguide/oql-delete-statement/
weight: 210
---

## Introduction

Starting with Mendix 11.1, we allow objects to be deleted in bulk using OQL `DELETE` statements.
These are translated to SQL statements that are sent to the database.
This can be much faster than retrieving the objects and then deleting the resulting list.

This feature should be considered experimental.
It is currently only accessible through the Java API by writing a Java action.

## Java API for OQL updates

OQL Statements can be executed using the `Core.createOqlStatement` Java API. Example:
```
Core.createOqlStatement("DELETE FROM Module.Customer WHERE Name = 'Mary'").execute(context) 
```
It is possible to pass values as parameters to the query. Example:
```
Core.createOqlStatement("DELETE FROM Module.Customer WHERE Name = $nameParam")
    .setVariable("nameParam", customerName)
    .execute(context)
```

## DELETE Statement syntax

The syntax of `DELETE` statements is
```
DELETE FROM <entity> WHERE <condition>
```
`condition` can be anything that can appear in an OQL [WHERE clause](/refguide/oql-clauses/#where).

### Joins

It is not possible to directly join other entities in the `FROM` clause.
But the same result can be achieved using long paths or subqueries. Examples:
```
DELETE FROM Module.Order WHERE Module.Order_Customer/Module.Customer/Name = 'Mary'
```
```
DELETE FROM Module.Order WHERE ID IN
  ( SELECT ID FROM Module.Order
    INNER JOIN Module.Customer ON Module.Customer/CustomerID = Module.Order/CustomerID
    WHERE Module.Customer/Name = 'Mary' )
```

## Limitations

* It is not possible to use OQL DELETE with entities that have associations with non-default delete behavior.
  (Associations that use either "Delete as well" or "Delete only if not associated".)
* It is not possible to use OQL DELETE for `System.FileDocument` or any specialization of it.
* Entity access is never applied to OQL statements.
* No event handlers will be executed.
* Runtime and client state will not be updated with the changes.
