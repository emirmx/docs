---
title: "Maia Make Capabilities"
url: /refguide/maia-make/
weight: 4
description: "Describes Maia Make capabilities in Mendix Studio Pro."
description_list: true
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction 

{{% alert color="info" %}}
Maia Make capabilities are available in Studio Pro 11.8 and above.

To use Maia Make capabilities, an internet connection and signing in to Studio Pro are required.
{{% /alert %}}

Maia Make is a set of AI-assisted development capabilities in Studio Pro that are available through a unified conversational interface. Describe your requirements in natural language, and Maia generates development artifacts such as data structures, pages, and microflows. 

You can also ask Maia to provide explanations of your existing documents, such as microflows, workflows, and pages. Moreover, this interface allows you to integrate with external tools, such as Playwright and Figma, via MCP Servers, and it supports story-based development by generating app artifacts based on existing user stories.

## Maia Capabilities Overview

The following table lists the major Maia Make capabilities, their descriptions, and the Studio Pro versions in which they were introduced as part of Maia Make:

| Capability | Description | Available in Maia Make from |
| --- | --- | --- |
| [Maia Chat](/refguide/maia-chat/) | Answers questions about all aspects of Mendix development. | Studio Pro 11.8 |
| [Maia Explain](/refguide/maia-explain/) | Explains the purpose and logic of existing documents such as microflows and pages. | Studio Pro 11.8 |
| [Maia for Domain Model](/refguide/maia-for-domain-model/) | Generates and explains domain models. | Studio Pro 11.8 |
| [Maia for Pages](/refguide/maia-for-pages/) | Generates pages and widgets from text or image input. | Studio Pro 11.8 |
| [Maia for Microflows](/refguide/maia-for-microflows/) | Generates microflow logic from natural language descriptions. | Studio Pro 11.8 |
| [Maia for Workflows](/refguide/maia-for-workflows/) | Generates workflows from natural language or image input. | Studio Pro 11.9 |
| [Maia for OQL](/refguide/maia-for-oql/) | Generates and manages OQL queries. | Studio Pro 11.9 |
| [Maia MCP Client](/refguide/maia-mcp/) | Connects Maia to external MCP servers, giving it access to third-party tools during chat. | Studio Pro 11.8 |
| [Studio Pro MCP Server](/refguide/studio-pro-mcp-server/) | Exposes Studio Pro as an MCP server for use by external AI tools. | Studio Pro 11.10 |
| [Maia Web Fetch](/refguide/maia-web-fetch/) | Fetches and reads content from public websites and APIs during chat. | Studio Pro 11.10 |
| [Maia Agent Skills](/refguide/maia-agent-skills/) | Extends Maia with reusable, domain-specific knowledge that applies automatically when relevant. | Studio Pro 11.11 |

In addition to the core capabilities listed above, Maia Make includes the following features:

| Capability | Description | Available in Maia Make from |
| --- | --- | --- |
| Story-based development | Generates app artifacts based on existing user stories to support story-driven development workflows. | Studio Pro 11.8 |
| PDF/image support | Allows you to provide PDFs and images as input to help Maia better understand your requirements. | Studio Pro 11.8 |
| Adding documents as context | Lets you add relevant documents, such as microflows and pages, to provide Maia with additional context during chat. | Studio Pro 11.8 |
| Editing existing documents | Enables Maia to modify existing documents, including renaming elements such as entities, attributes, and microflow parameters. | Studio Pro 11.8 |
| Removing elements | Allows Maia to remove elements from documents to support more advanced refactoring tasks. | Studio Pro 11.9 |
| Undo support | Allows you to undo Maia-generated changes on a per-document basis. | Studio Pro 11.9 |

### Other Supported Document Types

In Studio Pro 11.9, support for enumerations, constants, modules, Java actions, and JavaScript actions was added. In this version, Java actions are read-only; they can be used in microflows and explained. JavaScript actions can only be explained.

Starting with Studio Pro 11.10, Maia can generate JavaScript actions, add parameters to existing ones, and create or update the JavaScript file associated with a JavaScript action.

### Support for Folder Structure

In Studio Pro 11.10 and above, Maia understands and leverages the existing folder structure within your Mendix applications for all documents except for pages. This enables Maia to:

* Organize documents into folders: When creating new documents, Maia can place them directly into relevant folders, respecting your project's organization.
* Follow existing folder structures: Maia works within your established folder hierarchy, making it easier to maintain consistency.
* Adhere to Mendix best practices: Maia can help organize documents according to the standard Mendix best practices for folder structure. For detailed guidance on optimal organization of folders, refer to the [Folder Structure](/refguide/naming-convention-best-practices/#folder-structure) section in *Naming Convention Best Practices*.

## Using Maia Make Capabilities

Maia Make capabilities are enabled by default. You can disable them in Studio Pro **Preferences**, via the **Maia** tab.

To access the conversational interface and Maia Make capabilities, in the upper-right corner of Studio Pro, click the **Maia** pane. It appears under the **Chat** tab:

{{< figure src="/attachments/refguide/mendix-ai-assistance/maia-make/maia-make-interface.png" max-width=40% alt="Maia Make interface" >}}

Alternatively, you can also click **View** at the Studio Pro top bar and select **Maia** to open the interface.

{{% alert color="info" %}}
In Studio Pro 11.7 and below, the **Chat** tab is only used for [Maia Chat](/refguide/maia-chat/) where you can ask questions about all aspects of Mendix.

There is also the **Learn** tab under the **Maia** pane. It is a separate Maia capability that is not part of Maia Make capabilities. For more information, see [Maia Learn](/refguide/maia-learn/).
{{% /alert %}}

{{% alert color="warning" %}}
The Maia ({{% icon name="sparkles" %}} ) icon on the right side of the top bar does not work in Studio Pro 11.8.
{{% /alert %}}

### Maia Make Capabilities Interface Overview

The conversational interface includes the following options:

* **New chat** - It allows you to clear the messages and start a new conversation which does not reference your current chat.
* **Configure MCP Connections** ({{% icon name="plug" %}} icon) - It allows you to connect external [MCP](https://modelcontextprotocol.io/introduction) servers to Maia, giving it access to third-party tools during chat. For more information on how to configure MCP connections, see [Maia MCP Client](/refguide/maia-mcp/).
* **{{% icon name="paperclip" %}} Add** (Image, Story, PDF) - With this option, you can attach images, PDFs, or user stories to help Maia understand your requirements better.
* **Add file to Maia Chat** (@ icon) - It allows you to add certain logic or pages to Maia as context. You can also access this option by right-clicking the documents (microflows or pages) in the **App Explorer** and it appears in the context menu.

## Read More

* [Mendix AI Assistance (Maia)](/refguide/mendix-ai-assistance/)
