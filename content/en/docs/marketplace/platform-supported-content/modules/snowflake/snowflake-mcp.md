---
title: "Connect a Mendix AI Agent to a Snowflake-Managed MCP Server"
linktitle: "Connect Mendix to a Snowflake MCP Server"
url: /appstore/modules/snowflake/connect-ai-agent-to-snowflake-mcp/
description: "Describes the steps required to use a Snowflake-managed MCP server with a Mendix AI ageint."
weight: 80
---

## Introduction

The Model Context Protocol (MCP) is an open protocol that standardizes how Large Language Models (LLMs) can autonomously connect to apps. Many AI platforms and third-party systems have already adopted MCP for easier integration and empowerment of LLMs. Mendix provides an MCP Server module to facilitate an MCP server from a Mendix app, as well as an MCP Client module. For more information, see [Model Context Protocol (MCP)](/appstore/modules/genai/mcp/).

[Snowflake-managed MCP servers](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp) let AI agents securely retrieve data from Snowflake accounts without needing to deploy separate infrastructure. Mendix users can configure the [MCP Client Module](/appstore/modules/genai/mcp-modules/mcp-client/) to enable the connection from a Mendix AI agent to a Snowflake MCP server.

### Typical Use Cases

[What do we want to achieve?]

### Prerequisites {#prerequisites}

[Any specific versions of Studio Pro? Other prereqs?]

To establish a connection between a Mendix AI Agent and a Snowflake-managed MCP server, you must also install the following modules and their prerequisites:

* [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369)
* [MCP Client](https://marketplace.mendix.com/link/component/244893)
* [MCP Server](https://marketplace.mendix.com/link/component/240380)

## Preparing a Snowflake-Managed MCP Server

To configure a Snowflake-managed MCP server, follow these steps: 

1. In Snowflake, set up a database and schema which will be used by the server.
2. Create the following stored procedures which the MCP server will expose as tools:

    * `GET_SCHEMA_METADATA`
    * `RETRIEVE_RECORDS`
    * `INSERT_RECORD`

3. Create the MCP server. For more information, see [Create an MCP Server object](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp#create-an-mcp-server-object) in Snowflake documentation.
4. Create the authentication and access configuration, so it can invoked by Mendix.

    1. Retrieve the IP addresses.
    2. Create a `NETWORK RULE` using the IP addresses that you retrieved.
    3. Create a `NETWORK POLICY`.
    4. Set the user to use this policy.
    5. Create a Personal Access Token (PAT) for the user.

## Connecting a Mendix Agent to the MCP Server

After preparing the MCP server, you can now create a Mendix AI agent and connect it to the server by performing the following steps:

1. In Studio Pro, create a new app using the [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369).
2. In the [MCP Client](/appstore/modules/genai/mcp-modules/mcp-client/), add the credentials for your Large Language Model.
3. Create a microflow to retrieve the Snowflake user PAT that you created in the previous section.
4. Add the Snowflake MCP server.
5. Create an AI agent and configure the following properties:

    * LLM
    * Prompt
    * Snowflake-managed MCP server

6. Test your agent and verify that it can connect to the Snowflake-managed MCP server.

## Example

[video link when available]
