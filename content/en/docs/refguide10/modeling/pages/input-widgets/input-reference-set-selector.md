---
title: "Input Reference Set Selector"
url: /refguide10/input-reference-set-selector/
weight: 90
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="warning" %}}The **input reference set selector** widget is not supported on native mobile pages. 

To upgrade it to a React-compliant widget which works on a native page, see the Mendix React Client's [Migration Guide](/refguide10/mendix-client/react/#migration-guide).{{% /alert %}}

## Introduction

An **input reference set selector** is used to allow the end-user to display or select the value (or values) of a many-to-many (reference set) [association](/refguide10/associations/) by selecting the associated object (or objects).

An input reference set selector must be placed in a [data container](/refguide10/data-widgets/).

For example, you could group customers into groups, and each customer could belong to several groups. Each Group can have many customers. The entities **Customer** and **Group** have a many-to-many (reference set) relationship. An input reference set selector can be used to select the groups the customer belongs to.

What you can do with an input reference set selector depends on the **Owner** of the association. In the example domain model below, **Owner** is set to **Default** (in the association properties **'Customer' objects refer to 'Group' objects**).

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/input-reference-set-selector/domain-model-owner-default.png" alt="The domain model for an input reference set selector between Customer (parent) and Group where the owner is 'default' (as in, the Customer refers to the Group)" class="no-border" >}}

You can put an input reference set selector in a Customer data view to allow the user to select the Group (or Groups) to which the customer belongs. However, because the Customer is the owner of the association, you cannot put an input reference set selector in a Group data view to select the Customer (or Customers) in the Group.

To allow you to both add a Group to a Customer, and add a Customer to a Group, you need to set ownership of the association to **Both**.

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/input-reference-set-selector/domain-model-owner-both.png" alt="The domain model for an input reference set selector between Customer (parent) and Group where the owner is 'both' (as in, the Customer and Group refer to each other)" class="no-border" >}}

In the input reference set selector, the path to the attribute to be displayed (association, related entity, and attribute) is shown inside the input reference set selector, displayed between square brackets, and colored blue.

For example, using the domain model above, the following input reference set selector allows the end-user to associate a Customer with one or more Groups by setting the association **Customer_Group**. This is done by selecting the **Name** of the **Group** associated with the current **Customer**.

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/input-reference-set-selector/input-reference-set-selector.png" class="no-border" >}}

## Properties

An example of input reference set selector properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/input-reference-set-selector/input-reference-set-selector-properties.png"   width="250"  class="no-border" >}}

Reference set selector properties consist of the following sections:

* [Common](#common)
* [Data source](#data-source)
* [Design Properties](#design-properties)
* [Editability](#editability)
* [Events](#events)
* [General](#general)
* [Label](#label)
* [Selectable Objects](#selectable-objects)
* [Visibility](#visibility)

### Common Section {#common}

{{% snippet file="/static/_includes/refguide10/common-section-link.md" %}}

### Data Source Section {#data-source}

{{% snippet file="/static/_includes/refguide10/data-source-section-link.md" %}}

The attribute path specifies which attribute (or attributes) of an associated entity is shown in the reference set selector. The path must follow one association, of type reference set, starting in the entity of the data view.

### Design Properties Section {#design-properties}

{{% snippet file="/static/_includes/refguide10/design-section-link.md" %}}

### Editability Section {#editability}

{{% snippet file="/static/_includes/refguide10/editability-section-link.md" %}}

### Events Section {#events}

The on-change property specifies an action that will be executed when leaving the widget, either by using the <kbd>Tab</kbd> key or by clicking another widget, after the value has been changed.

{{% snippet file="/static/_includes/refguide10/events-section-link.md" %}}

### General Properties {#general}

#### Select Page

The select page property determines which page is displayed when the input reference selector is clicked. This page can be used to select associated objects from the list of selectable objects. This page should contain a data grid, template grid or list view connected to the same entity as the input reference set selector.

If an input reference set selector is never editable, a select page is not required.

See the [Show a Page](/refguide10/on-click-event/#show-page) section of *On Click Event and Events Section*. Note that select pages must have a [pop-up layout](/refguide10/layout/#layout-type).

{{% alert color="info" %}}
You can generate a new page to show by right-clicking the widget and selecting **Generate select page…**.
{{% /alert %}}

### Label Section {#label}

{{% snippet file="/static/_includes/refguide10/label-section-link.md" %}}

### Selectable Objects Section {#selectable-objects}

The properties in the Selectable objects section determine the objects from which the end user can make a selection. As source, you can use **Database** or **XPath**. When using **XPath**, you can add an **XPath constraint**, or use a **Constrained by** path.

For more information, see the [Selectable Objects](/refguide10/reference-selector/#selectable-objects) section of *Reference Selector*.

{{% alert color="info" %}}
You cannot use a microflow to define selectable objects in an input reference set selector.
{{% /alert %}}

### Visibility Section {#visibility}

{{% snippet file="/static/_includes/refguide10/visibility-section-link.md" %}}
