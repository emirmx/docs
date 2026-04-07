---
title: "Agent Editor"
url: /appstore/modules/genai/genai-for-mx/agent-editor/
linktitle: "Agent Editor"
description: "Describes the purpose, configuration, and usage of the Agent Editor and Agent Editor Commons modules from the Mendix Marketplace that allow developers to build, define, and refine agents, and integrate GenAI principles and agentic patterns into their Mendix app."
weight: 20
---

## Introduction

The [Agent Editor](https://marketplace.mendix.com/link/component/257918) module enables users to develop, test, and optimize their GenAI use cases by creating effective agents that interact with large language models (LLMs).
With the Agent Editor module, you can define agents at design time in Studio Pro (11.9.0 and higher), and manage their lifecycle as part of your project, by taking advantage of existing platform capabilities such as model documents, version control, and deployment capabilities. Agents can be defined and developed locally and subsequently deployed to cloud environments directly with the app model.

The Agent Editor is compatible with the Agent Commons module: you can define and manage prompts, microflows (as tools), external MCP servers, knowledge bases, and large language models to build agentic patterns that support your business logic. Additionally, it allows you to define variables that act as placeholders for data from the app session context, which are replaced with actual values when the end user interacts with the app.

The Agent Editor module includes a Studio Pro extension that allows users to define GenAI Agents as documents in the app model. The Agent Editor Commons module, which is installed as part of the same package, includes logic and activities to call these agents from microflows in a running application.


  {{% alert color="info" %}}
The Agent Editor will become available shortly after the Mendix Studio Pro 11.9 release as a downloadable extension on the Mendix Marketplace. Click 'Add to Saved' on the [Marketplace listing](https://marketplace.mendix.com/link/component/257918) and stay tuned for updates!
  {{% /alert %}}



### Typical Use Cases


### Features
The Agent Editor helps teams design, test, and ship agents as part of their app lifecycle in Studio Pro.

It provides the following features:

* Agent-specific Studio Pro documents for agent definitions and related dependencies, including text generation models, knowledge bases, and MCP services.
* Prompt authoring with placeholder support, so runtime values from user or context objects can be injected during execution.
* Tool and knowledge base configuration directly in the Agent editor, including temporary activation toggles for fast iteration and comparison.
* Built-in local test functionality from Studio Pro to validate prompts and agent behavior before release.
* Microflow integration through the **Call Agent** toolbox action in the **Agent Editor** category.
* Agent definitions as app-model documents under version control, making changes traceable and allowing rollback to previously committed states when needed.
* Deployment together with the app model, with environment-specific flexibility through constant overrides.

### Dependencies {#dependencies}


## Installation

If you are starting from a blank app or adding agent-editing functionality to an existing project, you need to manually install the [Agent Editor](https://marketplace.mendix.com/link/component/257918) package from the Mendix Marketplace. 
Before proceeding, ensure your project includes the latest versions of the required [dependencies](#dependencies). Follow the instructions in [How to Use Marketplace Content](/appstore/use-content/) to install the Agent Editor. 

After installation, two modules are added to your app:

* **Agent Editor** under **Add On modules** in the **App Explorer**. This module contains the Studio Pro extension that adds the new document types and editors.
* **Agent Editor Commons** under **Marketplace modules** in the **App Explorer**. This module contains the logic to call agents from microflows.

The detailed functionality of these modules is explained in the following sections of this page.


## Configuration
To use the Agent Editor functionalities in your app, you must perform the following tasks in Studio Pro:

1. Define the model.
2. Define the agent with a prompt, context entity and model settings.
3. Define and add tools and knowledge bases.
4. Test the agent.
5. Include the agent in the app logic.
6. Deploy the agent to cloud environments.
7. Improve the agent in next iterations.



### Define the model.

In Agent Editor, you define the model as a document in your app model. This model can then be linked to one or more agents in your project.

Defining a model document is mandatory. Without a model document, the agent you configure in the next steps cannot run.

At this moment, only models of the Mendix Cloud GenAI type are supported.

To define a model, perform the following steps:

1. In the **App Explorer**, right-click your module (for example, `MyFirstModule`) and select **Add other** > **Model**.
2. The Model editor opens directly after creating the model document.
3. In the **App Explorer**, create or locate a constant in the same module (for example, `MyFirstModule`) by selecting **Add other** > **Constant**.
4. Ensure this constant contains the key from your Text Generation resource in the Mendix Portal.
5. In the Model editor, select this constant in **Model key**.
6. After selecting the constant, verify that model data is imported.
7. In the **Connection** section of the Model editor, click **Test** to validate the connection from Studio Pro to the model resource.

{{% alert color="info" %}}
The value you use for the Text Generation key constant in Studio Pro can be different from the value used in cloud environments. Constant values can be overridden per environment during deployment.
{{% /alert %}}


### Define the agent with a prompt, context entity and model settings.

After defining the model, define the agent document and configure the prompts and context. This configuration is mandatory for the agent to run.

To define the agent, perform the following steps:

1. In the **App Explorer**, right-click your module and select **Add other** > **Agent**.
2. In the Agent editor, select the model by clicking **Select**. This opens a document selection dialog where you can choose from the model documents in your app.
3. In **System prompt**, enter the system instructions for the agent.
4. In **User prompt**, enter the concrete task for the agent that is used during each execution.
5. While writing prompts, include placeholders for variables by using double braces, for example, `{{variable}}`. The actual values of these placeholders are typically known at runtime based on the page context.
6. If placeholders are used, select the **Context entity**. This opens an entity selection dialog in Studio Pro where you can select the entity that provides the placeholder values at runtime.
7. Optionally, click **Edit** next to **Model settings** to configure parameters such as maximum tokens, temperature, and TopP, which control response length and output randomness. Consult the documentation of your model provider for the allowed ranges.

{{% alert color="info" %}}
Both **System prompt** and **User prompt** are currently mandatory because Agent Editor currently supports task-based agents only. Chat-based agents will be supported by the Agent Editor in a future release.
{{% /alert %}}

For more information about prompts and prompt engineering, see [Prompt Engineering](/appstore/modules/genai/prompt-engineering/).

Selecting a model is mandatory. You can save the document, but if model configuration is incomplete, Studio Pro will show consistency errors. These errors block running the app locally, cloud deployment, and agent testing in later steps.


### Define and add tools and knowledge bases.

To extend the capabilities of your agent, you can add tools directly in the Agent editor. In the Agent Editor, microflows and (external) MCP services can be added as tools to let the agent act dynamically and autonomously, or to access specific data based on input it determines. When the agent is invoked, it uses the function calling pattern to execute the required microflow by using the input specified in the model response. For more technical details about microflow tools and function calling behavior, see [Function Calling](/appstore/modules/genai/function-calling/).

#### Configure Consumed MCP Service

To use MCP tools, first create a consumed MCP service document in your module by selecting **Add other** > **Consumed MCP service** in the **App Explorer**.

In the consumed MCP service document, configure the following fields:

* **Endpoint**: This is the URL where the server can be reached. Create or select the constant that contains your MCP endpoint.
* **Credentials microflow** (optional): Select this when the server requires authentication. The microflow must return a list of `System.HttpHeader` objects. Input parameters are not allowed.
* **Protocol version**: Select the version used by your server. Typical values are `v2025_03_26` for MCP servers and `v2024_11_05` for SSE-type servers.

To validate the configuration, click **List tools** in the **Tools** section of the consumed MCP service document. If the connection succeeds, the list of exposed tools is shown.

In the consumed MCP service playground, authentication headers are used only to explore tools from Studio Pro and are not stored.

#### Add Tools to the Agent

Tools can then be added in the **Tools** section of the Agent editor by clicking **New** and selecting a tool type.

You can choose from the following tool types:

* **Microflow tool**: Select a microflow that returns a string. Provide a **Name** and **Description** so the LLM can determine when to use the tool.
* **MCP tool**: Select a consumed MCP service in the tool configuration.

In the Agent editor, tools can be temporarily disabled and re-enabled by using the **Active** checkbox. This is useful while iterating and testing the agent behavior with different tool combinations or descriptions. Only enabled tools will be usable by the agent at runtime when called in the app.

#### Configure Knowledge Base Document

Knowledge bases are configured as separate documents and can then be linked to agents.

To configure a knowledge base, create the document in your module by selecting **Add other** > **Knowledge base** in the **App Explorer**.

At this moment, only Mendix Cloud GenAI knowledge bases are supported.

In the Knowledge base editor:

* Set **Knowledge base key** by creating or selecting a constant in your module.
* After selecting the key, verify that knowledge base details are imported and shown.
* Optionally, click **List collections** to test the connection and display the available collections from the knowledge base resource in **Configured Collections**.

#### Link Knowledge Bases to the Agent

To link a knowledge base to an agent, use the **Knowledge bases** section in the Agent editor and click **New**.

In the knowledge base entry:

* Select the configured knowledge base document in **Knowledge base**.
* In **Collection**, select one of the available collections from the dropdown, or type/paste a collection name to reference a collection that does not exist yet.
* Provide **Name** and **Description** so the LLM can determine when this knowledge base should be used. This serves the same purpose as naming for tools.
* Optionally configure retrieval settings:
  * **Max results** controls the maximum number of chunks returned in a single retrieval.
  * **Min similarity** sets the cosine-similarity threshold between 0 and 1. Higher values (for example, 0.8) are stricter than lower values (for example, 0.2).

Knowledge base links can also be temporarily disabled and re-enabled by using the **Active** checkbox, which helps when comparing retrieval behavior during rapid iteration. Only enabled knowledge bases will be usable by the agent at runtime when called in the app.

{{% alert color="info" %}}
In this release, MCP tools support whole-server integration only. Selecting individual tools from the server is not yet supported.
{{% /alert %}}


### Test the agent.

The Agent editor provides a **Test** button to execute test calls by using your local app runtime.

Testing is available when the following conditions are met:

* The app model has no consistency errors in Studio Pro (as shown in the **Errors** pane).
* The app is running locally.
* The model configured in the model document is reachable.
* Values are provided for all placeholders so a concrete test case can be constructed.

If you change the agent definition (for example, by updating the system prompt or adding/removing tools), restart the local app runtime before testing again. The Agent editor provides a UI indication for this, but it is recommended to account for it explicitly while iterating.

When these conditions are met, you can use the test functionality to validate prompt behavior and configuration before integrating the agent into app logic.

If a call fails during testing, a generic error message is shown in the Agent editor UI. Detailed error information is available in the running app console in Studio Pro (the **Console** pane), similar to errors you would inspect while testing the app UI itself.


### Include the agent in the app logic.

Including an agent in the app logic is done by calling it from a microflow. The Agent Editor provides one toolbox action for this: **Call Agent** in the **Agent Editor** category. This action is currently focused on single-call, task-style execution.

When configuring the action, select the Agent document so that the right agent is called. If your prompts use variable placeholders, pass a context object to the action. This object must be of the selected context entity type so placeholders can be resolved at runtime.

Optionally, you can pass a `Request` object to set request-level values, and a `FileCollection` object with files to send along with the user message. Support for files and images depends on the underlying large language model. Refer to the documentation of the specific connector.

The output is a `GenAICommons.Response` object, aligned with the GenAI Commons and Agent Commons domain models and actions, which can be used for further logic.


### Deploy the agent to cloud environments.

Agents created with Agent Editor are documents in the app model. This means they are packaged and deployed together with the rest of the app whenever a deployment is performed.

Environment-specific flexibility is provided through constants. Values such as the model key, knowledge base key, or custom MCP endpoint can be overridden per app environment. For details, see [Environment Details: Constants](https://docs.mendix.com/developerportal/deploy/environments-details/#constants).

Agents created in Studio Pro (using Agent Editor) are visible in the Agent Commons UI, but they are not editable there.


### Improve the agent in next iterations.

To change agents, update the agent (and related) documents in the app model in Studio Pro and deploy the app to the cloud node again for the changes to take effect.

To return to historical agent versions, use version control to inspect previously committed states of the agent document and related documents. This allows you to compare changes over time and restore an earlier configuration when needed.


## Troubleshooting