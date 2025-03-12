---
title: "Association Storage Options"
linktitle: "Association Storage"
url: /refguide/association-storage/
weight: 25
---

## Introduction

Association storage options were introduced in Mendix 10.21 to give you more control over how associations are implemented in the underlying database.

## Association Tables vs. Direct Associations

<!--- Everything except simple associations is in tables – what do you mean "except simple associations?" --->

Prior to Mendix 10.21, all associations were stored in association tables. This had the advantage that you didn't have to worry about the [multiplicity](/refguide/association-properties/#multiplicity) or [navigability](/refguide/association-properties/#navigability) of the associations. You could change things as your domain model evolved.

In Mendix 10.21 you can choose to implement some associations as direct associations. This means that the ID of the **Child** object is stored as a foreign key column of the **Parent** object (for example the "many" side of the association) in the underlying database table, thus removing the need for a association table.

XPath and OQL queries work identically for both association tables and direct associations. You do not have to change anything or learn different flavors of these languages to work with them.

## Default Association Storage

In Mendix 10.20 and below associations are always implemented as association tables.

In Mendix 10.21 and above, the following defaults apply:

* **New projects** – one-to-many and one-to-one associations are implemented as direct associations
* **Upgraded projects** – for projects which are upgraded from an older version of Mendix, all new associations acontinue to be implemented as association tables

{{% alert color="info" %}}
In your app settings you can [change the default](/brokenlink) for all new associations.
{{% /alert %}}

## Advantages of Direct Associations

Because they don't have association tables, using direct associations can bring the following advantages:

* In many cases, using direct associations can speed up queries and statements
* They use less space in the database
* You can choose to implement direct associations for some of your associations, while leaving others as association tables

## Limitations of Direct Associations

You cannot replace association tables with direct associations for all types of association in your app. Direct associations have the following limitations: 

* You can only use direct associations for one-to-many (1-*, default owner) and one-to-one (1-1, owner both) associations – you cannot use it for many-many associations.
* Direct associations are only allowed between persistable entities or between a persistable entity and a persistable external entity. You cannot use them on view entities, non-persistable entities, or between external entities.

{{% todo %}}Originally said "remote persistent" - I don't think we document two different types of remote entity?{{% /todo %}}

## Enabling Direct Associations

Enabling direct associations is simple, and has the following features:

* The choice is reversible – you can decide to revert to using association tables (but see things to think about [before switching to direct associations](#before))
* Direct associations are available in Mendix versions 10.21 and above
* You can decide whether to enable it for a specific association, or [change the default](/brokenlink) for all new associations

For more information, see the [Association Storage](/refguide/association-properties/#storage) section of *Association Properties*.

{{% todo %}}Do we want to say this without a firm timeline?{{% /todo %}}

The development pipeline includes plans to bulk migrate the database to avoid long downtime for large databases.

## Before Switching to Direct Associations{#before}

Before deciding to switch from an association table to a direct association, bear the following in mind:

* Associations have to be rewritten to the database so migration can take a long time, especially where you have a large amount of data already stored in your database 
* Queries are not always faster, and might not be faster in your use case
* If you have written any custom SQL that accesses Mendix tables directly, this might break – existing XPaths and OQL queries will not be affected