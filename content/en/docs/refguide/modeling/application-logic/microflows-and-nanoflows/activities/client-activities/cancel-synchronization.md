---
title: "Cancel synchronization"
url: /refguide/cancel-synchronization/
weight: 70
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="info" %}}
This activity can only be used in nanoflows.
{{% /alert %}}

## Introduction

The **Cancel synchronization** activity cancels a running synchronization. You can start synchronization again later. For synchronization behavior, see [Synchronize](/refguide/mobile/building-efficient-mobile-apps/offlinefirst-data/synchronization/).

## Properties

There are two sets of properties for this activity, those in the dialog box on the left, and those in the properties pane on the right:

{{< figure src="/attachments/refguide/modeling/application-logic/microflows-and-nanoflows/activities/client-activities/cancel-synchronization/cancel-synchronization-properties.png" class="no-border" >}}

The **Cancel synchronization** properties pane consists of the following sections:

* [Action](#action)
* [Common](#common)

## Action Section {#action}

The **Action** section of the properties pane shows the action associated with this activity.

You can open a dialog box to configure this action by clicking the ellipsis (**…**) next to the action.

You can also open the dialog box by double-clicking the activity, or right-clicking the activity and selecting **Properties**.

## Common Section {#common}

{{% snippet file="/static/_includes/refguide/microflow-common-section-link.md" %}}

## Limitations

* If the synchronization is in a step that applies file-related changes, it is not canceled to avoid leaving the system in an inconsistent state.
* If the synchronization is already in the final steps, cancellation is not applied because most processing is already complete, so it is allowed to finish.
