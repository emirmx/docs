---
title: "Create Variable"
url: /refguide10/create-variable/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="info" %}}
This activity can be used in both microflows and nanoflows.
{{% /alert %}}

## Introduction

The **Create variable** activity allows you to create a new variable and assign a value to it. 

For example, you can create a *$Discount* variable and assign a value of *0.5* to give customers a 50% discount and use this value to calculate a price for a customer.

### Variable Scope

A variable will be in scope for the entire flow and can be used by all activities following the create variable activity in the flow.

Variables created inside a loop are not accessible outside the loop. In this case the variable will persist within the loop but the value will be reset to the initial value for each iteration.

## Properties

There are two sets of properties for this activity, those in the dialog box on the left, and those in the properties pane on the right:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/activities/variable-activities/create-variable/create-variable-properties.png" alt="Create Variable Properties" width="650px" class="no-border" >}}

The **Create variable** properties pane consists of the following sections:

* [Action](#action)
* [Common](#common)

## Action Section {#action}

The **Action** section of the properties pane shows the action associated with this activity.

You can open a dialog box to configure this action by clicking the ellipsis (**…**) next to the action.

You can also open the dialog box by double-clicking the activity, or right-clicking the activity and selecting **Properties**.

### Data Type

**Data type** defines what type of data is stored in the variable. A variable can have one of the following [data types](/refguide10/data-types/): Boolean, Enumeration, Decimal, Integer/Long, or String.

### Initial Value

Defines the initial value of the variable. The value is entered using an [expression](/refguide10/expressions/) (the result of the microflow expression must match the data type of the variable).

### Variable Name

Variable name defines the name of the resulting variable. The variable can be used by all activities following this activity in the flow. Each variable you create must have a different name.

## Common Section {#common}

{{% snippet file="/static/_includes/refguide10/microflow-common-section-link.md" %}}

## Read More

* [Activities](/refguide10/activities/)
