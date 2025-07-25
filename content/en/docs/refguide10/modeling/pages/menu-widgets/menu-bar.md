---
title: "Menu Bar"
url: /refguide10/menu-bar/
weight: 1
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="warning" %}}The menu bar widget is not supported on native mobile pages.{{% /alert %}}

## Introduction

A menu bar shows menu items of a [navigation profile](/refguide10/navigation/#profiles) or in a [menu](/refguide10/menu/) document in the form of a horizontal bar with items. These items are determined by the [Menu source](#menu-source) and are either configured in the [Navigation](/refguide10/navigation/) or a [Menu](/refguide10/menu/).

Menu bars can go two levels deep, that means menu items can have sub-items. For more information on menu items and their properties, see [Menu](/refguide10/menu/).

{{< figure src="/attachments/refguide10/modeling/pages/menu-widgets/menu-bar/menu-bar.png" alt="Menu Bar" class="no-border" >}}

## Properties

An example of menu bar properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/pages/menu-widgets/menu-bar/menu-bar-properties.png"   width="250"  class="no-border" >}}

Menu bar properties consist of the following sections:

* [Common](#common)
* [Design properties](#design)
* [General](#general)

### Common Section {#common}

{{% snippet file="/static/_includes/refguide10/common-section-link.md" %}}

### Design Properties Section {#design}

{{% snippet file="/static/_includes/refguide10/design-section-link.md" %}}

### General Section {#general}

#### Menu Source {#menu-source}

The items that are shown in the menu widget are determined by the **Menu source**. Possible menu sources are described in the table below:

| Value              | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| Project navigation  *(default)* | The menu items are taken from one of profiles defined in the [Navigation](/refguide10/navigation/#profiles). |
| Menu document      | The menu items are taken from a [menu](/refguide10/menu/) document.       |

#### Profile 

Only available when the [menu source](#menu-source) is set to **Project navigation**. The **Profile** property specifies what [navigation profile](/refguide10/navigation/#profiles) is used for the widget. 

Default: *Responsive*

#### Menu 

Only available when the [menu source](#menu-source) is set to **Menu document**. The **Menu** property specifies what [menu](/refguide10/menu/) document is used for the widget.

## Read More

* [Page](/refguide10/page/)
* [Menus and Navigation](/refguide10/menu-widgets/)
* [Properties Common in the Page Editor](/refguide10/common-widget-properties/)
