---
title: "Listen to Widget Source"
url: /refguide9/listen-to-grid-source/
weight: 70
---

## Introduction

The listen-to-widget data source is a data view specific source that allows a data view to display detailed information on an object selected in a data grid, template grid, or a list view on the same page. This is especially useful when displaying large amount of data, which limits the information available per object, as it allows the user to view details of an individual object without having to open a new page.

{{< figure src="/attachments/refguide9/modeling/pages/data-widgets/data-sources/listen-to-grid-source/listen-to-widget-example.jpg" alt="Listen to Widget Example"   width="400"  class="no-border" >}}

A data view in an image above listens to a data grid. In this example, the data view will display the name of the selected product if one is selected.

List views, template grids, and data grids are list widgets and can be listened to. If no object is selected in the list widget, the data view will remain empty and unresponsive.

To enable **Listen to widget** in common data sources, do the following per your use case: 

* DataGrid2: Select **Selection** > **Single** and **Selection method** > **Row click**  to be able to select a  DataGrid2 as a **Listen to widget** data source in your dataview.
* Gallery: Select **Selection** > **Single** to be able to select the Gallery as a **Listen to widget** data source in your dataview.

## Properties

### List Widget

Specifies the list widget which controls the object shown in the data view.
