---
title: "Connect a Mendix AI Agent to a Snowflake-Managed MCP Server"
linktitle: "AI Agents for a Snowflake MCP Server"
url: /appstore/modules/snowflake/connect-ai-agent-to-snowflake-mcp/
description: "Describes the steps required to use a Snowflake-managed MCP server with a Mendix AI ageint."
weight: 80
---

## Introduction

[Snowflake-managed MCP servers](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp) let AI agents securely retrieve data from Snowflake accounts without needing to deploy separate infrastructure. Mendix users can configure the [MCP Client Module](/appstore/modules/genai/mcp-modules/mcp-client/) to enable the connection from a Mendix AI agent to a Snowflake MCP server.

### Typical Use Cases

[What do we want to achieve?]

### Prerequisites {#prerequisites}

[Any specific versions of Studio Pro? Other prereqs?]

To establish a connection between a Mendix AI Agent and a Snowflake-managed MCP server, you must also install and configure the following modules and starter apps and their prerequisites:

* [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369)
* 

## Configuring the Mendix Email Connector for Amazon SES

To configure your SES account in the Email Connector in Studio Pro, follow these steps: 

1. Get the following details from Amazon SES: 
    * SMTP hostname 
    * SMTP username 
    * SMTP password

    For more information, see [Obtaining Amazon SES SMTP credentials](https://docs.aws.amazon.com/ses/latest/dg/smtp-credentials.html).

    {{% alert color="info" %}}Only email IDs and identities configured under Verified identities, and that are verified for Amazon SES accounts, can be used as sender and receiver.{{% /alert %}}

2. Download the Email Connector module and import it into your Studio Pro app. For more information, see [Email Connector](/appstore/modules/email-connector/).

    {{% alert color="warning" %}}Ensure that you follow the prerequisites listed in the [Email Connector documentation](/appstore/modules/email-connector/). Missing a step might lead to errors.{{% /alert %}}

3. Set up the Email Connector. For more information, see [Setting Up the Email Connector in Studio Pro](/appstore/modules/email-connector/#setup).  
4. On the **EmailConnector_Overview** page, click **Add email account**. 
5. Enter the following details: 
    * **Email** - SMTP username for Amazon SES 
    * **Password** -  SMTP password for Amazon SES 
6. Click **Next**.
7. Click **OK** to manually configure your email account. 
8. Select the **Send emails** checkbox, and then enter the following details: 
    * **Protocol** - SMTP 
    * **Server host** - enter SMTP hostname for Amazon SES 
    * **Server port** - any configured STARTTLS port for Amazon SES (for example, 25, 587, 2587, and so on) 
    * Select Use TLS / Use SSL accordingly 
9. Click **Finish**. 
