---
title: "Batch Widget Conversions for React Client"
url: /refguide/mendix-client/batch-conversion/
description: "Describes how to use the batch conversion feature to convert multiple widgets for React client compatibility."
weight: 3040
---

## Introduction

When migrating your application to the [React client](/refguide/mendix-client/react/), you may need to convert multiple widgets that are not compatible with the React client. Studio Pro's batch conversion feature allows you to efficiently convert multiple unsupported widgets at once, rather than converting them individually.

The batch conversion feature automatically identifies all widgets in your application that need to be converted for React client compatibility and provides options to convert them in bulk. This significantly reduces the time and effort required to migrate large applications to the React client.

The batch conversion feature was introduced in Mendix version 11.4.0.

## Supported Widget Conversions

The batch conversion feature supports the following widget conversions:

| Original Widget                                                         | Converts To                                   |
|-------------------------------------------------------------------------|-----------------------------------------------|
| [Dynamic Image](/refguide/image-viewer/)                                | [Image](/appstore/widgets/image/)             |
| [Static Image](/refguide/image/)                                        | [Image](/appstore/widgets/image/)             |
| [Reference Selector](/refguide/reference-selector/)                     | [Combo Box](/appstore/widgets/combobox/)      |
| [Reference Set Selector](/refguide/reference-set-selector/)             | [Combo Box](/appstore/widgets/combobox/)      |
| [Input Reference Set Selector](/refguide/input-reference-set-selector/) | [Combo Box](/appstore/widgets/combobox/)      |
| [Drop-down](/refguide/drop-down/)                                       | [Combo Box](/appstore/widgets/combobox/)      |
| [Data Grid](/refguide/data-grid/)                                       | [Data Grid 2](/appstore/modules/data-grid-2/) |

## Using Batch Conversion

To start the batch conversion process you can do one of the following:

* Right-click on an incompatible widget in Studio Pro and select **Convert all to {THE WIDGET YOU WANT TO CONVERT INTO}**
* Right-click on an error or a deprecation for an incompatible widget and select **Convert all to {THE WIDGET YOU WANT TO CONVERT INTO}**

The widget conversion mechanism displays a dialog box containing a list of modules indicating the number of widgets that will be converted and how many of those are in excluded documents.

By default, all modules are selected and widgets in excluded documents are included.

You can select or deselect individual modules, and there are also options to select or deselect all modules or just marketplace modules, and to include or exclude widgets in excluded documents.

Click **Convert** to convert the selected widgets.

{{< figure src="/attachments/refguide/runtime/mendix-client/batch-conversion-dialog.png" class="no-border" >}}

### Handling Conversion Limitations

Some widgets may not be in a convertible state due to their configuration or properties. In such cases, Studio Pro will convert as many widgets as possible and will tell you how many widgets could not be converted automatically. You can then convert the remaining widgets manually by finding them through the **Error** pane if you wish.

When you do a manual conversion, right-click each individual widget and select the conversion you want. The context menu brings up a dialog box providing you with the reasons the selected widget could not be converted automatically during the batch process.
For detailed information about conversion limitations, see [Widget Conversion Limitations](/refguide/mendix-client/widget-conversion-limitations/).
