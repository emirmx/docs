---
title: "Page Generator"
url: /refguide/page-generator/
weight: 20
description: "Describes the features in Page Generator."
---

## Introduction

{{% alert color="info" %}}
Page Generator is currently an experimental feature introduced in Studio Pro 10.21.0. For more information on experimental features, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

{{% alert color="info" %}}
To use Page Generator, internet connection and signing in to Studio Pro are required.
{{% /alert %}}

Maia Page Generator is an AI-powered tool that you can use for generating a [page](/refguide/page/). It helps you by adding and configuring widgets. In Studio Pro 10.21 and above, you can use Page Generator on existing pages. As an experimental feature, Page Generator still has several limitations. For more information, see the [Limitations](#limitation) section below.

## Using Page Generator

In Studio Pro 10.21 and above, Page Generator is disabled by default. If you want to enable or disable this feature, go to **Edit** > **Preferences** > the **New Features** tab > the **Maia** section.

You can find it in the the toolbar of a page:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/page-generator/page-generator.png" max-width=80% >}}

After clicking **Generate Page**, a dedicated chat interface will appear at the right side of Studio Pro under the **Maia** tab:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/page-generator/chat-interface.png" max-width=42% >}}

Describe your page or its main goals in the chatbox. Maia will use this information to add relevant widgets to the page and configure them for you. When generating a page, Maia knows about the domain model of the module you're currently working on. For example, it might include a [Data View](/refguide/data-view) with some [Text Box](/refguide/text-box) widgets for an attribute of an Entity.

In addition to a text message, you can also select an image. Maia will use the image to understand your request better, for example it can recognize the layout of a page and replicate it. The image can be a screenshot or wireframe. You can also provide a screenshot and describe difference, an example of this kind of message could look like "Generate a page based on this image, but change the header to Welcome."

{{% alert color="info" %}}
In this dedicated chat, only requests related to Page Generation will be properly handled. If you have other questions, close this chat and go back to the general [Maia Chat](/refguide/maia-chat/) interface.
{{% /alert %}}

### Best Practices for Text Input

To achieve optimal results, provide context about your page by describing its main use cases, customer needs, or other relevant details. The more Maia knows about your page, the more tailored and accurate the generated page will be.

Below are some examples you can use as a starting point:

* The page will be used to ...
* I need a page to be able to edit my entity ...
* Create a page like the image.
* Base the page on the image, but change ... to ...

### Best Practices for Image input

The image is limited to at most 512 kb in size. Make sure to select a clear image, so that Maia has the best idea of what you're trying to achieve. For example, a screenshot, a design mock-up or a close-up picture of a drawing. Avoid heavily compressed images or low quality images, because those might cause important details to be lost.

Keep in mind that Maia only analyzes the structure of the screenshot. The theming of your app, such as the color scheme, will not be changed. This can cause some differences between the selected image and the generated page.

## Limitations {#limitation}

As an experimental feature, Page Generator has some limitations.

### Empty pages only

The experimental version of Page Generator released in 10.21 is intended to be used with empty pages. Any widgets already on the page will be removed.

### Supported widgets

In the experimental version of Page Generator release in 10.21, not all widgets are supported. 

## Read More

* [Pages](/refguide/page/)
* [Mendix AI Assistance (Maia)](/refguide/mendix-ai-assistance/)
* [Maia Chat](/refguide/maia-chat/)
