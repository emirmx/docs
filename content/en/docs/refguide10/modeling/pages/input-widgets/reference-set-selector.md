---
title: "Reference Set Selector"
url: /refguide10/reference-set-selector/
weight: 80
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="warning" %}}The **reference set selector** widget is not supported on native mobile pages. 

To upgrade it to a React-compliant widget which works on a native page, see the Mendix React Client's [Migration Guide](/refguide10/mendix-client/react/#migration-guide).{{% /alert %}}

## Introduction

A **reference set selector** is used to allow the end-user to display or select the value (or values) of a many-to-many (reference set) [association](/refguide10/associations/) by selecting the associated object (or objects).

A reference set selector must be placed in a [data container](/refguide10/data-widgets/).

For example, you could group customers into groups, and each customer could belong to several groups. Each Group can have many customers. The entities **Customer** and **Group** have a many-to-many (reference set) relationship. A reference set selector can be used to select the groups the customer belongs to.

What you can do with a reference set selector depends on the **Owner** of the association. In the example domain model below, **Owner** is set to **Default** (in the association properties **'Customer' objects refer to 'Group' objects**).

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/reference-set-selector/domain-model-owner-default.png" alt="The domain model for a reference set selector between Customer (parent) and Group where the owner is 'default' (as in, the Customer refers to the Group)" class="no-border" >}}

You can put a reference set selector in a Customer data view to allow the user to select the Group (or Groups) to which the customer belongs. However, because the Customer is the owner of the association, you cannot put a reference set selector in a Group data view to select the Customer (or Customers) in the Group.

To allow you to both add a Group to a Customer, and add a Customer to a Group, you need to set ownership of the association to **Both**.

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/reference-set-selector/domain-model-owner-both.png" alt="The domain model for a reference set selector between Customer (parent) and Group where the owner is 'both' (as in, the Customer and Group refer to each other)" class="no-border" >}}

In the reference set selector, the related entity and association used to connect it to the entity in the data view are displayed at the top of the reference set selector, and the names of the attributes of the associated objects which will be displayed are shown inside the reference set selector. Each attribute is displayed in a [grid column](/refguide10/columns/). The association and related entity and attributes are displayed between square brackets, and colored blue.

For example, using the domain model above, the following reference set selector allows the end-user to associate a Customer with one or more Groups by setting the association **Customer_Group**. This is done by selecting the **Name** of the **Group** associated with the current **Customer**.

{{< figure src="/attachments/refguide10/modeling/pages/input-widgets/reference-set-selector/reference-set-selector.png" class="no-border" >}}

The reference set selector looks a lot like a [data grid](/refguide10/data-grid/) and consequently shares many properties with it. The main differences are that the reference set selector lacks a search bar and that it has **Add** and **Remove** buttons instead of **New** and **Delete**. This is because they perform slightly different functions:

