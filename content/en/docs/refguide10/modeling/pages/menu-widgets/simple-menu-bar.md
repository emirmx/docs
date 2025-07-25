---
title: "Simple Menu Bar"
url: /refguide10/simple-menu-bar/
weight: 2
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="warning" %}}The simple menu bar widget is not supported on native mobile pages.{{% /alert %}}

## Introduction

A simple menu bar shows menu items of a [navigation profile](/refguide10/navigation/#profiles) or in a [menu](/refguide10/menu/) document in the form of a horizontal or vertical bar. These items are determined by the [Menu source](#menu-source) and are either configured in the [Navigation](/refguide10/navigation/) or a [Menu](/refguide10/menu/).

Sub-items of menu items are not displayed by this widgets, that means the menu structure can only have one level. For more information on menu items and their properties, see [Menu](/refguide10/menu/).

{{< figure src="/attachments/refguide10/modeling/pages/menu-widgets/simple-menu-bar/simple-menu-bar.png" alt="Simple Menu Bar" class="no-border" >}}

## Properties

An example of a simple menu bar properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/pages/menu-widgets/simple-menu-bar/simple-menu-bar-properties.png" alt="Simple Menu Bar Properties"   width="250"  class="no-border" >}}

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
| Project navigation  *(default)* | The menu items are taken from one of the profiles defined in the [Navigation](/refguide10/navigation/). |
| Menu document      | The menu items are taken from a [menu](/refguide10/menu/) document.       |

#### Profile 

Only available when the [Menu source](#menu-source) is set to **Project navigation**. The **Profile** property specifies what [Navigation profile](/refguide10/navigation/#profiles) is used for the widget. 

Default: *Responsive*

#### Menu 

Only available when the [Menu source](#menu-source) is set to **Menu document**. The **Menu** property specifies what [Menu](/refguide10/menu/) document is used for the widget.

#### Orientation

This property determines how the simple menu bar is laid out.

| Orientation | Description |
| --- | --- |
| Horizontal  *(default)* | The menu items are next to each other and the images are above the captions. |
| Vertical | The menu items are underneath each other and the images are next to the captions. |

## Read More

* [Page](/refguide10/page/)
* [Menus and Navigation](/refguide10/menu-widgets/)
* [Properties Common in the Page Editor](/refguide10/common-widget-properties/)
