---
title: "Snowflake ConversationalUI Bridge"
url: /appstore/connectors/snowflake/snowflake-conversationalui-bridge/
description: "Describes the configuration and usage of the Mendix-Snowflake AI Data Connector from the Mendix Marketplace." 
weight: 20
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## Introduction

The [Snowflake ConversationalUI Bridge module](https://marketplace.mendix.com/link/component/225717) provides a way to implement the Conversational UI for Snowflake Cortex Analyst.

### Typical Use Cases

When using the Snowflake AI Data Connector and want to implement Conversational UI for Snowflake Cortex Analyst one can use the SnowflakeConversational UI Bridge module to easily implement Conversational UI for Cortex Analyst.

* Authentication:

    * Authentication with an RSA key pair according to PKCS #8 standard
    * Authentication with OAUTH through an OIDC provider or PAT -> You will need to add your OAuth token or PAT and optionally its expiration date to the BearerToken created in the "ChatCompletions_CallLLM" microflow. 

* Functionality: Easily implement chat interfaces for Cortex Analyst using Conversational UI:

    * Test page "SnowflakeConfig_TestingPage" for the different chat interfaces provided by conversational UI. Acts a a quick testing page and an Example of how one could set up a chat interface for Cortex Analyst using ConversationalUI. Simply add the page to your project navigation and start exploring.
    * CortexAnalystDeployedModel overview page "CortexAnalystDeployedModel_Overview". Used to configure your Contex Analyst implementations by creating CortexAnalystDeployedModels. Needs SnowflakeAIDataConnector.ConnectionDetails object.
    * Snippet for Cortex Analyst Model configuration page 'Snippet_ModelConfig'. Needs SnowflakeAIDataConnector.ConnectionDetails object.
    * Example page "FullScreenChat_HistoryAndProviderSelection" for full screen chat with history bar and provider selection.
    * Action Microflow: 'ChatCompletions_CallLLM' for the "CortexAnalystDeployedModel" which is set in 'CortexAnalystDeployedModel_Create'. Handels calls to Cortex Analyst and maps GenAICommons request to Cortex Analyst request and Cortex Analyst response to GenAICommons response.
    * Action Microflow: 'ChatContext_ChatWithHistory_ActionMicroflow' for the "ConversationalUI.ProviderConfig" which is set in 'SnowflakeConversationalUIBridge.ProviderConfig_GetCreate'. Does some pre and post processing creating Conversational UI Messages, References, etc.


### Prerequisites {#prerequisites}

The Snowflake Conversational UI Bridge Module requires Mendix Studio Pro version 10.24.13 or above.

To use the Snowflake Conversational UI Bridge Module, you must also install and configure the following modules from the Mendix marketplace:

* [Snowflake AI Data Connector](https://marketplace.mendix.com/link/component/225717) – This module and its dependencies are a required dependency for Snowflake Conversational UI Bridge Module.
* [Conversational UI](https://marketplace.mendix.com/link/component/239450) – This module and its dependencies are a required dependency for Snowflake Conversational UI Bridge Module.

### Licensing and Cost

This connector is available as a free download from the Mendix Marketplace, but the services in Snowflake to which is connects may incur a usage cost. For more information, refer to the [Snowflake documentation](https://www.snowflake.com/en/data-cloud/pricing-options/).

Depending on your use case, your deployment environment, and the type of app that you want to build, you may also need a license for your Mendix app. For more information, refer to [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/).

## Installation

Follow the instructions in [How to Use Marketplace Content in Studio Pro](/appstore/general/app-store-content/) to import the Snowflake Conversational UI Bridge module into your app.

## Configuration

After you install the Snowflake Conversational UI Bridge Module, you can find it in the **App Explorer**, in the **Marketplace modules** section. The connector provides a domain model and several Action microflows and snippets that you can use. 


### Example Implementation

 The [Snowflake showcase app](https://marketplace.mendix.com/link/component/225845) contains example implementations of the Cortex Analyst Conversational UI chat implementations.For more information, see [Snowflake Cortex Analyst](/appstore/modules/genai/snowflake-cortex/#functionalities).