* The **Add** button adds an association to an existing object. You will need to specify the page which opens when you want to add a new association. For more information, see [Add Button](/refguide10/control-bar/#add-button).
* The **Remove** button removes the association to an object, but does not change or delete the object itself

{{% alert color="info" %}}
You must explicitly commit the object in the data view containing your reference set selector to save the association changes. This can be done, for example, by having a **Save** button for the object in the data view (as shown for the *Customer* entity in the picture above).
{{% /alert %}}

## Properties Pane

The properties pane is divided into two major sections by a toggle at the top of the pane: **Properties** and **Styling**. Reference set selector consist of the following sections:

Properties:

* [General](#general)
* [Data source](#data-source)
* [Visibility](#visibility)
* [Events](#events)
* [Common](#common)
* [Selectable Objects](#selectable-objects)

Styling:

* [Design Properties](#design-properties)
* [Common](#common-styling)

There are additional sets of properties which do not appear in the properties of the reference set selector widget.

1. The control bar contains the buttons needed to search, add, and remove associations. For more information see [Control Bar](/refguide10/control-bar/).

2. Each attribute is displayed in a column. You can find out more about the properties of these columns in [Grid Columns](/refguide10/columns/)

## Properties

### General Section {#general}

#### Width Unit {#width-unit}

You can decide how to define the widths of the columns in the reference set selector. The possible values are:

* Pixels – define the width in pixels
* Percentage – define the width as a percentage of the width of the reference set selector widget

#### Column Widths {#column-widths}

The column widths are defined as either percentages or pixels. The value for the columns are separated by semi-colons. For example a narrow column and a wide column could be defined as `20;80`. If the widths are defined in percentages, they have to add up to 100.

If column widths are defined as percentages, you can also change the widths of columns by dragging the separating line between columns.

#### Number of Rows {#number-of-rows}

With this property you can change the number of rows that will be shown in one page. See also the property 'Show empty rows'.

Default: *5*

#### Show Empty Rows {#show-empty-rows}

If you choose to show empty rows there will always be the grid that shows the same number of rows (see 'Number of rows') even if there are fewer objects to show on the page.

Default: *False*

#### Tooltip Page {#tooltip-page}

A tooltip page is a page that appears when you hover your mouse over a row. The tooltip page must contain a data view on the same entity as the data grid.

The tooltip will only be shown for the columns you specify. The default for each column is that the tooltip will *not* appear. See [Grid Columns](/refguide10/columns/) for more information.

#### Show Control Bar {#show-control-bar}

This property indicates whether the control bar will be visible in the end-user interface. The control bar also includes the paging buttons. See [Control Bar](/refguide10/control-bar/) for more information.

{{% alert color="warning" %}}
Even if the control bar is invisible there can still be a default button that is triggered by (double) clicking on a row. See the property 'Default button trigger' and [control bar](/refguide10/control-bar/) for more information.
{{% /alert %}}

Default: *True*

#### Show Paging Bar {#show-paging-bar}

With this property, you can change the way the paging bar is shown.

| Value | Description |
| --- | --- |
| Yes (with total count) | The paging bar is shown, including the **Go to last page** button and the total count of objects. |
| Yes (without total count) | The paging bar is shown (except for the **Go to last page** button). Also, the total count of objects is not shown, as page numbers are shown instead. |
| No | The paging buttons are not shown. |

Default: *Yes (with total count)*

{{% alert color="warning" %}}
Hiding the control bar also hides the paging buttons. For details, see [Show Control Bar](#show-control-bar).
{{% /alert %}}

#### Selection Mode {#selection-mode}

The selection mode determines whether and how the user can select items in the reference set selector.

| Value | Description |
| --- | --- |
| No selection | The user cannot select items. Of this is chosen, then you cannot have a **Remove** button in your reference set selector|
| Single selection  *(default)* | The user can select a single item by clicking on it. Clicking another item will make that item the selection. Clicking a selected item will deselect it. |
| Single selection and maintain | The user can select a single item by clicking on it. The first item is always selected by default. Clicking another item will make that item the selection. Clicking a selected item will not deselect it. |
| Multi-selection | The user can select multiple items by clicking the first one and holding the <kbd>Ctrl</kbd> key while clicking on other items. Clicking an item without the <kbd>Ctrl</kbd> key will deselect all other selected items and make the clicked item the selection. |
| Simple multi-selection | The user can select multiple items by clicking on them in turn. |

#### Select First {#select-first}

Specifies whether the first item should be selected by default when the reference set selector is first shown.

Possible values:

* No *(default)* 
* Yes

#### Default Button Trigger {#default-button-trigger}

The default button can be triggered by single- or double-clicking a row.

| Value | Description |
| --- | --- |
| Single click | A single click triggers the default button. This cannot be used in combination with allowing the user to select rows. |
| Double click  *(default)* | A double-click triggers the default button. |

### Data Source Section {#data-source}

{{% snippet file="/static/_includes/refguide10/data-source-section-link.md" %}}

The attribute path specifies which attribute (or attributes) of an associated entity is shown in the reference set selector. The path must follow one association of type reference set starting in the entity of the data view.

{{% alert color="warning" %}}
You cannot currently use non-persistable entities in a reference set selector.
{{% /alert %}}

### Visibility Section {#visibility}

{{% snippet file="/static/_includes/refguide10/visibility-section-link.md" %}}

### Events Section {#events}

The on-change property specifies an action that will be executed when leaving the widget, either by using the <kbd>Tab</kbd> key or by clicking another widget, after the value has been changed.

{{% snippet file="/static/_includes/refguide10/events-section-link.md" %}}

### Common Section{#common}

The rows of the reference set selector can be sorted using the `Sort order` property. It is only available for **Database** [data sources](#data-source). For more information on sorting, see [Sort Order](/refguide10/sort-order/).

{{% snippet file="/static/_includes/refguide10/common-section-link.md" %}}

### Selectable Objects Section {#selectable-objects}

The properties in the Selectable objects section determine the objects from which the end user can make a selection.

The **Source** property sets the way to define the selectable objects:

* Database *(default)*
* XPath

For more information, see the [Selectable Objects Section](/refguide10/reference-selector/#selectable-objects) section of *Reference Selector*.

{{% alert color="info" %}}
You cannot use a microflow to define selectable objects in a reference set selector.
{{% /alert %}}

## Styling

### Design Properties Section {#design-properties}

{{% snippet file="/static/_includes/refguide10/design-section-link.md" %}}

### Common Section{#common-styling}

{{% snippet file="/static/_includes/refguide10/common-section-link.md" %}}

## Read More

* [Data view](/refguide10/data-view/)
* [Entities](/refguide10/entities/)
* [Associations](/refguide10/associations/)
* [Control Bar](/refguide10/control-bar/)
