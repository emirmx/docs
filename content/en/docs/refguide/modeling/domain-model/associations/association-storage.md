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

## Advantages of Direct Associations

	Speed up queries and statements

	Reduce space use in db

    Existing XPath and OQL queries continue to work without change.

## Limitations of Direct Associations

	Only for 1-* associations

	For both and default ownership

	Only persistent entities and remote-persistent

## Enabling Direct Associations

Enabling direct associations is simple, and has the following features:

* The choice is reversible – you can decide to revert to using association tables (but see things to think about [before switching to direct associations](#before))
* Direct associations are available in Mendix versions 10.21 and above
* You can decide whether to enable it for a specific association, or [make it the default](/brokenlink) for all new associations

For more information, see the [Association Storage](/refguide/association-properties/#storage) section of *Association Properties*.

{{% todo %}}Do we want to say this without a firm timeline?{{% /todo %}}

The development pipeline includes plans to bulk migrate the database to avoid long downtime for large databases.

## Before Switching to Direct Associations{#before}

Before deciding to switch from an association table to a direct association, bear the following in mind:

* Associations have to be rewritten to the database so migration can take a long time, especially where you have a large amount of data already stored in your database 
* Queries are not always faster, and might not be faster in your use case
* If you have written any custom SQL, this might break – existing XPaths and OQL queries will not be affected