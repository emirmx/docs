---
title: "Maia MCP Server"
linktitle: "MCP Server"
url: /refguide/maia-mcp-server/
weight: 80
description: "Describes the features in Maia MCP Server."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

{{% alert color="info" %}}
This feature is available in Studio Pro 11.10 and above. 

To use Maia MCP Client, an internet connection and signing in to Studio Pro are required.
{{% /alert %}}

The Maia MCP Server in Studio Pro enables you to leverage Maia's capabilities directly from external clients, including AI Coding Assistants, Agents, and other MCP-based LLM tools.

## Enabling the MCP Server

To enable the MCP Server, navigate to **Preferences** > **Maia** > **MCP Server** and check **Enable MCP Server**. From this menu, you can also configure the port to use.

### Connecting External Clients

Once enabled, you can connect external clients to the MCP Server. For example, to add it to Claude Code, run the following command:
    
`claude mcp add <name> --transport http http://localhost:<port>/mcp`

Replace `<name>` with your preferred server name and `<port>` with the port configured in **Preferences**.

## Key Highlights

* Full Maia feature parity — The MCP Server exposes the same capabilities as Maia within Studio Pro.
* Live updates — Any changes made through Maia via the MCP Server are reflected in real time within Studio Pro.

## Read More

* [Mendix AI Assistance (Maia)](/refguide/mendix-ai-assistance/)
* [Maia Chat](/refguide/maia-chat/)