---
title: "Installation and configuration"
url: /partners/siemens/bom-component-configuration/
weight: 2
description: "Configuration instructions and explanation for the usage of the Teamcenter BOM component."
---

## Configuration
### After-startup
Add the TcConnector after-startup microflow to your project: Startup [TcConnector]. This microflow registers the required request handlers used by the **TcBOM widget**.

### General tab
{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/bom-widget-configuration-general.png">}}

Configure the following properties on the widget:
* `Widget identifier`: Unique identifier for this instance of the widget. This is useful if you have multiple **TcBOM widgets** in your application that need to display different data. You can use this ID to discriminate and return the appropriate data.
* `Product UID`: Teamcenter UID of the product (item or assembly) whose BOM will be displayed.
* `Configuration settings`: Comma-separated options to control BOM loading and display (see below).
* `Revision rule UID`: Teamcenter UID of the revision rule to apply when loading the BOM.

#### Configuration settings
When leaving the configuration settings, by default, all configuration options are shown in the configuration header menu: variants, arrangements, partitions, unit effectivity, date effectivity configuration.

To hide any option, use the corresponding values as a comma-separated list in `Configuration settings`.
Supported `Configuration settings` values:
* `HideVariantConfiguration`: hides variant-based configuration.
* `HideArrangementConfiguration`: hides arrangement-based configuration.
* `HidePartitionConfiguration`: hides partition-based configuration.
* `HideUnitEffectivityConfiguration`: hides unit effectivity configuration.
* `HideDateEffectivityConfiguration`: hides date effectivity configuration.

In the example, below, the variant and date effectivity configurations will be hidden, while the other settings will be shown:
`ConfigurationSettings` = `"HideVariantConfiguration,HideDateEffectivityConfiguration"`

### Selection tab

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/bom-widget-configuration-selection.png">}}

The widget supports cross-selection to coordinate with other page components. The cross-selection is one-way where selecting a BOM line in the BOM populates its properties on a configured entity. 
* `Selected UID`: UID of the ItemRevision that was last selected in the BOM.
* `Selected Item ID`: ID of the ItemRevision that was last selected in the BOM.
* `Selected Item Revision ID`: Revision ID of the ItemRevision that was last selected in the BOM.