---
title: "List Activities"
url: /refguide/list-activities/
weight: 20
---

## Introduction

{{% alert color="info" %}}
List activities work differently in microflows and in nanoflows. In nanoflows, changes done to the lists in a sub-nanoflow are not reflected in the original nanoflow, whereas in microflows, such changes are reflected.{{% /alert %}}

When working with the Mendix Platform, you can use microflows to manipulate not only single objects but whole lists of entities with a single activity.

Additional activities which work on lists, [commit object(s)](/refguide/committing-objects/), [delete object(s)](/refguide/deleting-objects/), and [retrieve](/refguide/retrieve/), are in the [Object Activities](/refguide/object-activities/) section of the toolbox. You can also [loop](/refguide/loop/) through a list to perform activities on the individual objects.

The activities described in this document are in the **List Activities** section of the **Toolbox**.

The following are the list activities you can use in your microflow or nanoflow:

* [Aggregate List](/refguide/aggregate-list/) – calculates aggregated values over a list
* [Change List](/refguide/change-list/) – adds objects to, and removes objects from a list
* [Create List](/refguide/create-list/) – creates an empty list
* [List Operation](/refguide/list-operation/) – performs actions on a list and, if the result is a list, returns a new list containing the result

## Read More

* [Activities](/refguide/activities/)
