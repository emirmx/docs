---
title: "Maia Plan as MCP Server"
url: /developerportal/maia-plan-mcp/
description: "Describes how to configure and use Maia Plan as an MCP server to retrieve project scope and solution content."
weight: 10
---

## Introduction

The Maia Plan MCP Server provides read-only access to selected Maia Plan artifacts through the Model Context Protocol (MCP). External MCP clients can retrieve plan content in Markdown format, including project scope and project solution, including epics and stories.

The MCP server has the following key characteristics:

* Read-only interface – No create, update, or delete operations are supported.
* Project-level access control – Each request is validated against user permissions, so users can only retrieve data for projects they are allowed to view.
* Focused tool set – Three tools are available:

    * Find a plan using search terms (returns a plan UUID for follow-up calls).
    * Get project scope as Markdown.
    * Get project solution as Markdown.
* Per-project retrieval – Each call targets a specific project identified by UUID.

## Prerequisites

Before you can use the Maia Plan MCP Server, you need the following:

* Access to at least one Maia Plan project in the Mendix Portal.
* A personal access token (PAT) with the appropriate scopes. For details on how to create and configure your PAT, refer to [Setting Up Your Personal Access Token](/apidocs-mxsdk/mxsdk/set-up-your-pat/).
* An MCP client capable of connecting to external servers, such as Claude Code or a custom MCP implementation.

## Configuring the MCP Server {#configure}

To connect your MCP client to the Maia Plan MCP Server, configure the server URL and authentication method.

### Server Configuration

Configure your MCP client with the following server details:

* **Protocol** – The server uses the standard MCP protocol over HTTPS.
* **URL** – Use the Maia Plan MCP Server endpoint URL provided by Mendix.

### Authentication {#authentication}

The Maia Plan MCP Server requires authentication using a personal access token (PAT).

Create a PAT with the required scopes for Maia Plan access. Consult your Mendix administrator if you are unsure which scopes are needed.    
For instructions on creating a PAT, refer to [Setting Up Your Personal Access Token](/apidocs-mxsdk/mxsdk/set-up-your-pat/).

Store your PAT securely. Do not hardcode credentials into scripts or configuration files that may be committed to version control.

The recommended approach is to save your PAT as an environment variable. For example:

* On macOS and Linux, add the following line to your shell profile (`~/.bashrc`, `~/.zshrc`, or similar):

    ```bash
    export MENDIX_TOKEN="your-personal-access-token-here"
    ```

* On Windows, set the environment variable using the System Properties dialog or PowerShell:

    ```powershell
    [Environment]::SetEnvironmentVariable("MENDIX_TOKEN", "your-personal-access-token-here", "User")
    ```

### Configuring Your MCP Client

The steps to configure your MCP client depend on the client you are using. The following sections provide examples for common MCP clients.

#### Configuring Claude Code

To configure Claude Code to connect to the Maia Plan MCP Server, follow these steps:

1. Open your Claude Code configuration file.
2. Add the Maia Plan MCP Server to the list of available MCP servers.
3. Specify the server URL and authentication method (using the `MENDIX_TOKEN` environment variable).
4. Save the configuration file.

For detailed instructions, refer to the [Claude Code documentation](https://docs.anthropic.com/claude/docs/model-context-protocol).

#### Configuring Other MCP Clients

For other MCP clients, refer to the client's documentation for instructions on adding external MCP servers and configuring authentication. Most MCP clients support environment variable-based authentication, which is the recommended approach.

## Available Tools {#tools}

The Maia Plan MCP Server provides three tools for retrieving plan data.

### Find Plans {#find-plans}

Use this tool to search for plans using search terms. The tool returns a plan UUID that you can use in follow-up calls to retrieve plan details.

Tool specifics:

* When to use:

    * You know the project name or other identifying information, but you do not have the project UUID.
    * You want to discover available plans before retrieving plan content.

* Inputs:

    * Search terms – One or more keywords related to the project name, description, or other identifying information.

* Returns:

    * A list of matching plans, each with a plan UUID.

{{% alert color="info" %}}
The tool only returns plans for projects you have permission to view. If you do not have access to a project, it does not appear in the search results.
{{% /alert %}}

### Get Plan Scope {#get-scope}

Use this tool to retrieve the project scope as Markdown. The project scope includes the project goal, success criteria, target users, and requirements.

Tool specifics:

* When to use:

    * You want to review the high-level project objectives and requirements.
    * You need to document or share the project scope outside of Maia Plan.

* Inputs:

    * Plan UUID – The unique identifier for the plan. You can obtain this UUID using the [Find Plans](#find-plans) tool.

* Returns:

    * The project scope in Markdown format.

### Get Plan Solution {#get-solution}

Use this tool to retrieve the project solution as Markdown. The project solution includes all epics and stories defined in the plan.

Tool specifics:

* When to use:

    * You want to review the detailed implementation plan with epics and stories.
    * You need to export the project solution for documentation, reporting, or integration with other systems.

* Inputs:

    * Plan UUID – The unique identifier for the plan. You can obtain this UUID using the [Find Plans](#find-plans) tool.

* Returns:

    * The project solution in Markdown format, including all epics and their associated stories.

## How It Works {#how-it-works}

The following steps describe how the Maia Plan MCP Server processes requests:

1. An MCP client calls one of the available tools and includes an `Authorization` header with your personal access token.
2. The server validates the token and identifies you.
3. The server filters plans based on your access permissions.
4. If you called the [Find Plans](#find-plans) tool, the server returns a list of plan UUIDs for projects you can access.
5. If you called the [Get Plan Scope](#get-scope) or [Get Plan Solution](#get-solution) tool, the server validates that you have access to the specified project and returns the requested Markdown content.

## Security and Access Control {#security}

The Maia Plan MCP Server enforces the following security measures:

* Authentication required – All requests must include a valid personal access token.
* Project-level authorization – Access is checked for each project. You can only retrieve data for projects you have permission to view in Mendix Portal.
* Read-only access – The MCP server does not support create, update, or delete operations. All tools provide read-only access to plan data.
* Unauthorized requests denied – If you attempt to access a project you do not have permission to view, the server returns an error.
