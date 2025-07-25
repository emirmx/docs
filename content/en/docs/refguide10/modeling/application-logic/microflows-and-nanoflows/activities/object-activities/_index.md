---
title: "Object Activities"
url: /refguide10/object-activities/
weight: 10
#To update screenshots of these microflows in Studio Pro, use the Microflow Screenshots app.
---

## Introduction

When working with the Mendix Platform, objects of entities are always being manipulated. This happens implicitly within [data containers](/refguide10/data-widgets/) on a page, or explicitly using activities in microflows and nanoflows.

The activities in this section of the microflow and nanoflow toolbox generally work on single objects, however **commit object(s)**, **delete object(s)**, and **retrieve** also work on lists of objects. For other activities which work with lists, see [List Activities](/refguide10/list-activities/).

The activities described in this document are in the **Object Activities** section of the **Toolbox**.

The following are the object activities you can use in your microflow or nanoflow:

* [Cast object](/refguide10/cast-object/) – changes the type of object from a generalized object type to the specialized object type
* [Change object](/refguide10/change-object/) – changes the members of an object
* [Commit object(s)](/refguide10/committing-objects/) – either stores objects of persistable entities in the database or stores objects of non-persistable entities in memory to allow them to be rolled back
* [Create object](/refguide10/create-object/) – creates an object
* [Delete object(s)](/refguide10/deleting-objects/) – activity deletes one or more objects
* [Retrieve](/refguide10/retrieve/) – gets one or more objects of an entity
* [Rollback object](/refguide10/rollback-object/) – undoes uncommitted changes made to an object

{{% alert color="info" %}}
Any changes made to an object are visible in all widgets. This is because changes are stored globally in the client.
{{% /alert %}}

## Read More

* [Activities](/refguide10/activities/)
