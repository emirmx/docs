---
title: "List Operation"
url: /refguide10/list-operation/
weight: 4
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="info" %}}
This activity works differently in microflows and in nanoflows. In nanoflows, changes done to the lists in a sub-nanoflow are not reflected in the original nanoflow, whereas in microflows, such changes are reflected.
{{% /alert %}}

## Introduction

The **List operation** activity can perform various actions on a list. The result of the action is returned as a new list in contrast to the [Change list](/refguide10/change-list/) activity.

The actions which can be performed are:

* Union 
* Intersect 
* Subtract 
* Contains 
* Equals 
* Sort 
* Filter 
* Filter by expression
* Find 
* Find by expression 
* Head 
* Tail 
* Range

See below for details on these actions.

## Properties

An example of list operation properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/activities/list-activities/list-operation/list-operation-properties.png" alt="list operation properties" width="650px" class="no-border" >}}

There are two sets of properties for this activity, those in the dialog box on the left, and those in the properties pane on the right.

The list operation properties pane consists of the following sections:

* [Action](#action)
* [Common](#common)

## Action Section {#action}

The **Action** section of the properties pane shows the action associated with this activity.

You can open a dialog box to configure this action by clicking the ellipsis (**…**) next to the action.

You can also open the dialog box by double-clicking the activity, or right-clicking the activity and selecting **Properties**.

### Operation

A list operation action can execute any of the following operations. The operations are categorized by the type of operands they have:

* Binary – operations which work with a second list or object
* Member Inspections – operations which work with specified elements (attributes and associations) of the objects in the list
* Unary – operations which work on the list with no other operands

#### Binary

These binary operations have as an input a list and either another list or an object. They return another list or a Boolean, depending on the operation. All lists and objects must relate to the same entity.

| Operation | Description | Result Type |
| --- | --- | --- |
| Union | The result is a combination of the elements of both parameters avoiding duplicates. | List |
| Intersect | The result is a list containing elements that appear in both parameters. | List |
| Subtract | The result is the first parameter with the element (or elements) of the second parameter removed. | List |
| Contains | Checks whether all elements of the second parameter are present in the first parameter. | Boolean |
| Equals | Checks whether the lists contain the same elements. | Boolean |

#### Member Inspections

These operations takes a list and one or more members (attributes or associations) as input. They return either an object or another list, depending on the operation.

| Operation | Description | Result Type |
| --- | --- | --- |
| Sort | Allows you to sort a list based on a number of attributes. The attributes are ordered to determine their priority while sorting. You cannot use associations to sort a list. Sorting attributes from generalized entities is not allowed. For more information, see the [Sort Strategies: Nanoflows vs. Microflows](#sort) section below. | List |
| Find | Finds the first object of which the member has the given value. | Object |
| Filter | Finds all objects of which the member has the given value. | List |

#### Unary

These unary operations have a list as the single operand and return either an object or another list, depending on the operation.

| Operation | Description | Result Type |
| --- | --- | --- |
| Head | The result is the first element of the list, or empty if the parameter contains zero elements or was initialized as empty. | Object |
| Tail | The result is a list containing all elements of the parameter except the first, or an empty list if the parameter contains zero elements or was initialized as empty. | List |

#### Expression

These operations take a list and filter it based on an expression. Inside the expression, `$currentObject` can be used to perform the filtering.

| Operation | Description | Result Type |
| --- | --- | --- |
| Find by expression | Finds the first object that matches the given expression. | Object |
| Filter by expression | Finds all the objects that match the given expression. | List |

#### Range {#range}

This operation takes a list and filters it based on two expressions: `offset` and `amount`.

| Operation | Description | Result Type |
| --- | --- | --- |
| Range | Retrieve a given number of objects (**Amount**) starting at a given index (**Offset**). The `amount` and `offset` are expressions that should result in a number. Note that the first object has an offset of 0. An amount of 0 means that all objects are retrieved. | List |

### Sort Strategies: Nanoflows vs. Microflows {#sort}

Microflows provide locale-sensitive string comparison for a sort operation, ensuring strings are sorted according to the rules of a specific locale. Nanoflows do not have this capability. However, from Studio Pro version 10.10, nanoflows are updated to ignore case sensitivity during sorting.

### List Name, Object Name, or Variable Name

This is the name of the resulting List, Object, or Boolean variable. The result can be used by all activities that follow this activity.

## Common Section {#common}

{{% snippet file="/static/_includes/refguide10/microflow-common-section-link.md" %}}
