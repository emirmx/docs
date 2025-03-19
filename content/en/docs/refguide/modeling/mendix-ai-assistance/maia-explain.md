---
title: "Maia Explain"
linktitle: "Explain"
url: /refguide/maia-explain/
weight: 10
description: "Describes the features in Maia Explain."
---

## Introduction 

{{% alert color="info" %}}
Maia Explain is currently an experimental feature introduced in Studio Pro 10.21.0. For more information on experimental features, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

{{% alert color="info" %}}
To use Maia Explain, an internet connection and signing in to Studio Pro are required.
{{% /alert %}}

Maia Explain is an AI-powered tool that helps you easily understand a microflow or a nanoflow. It explains the general purpose of the logic and highlights specific technical details to help you understand the logic further.

## Using Maia Learn

To enable Maia Explain, go to **Edit** > **Preferences** > **New Features** and select **Enable Maia Explain (experimental)**.

Once enabled, there are two ways to access Maia Explain:

* In the toolbar of the microflow or nanoflow editor, click **Explain**.
* In the App Explorer, right-click a microflow or a nanoflow to open its context menu, and click **Explain**.

A message is sent to Maia and an explanation interface appears on the right side of Studio Pro under the **Maia** tab:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/maia-explain/maia-explain-interface.png" width="300px">}}

The initial answer consists of two parts: 

* **Explanation** - provides a high-level overview of the logic, including its overall purpose
* **Technical Highlights** - highlights the technical and functional details of the logic, including any input parameters and return value, if available 

You can ask follow-up questions for further clarification or suggestions to improve the logic. For example, you could type in the chat: *Can you improve this microflow?*.

{{% alert color="info" %}}
In this dedicated chat, only requests related to Maia Explain will be properly handled. If you have other questions, close this chat and go back to the general [Maia Chat](/refguide/maia-chat/) interface.
{{% /alert %}}

## Read More

* [Maia Chat](/refguide/maia-chat/)