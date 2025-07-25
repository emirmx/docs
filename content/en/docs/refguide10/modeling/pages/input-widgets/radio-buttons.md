---
title: "Radio Buttons"
url: /refguide10/radio-buttons/
weight: 50
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

{{% alert color="warning" %}}The radio buttons widget is not supported on native mobile pages.{{% /alert %}}

**Radio Buttons** are used to display and, optionally, allow the end-user to edit the value of an attribute or a variable of [data type](/refguide10/data-types/) *Boolean* or *Enumeration*.

When the page is displayed to the end-user, all the possible values are listed, with a filled-in circle next to the selected value and an empty circle next to the unselected value (or values). Only one value can be chosen – choosing another value deselects the current value. For example:

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/radio-buttons/radio-buttons-displayed.png" class="no-border" >}}

A radio button must be placed within a data context to display or edit the intended value:

* A [data container](/refguide10/data-widgets/) widget containing an object
* A snippet containing one or more [parameters](/refguide10/page-properties/#parameters)
* A page or a snippet containing one or more [variables](/refguide10/page-properties/#variables)

The name of the configured value is shown inside the radio button widget, between square brackets, and colored blue.

For example, the following image contains two sets of radio buttons. The first allows the end-user to see, and set, the value of an enumeration identifying the preferred time to contact this person (**PreferredContact**). The second allows the end-user to see, and set, a Boolean indicating whether this is a **Personal** contact.

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/radio-buttons/radio-buttons.png" class="no-border" >}}

## Properties Pane

The properties pane is divided into two major sections by a toggle at the top of the pane: **Properties** and **Styling**. Radio button properties consist of the following sections:

Properties:

* [General](#general)
* [Data Source](#data-source)
* [Label](#label)
* [Editability](#editability)
* [Visibility](#visibility)
* [Common](#common)
* [Events](#events)

Styling:

* [Design Properties](#design-properties)
* [Common](#common-styling)

## Properties

### General Section{#general}

#### Orientation

This property defines whether the radio buttons are rendered as a **Horizontal** or **Vertical** list.

Default: *Horizontal*

### Data Source Section{#data-source}

{{% snippet file="/static/_includes/refguide10/data-source-section-link.md" %}}

### Label Section{#label}

{{% snippet file="/static/_includes/refguide10/label-section-link.md" %}}

### Editability Section{#editability}

{{% snippet file="/static/_includes/refguide10/editability-section-link.md" %}}

### Visibility Section{#visibility}

{{% snippet file="/static/_includes/refguide10/visibility-section-link.md" %}}

### Common Section{#common}

{{% snippet file="/static/_includes/refguide10/common-section-link.md" %}}

### Events Section{#events}

#### On Change{#on-change}

The on-change property specifies an action that will be executed when leaving the widget, either by using the <kbd>Tab</kbd> key or by clicking another widget, after the value has been changed.

{{% snippet file="/static/_includes/refguide10/events-section-link.md" %}}

#### On Enter

The on-enter property specifies an action that will be executed when the widget is entered, either by using the <kbd>Tab</kbd> key or by clicking it with the mouse.

{{% snippet file="/static/_includes/refguide10/events-section-link.md" %}}

#### On Leave

The on-leave property specifies an action that will be executed when leaving the widget, either by using the <kbd>Tab</kbd> key or by clicking another widget.

This differs from the [On change](#on-change) property in that the event will always be triggered, even if the value has not been changed.

{{% snippet file="/static/_includes/refguide10/events-section-link.md" %}}

## Styling

### Design Properties Section{#design-properties}

{{% snippet file="/static/_includes/refguide10/design-section-link.md" %}} 

### Common Section{#common-styling}

{{% snippet file="/static/_includes/refguide10/common-section-link.md" %}}

## Read More

* [Data View](/refguide10/data-view/)
* [Attributes](/refguide10/attributes/)
* [Variables](/refguide10/page-properties/#variables)
