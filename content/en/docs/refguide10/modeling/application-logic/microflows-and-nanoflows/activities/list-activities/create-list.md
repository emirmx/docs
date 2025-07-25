---
title: "Create List"
url: /refguide10/create-list/
weight: 3
---

{{% alert color="info" %}}
This activity works differently in microflows and in nanoflows. In nanoflows, changes done to the lists in a sub-nanoflow are not reflected in the original nanoflow, whereas in microflows, such changes are reflected.
{{% /alert %}}

## Introduction

The **Create list** activity creates an empty list.

## Properties

An example of create list properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/activities/list-activities/create-list/create-list-properties.png" alt="create list properties" width="650px" class="no-border" >}}

There are two sets of properties for this activity, those in the dialog box on the left, and those in the properties pane on the right.

The create list properties pane consists of the following sections:

* [Action](#action)
* [Common](#common)

## Action Section{#action}

The **Action** section of the properties pane shows the action associated with this activity.

You can open a dialog box to configure this action by clicking the ellipsis (**…**) next to the action.

You can also open the dialog box by double-clicking the activity, or right-clicking the activity and selecting **Properties**.

### Entity

The entity of the objects in the list. All objects in the list must be of the same type.

### List Name

This is the name of the list which can be used by all activities that follow this activity.

## Common Section{#common}

{{% snippet file="/static/_includes/refguide10/microflow-common-section-link.md" %}}
