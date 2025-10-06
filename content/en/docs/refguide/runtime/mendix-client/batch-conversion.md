---
title: "Batch Widget Conversions for React Client"
url: /refguide/mendix-client/batch-conversion/
description: "Describes how to use the batch conversion feature to convert multiple widgets for React client compatibility."
weight: 30
---

## Introduction

When migrating your application to the [React client](/refguide/mendix-client/react/), you may need to convert multiple widgets that are not compatible with React client. Studio Pro's batch conversion feature allows you to efficiently convert multiple unsupported widgets at once, rather than converting them individually.

The batch conversion feature automatically identifies all widgets in your application that need to be converted for React client compatibility and provides options to convert them in bulk. This significantly reduces the time and effort required to migrate large applications to the React client.

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

To start the batch conversion process you can either:

1. Right-click on an incompatible widget in Studio Pro and select **Convert all to [target widget]**
2. Right-click on an error or a deprecation for an incompatible widget and select **Convert all to [target widget]**

The Widget Conversion dialog displays:

{{< figure src="/attachments/refguide/runtime/mendix-client/batch-conversion-dialog.png" class="no-border" >}}

### Handling Conversion Limitations

Some widgets may not be in a convertible state due to specific configurations or properties. In such cases, Studio Pro will attempt to convert as many widgets as possible and will inform you about how many widgets could not be converted automatically. You can then choose to convert the remaining widgets manually by finding them through the error pane. 

{{% alert color="info" %}}
For detailed information about conversion limitations, see [Widget Conversion Limitations](/refguide/mendix-client/widget-conversion-limitations/).
{{% /alert %}}
