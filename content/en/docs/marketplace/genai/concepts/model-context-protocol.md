---
title: "Model Context Protocol (MCP)"
url: /appstore/modules/genai/mcp/
linktitle: "Model Context Protocol (MCP)"
weight: 50
description: "This document describes the Model Context Protocol (MCP) and how it is used in Mendix."
---

## Introduction

The [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) is an open protocol that standardizes how Large Language Models (LLMs) can autonomously connect to apps. Many AI platforms and third-party systems have already adopted MCP for easier integration and empowerment of LLMs. Mendix provides an [MCP Server](/appstore/modules/genai/genai-for-mx/mcp-server/) module to facilitate a MCP server from a Mendix app, enabling developers to expose tools and prompts to external MCP clients.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/mcpserver/mcp-client-server-architecture.jpg" >}}

As described in the diagram above, a user can chat with a model in the MCP Host application (for example, Claude Desktop), which connects to the MCP Server (a Mendix app) via MCP to discover available tools and prompts. If the user's request can be solved with any available tool, the LLM can call a tool which is executed in the Mendix app and returns the result. This approach allows developers to integrate their Mendix app with existing external AI systems.

MCP provides a vendor-agnostic approach to integrate third-party services with LLM interaction to enrich its context beyond training data and the current chat conversation. In Mendix, this enables developers to build microflows which can be exposed as tools to external AI tools and thus creating powerful AI-agentic systems using low-code.

## Terminology

To understand the basics of MCP, it is important to know the common terminology.

### MCP Host

The MCP host is typically the application that facilitates interaction with LLMs. While a chat interface is the most common use case, the host can support a variety of interaction use cases. The host takes care of the communication between users and models, while enabling users to manage their AI use, for example, managing credentials or historical chat conversations.

### MCP Client

The MCP client usually runs inside of the MCP Host and is responsible of connecting to one MCP server outside of the MCP host application. The client follows the MCP principles and should technically be able to connect to any MCP server. A host can maintain multiple clients at the same time and thus connecting to multiple MCP servers.

### MCP Server

MCP server exposes resources that can be discovered by MCP clients and made available to both the model and the user during a chat interaction. Once a request is made, the server processes it using its built-in logic and returns a value for the MCP host to use.

Servers can expose the following three main types of resources:

#### Tools

The most common type of resource is tool, which exposes executable functionality such as microflows to clients. Those can not only contain simple retrieve logic to get information from a database, but also perform actions on behalf of the user, for example, submitting a report or changing values. Typically, a model in the MCP host chooses when to call a tool based on the context of the chat.

#### Prompts

Prompts can be exposed to define templates and to structure interactions between the user and the model, for example, by allowing specific input fields that fill placeholders. In Mendix MCP servers, prompts work similarly to tools, as they also execute a microflow. Both end-users and LLMs choose when to use a prompt based on the prompt's definition.

#### Resources

In general, generic resources, such as files or database records, can be exposed via MCP. There are no guidelines on how to use resources on the client side; for example, if a user first needs to add them to a chat or if they are automatically incorporated to the chat context. Currently, Mendix does not support resources to be exposed.

## MCP Example in the GenAI Showcase App

An example for setting up an MCP server is available in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475). The app guides you through available tools and prompts that will be exposed when the server is started. Additionally, it shows an example for setting up authentication using username and password. Claude Desktop is used as an example MCP host to demonstrate the client-server communication. However, other MCP hosts, such as GitHub Copilot, can also be used.

## Read More

* The [MCP Server module](/appstore/modules/genai/genai-for-mx/mcp-server/)
* The [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) provides an example on how to expose microflows as tools via the MCP Server module. 
* The official [MCP docs](https://modelcontextprotocol.io/introduction)
* The [MCP Java SDK GitHub Repository](https://github.com/modelcontextprotocol/java-sdk)
* A blog post on [How to use MCP to bring Mendix Business Logic into Claude for Desktop](https://www.mendix.com/blog/how-to-use-mcp-to-bring-mendix-business-logic-into-claude-for-desktop/)
