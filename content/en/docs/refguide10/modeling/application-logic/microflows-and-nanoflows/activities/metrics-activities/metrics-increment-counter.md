---
title: "Increment Counter"
url: /refguide10/metrics-increment-counter/
weight: 30
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="info" %}}
This activity can only be used in microflows.
{{% /alert %}}

## Introduction

The **Increment Counter** activity can be used to increment a metrics counter by one.

## Properties

An example of increment counter properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/activities/metrics-activities/metrics-increment-counter/properties.png" alt="Increment Counter Properties" class="no-border" >}}

There are two sets of properties for this activity, those in the dialog box on the left, and those in the properties pane on the right.

The **Increment Counter** properties pane consists of the following sections:

* [Action](#action)
* [Common](#common)

## Action Section {#action}

The **Action** section of the properties pane shows the action associated with this activity.

You can open a dialog box to configure this action by clicking the ellipsis (**…**) next to the action.

You can also open the dialog box by double-clicking the activity, or right-clicking the activity and selecting **Properties**.

### Name

The name of the counter whose value you want to increment by 1, which must adhere to the following rules:

* The name can only contain alpha-numeric characters, dots or underscores.
* The name must start with a letter.
* The name cannot start with `mx`, because this prefix is reserved for Mendix internal metrics.
* The name is case-insensitive.

{{% alert color="info" %}}
It is recommended to use a common prefix that uniquely defines your organization and application.
{{% /alert %}}

### Tags

You can specify a list of tags to enrich the counter name with key/value pairs. See [Tag Naming](https://micrometer.io/docs/concepts#_tag_naming) on the Micrometer site for more guidance on using tags.

## Common Section {#common}

{{% snippet file="/static/_includes/refguide10/microflow-common-section-link.md" %}}

## Read More

* [Metric Configuration](/refguide10/metrics/)
* [Metrics Activities](/refguide10/metrics-activities/)
* [Meter Concepts](https://micrometer.io/docs/concepts)
