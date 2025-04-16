---
title: "Maia for OQL"
linktitle: "Maia for OQL"
url: /refguide/maia-for-oql/
weight: 5
description: "Describes the features in Maia for OQL Generation."
---

## Introduction

{{% alert color="info" %}}
**Maia for OQL** is currently an experimental feature. For more information on experimental features, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

{{% alert color="info" %}}
An internet connection and signing in to **Studio Pro** are required to use **Maia for OQL**.
{{% /alert %}}

**Maia for OQL** is a powerful feature that enables you to generate and manage OQL (Object Query Language) queries through an intuitive interface. It is designed to simplify query creation and reduce manual effort. As this feature is experimental, it currently includes some limitations. For more details, refer to the [Limitations](#limitations) section below.

## Using Maia for OQL

To enable this feature, navigate to **Edit** > **Preferences** > the **New Features** tab, and enable it under the **Maia** section.

Once enabled, you can access it from the toolbar in the **OQL Editor**:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/maia-oql-generator/maia-for-oql-button.png" max-width=80% >}}

Clicking **Maia for OQL** opens a dedicated chat interface on the right side of Studio Pro, under the **Maia** tab:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/maia-oql-generator/maia-pane-for-oql-generator.png" max-width=42% >}}

To use **Maia for OQL**, simply describe the data you need — for example, _“Show all active orders with customer names”_ — and Maia will generate the most relevant OQL query based on the data available in the same module.

Maia interprets your intent and provides a query that fits your requirements, helping you avoid manual query creation and common syntax errors.

## Limitations {#limitations}

* **Maia for OQL** currently supports only **view entities**.
* Associations with a **custom name** are not supported.
* **Cross-module associations** are not supported.
* **Chat history** does not retain previous OQL examples.
