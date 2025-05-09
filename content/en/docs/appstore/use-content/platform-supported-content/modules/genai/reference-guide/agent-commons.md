---
title: "Agent Commons"
url: /appstore/modules/genai/genai-for-mx/agent-commons/
linktitle: "Agent Commons"
description: "Describes the purpose, configuration and usage of the Agents Commons module from the Mendix Marketplace that allows developers to build, define and refine Agents, to integrate GenAI principles and Agentic patterns into their Mendix app."
weight: 20

---

## Introduction

The [Agent Commons](https://marketplace.mendix.com/link/component/239450) module enables users to develop, test, and optimize their GenAI use cases by creating effective agents that interact with large language models (LLMs).
With the Agent Commons module, you can use the Agent Builder interface within your app to define agents at runtime and manage multiple versions over time.

You can wire up prompts, microflows (as tools), knowledge bases, and large language models to build agentic patterns that support your business logic. The Agent Builder also allows you to define variables that act as placeholders for data from the app session context which are replaced with actual values when the end user interacts with the app.

The Agent Commons module includes the necessary data model, pages, and snippets to seamlessly integrate the agent builder interface into your app and start using agents within your app logic.

### Typical Use Cases

Typical use cases for Agent Commons include:

* Incorporating one or more agentic patterns in the app that involve interactions with a LLM. These patterns may also include microflows as tools, knowledge bases, and guardrails.

* Enabling prompt updates or improvements without modifying the underlying LLM integration code or low-code application logic. This allows non-developers, such as data scientists to change prompts and iterate on agent configurations.

* Supporting rapid iteration on prompts, microflows, knowledge bases, models, and variable placeholders in a playground setup, separate from core app logic.

### Features

The Agent Commons module offers the following features:

* Agent Builder UI components and data model for managing, storing, and rapidly iterating on agent versions at runtime. No app deployment is required to update an agent.

* Drag and drop operations for calling both single-call and conversational agents from microflows and workflows.

* Prompt placeholders, allowing dynamic insertion of values based on user or context objects at runtime.

* Logic to define and run tests individually or in bulk, with result comparisons.

* Export/import functionality for transporting agents across different app environments (for example, local, acceptance, production).

* The ability to manage the active agent version used by the app logic in the app environment, eliminating the need for redeployment.

{{% alert color="info" %}} The current scope of the module focuses on LLM invocations using a variety of prompts, optionally enhanced with placeholders (variables). Agents can be further extended by integrating microflows with a single parameter as tools using the [Function Calling](/appstore/modules/genai/function-calling/) setup, and by connecting to knowledge bases provided through [Mendix Cloud GenAI Resources](/appstore/modules/genai/mx-cloud-genai/resource-packs/#knowledge-bases). {{% /alert %}}

### Dependencies {#dependencies}

The Agent Commons module requires Mendix Studio Pro version [10.21.0](/releasenotes/studio-pro/10.21/#10210) or above.

In addition, install the following modules:

* [Community Commons](https://marketplace.mendix.com/link/component/170)
* [GenAI Commons](https://marketplace.mendix.com/link/component/239448)
* [Mendix Cloud GenAI Connector](https://marketplace.mendix.com/link/component/239449)
* [Conversational UI](https://marketplace.mendix.com/link/component/239450)

## Installation

If you are starting from a blank app or adding agent-building functionality to an existing project, you need to manually install the [Agent Commons](https://marketplace.mendix.com/link/component/239450) module from the Mendix Marketplace. 
Before proceeding, ensure your project includes the latest versions of the required [dependencies](#dependencies). Follow the instructions in [How to Use Marketplace Content](/appstore/use-content/) to install the Agent Commons module.

## Configuration

To use the Agent Commons functionalities in your app, you must perform the following tasks in Studio Pro:

1. Assign the relevant [module roles](#module-roles) to the applicable user roles in the project **Security**.
2. Add the [Agent Builder UI to your app](#ui-components) by using the pages and snippets as a basis.
3. Ensure that a [deployed model](#deployed-models) is configured.
4. [Define](#define-prompt), the prompts, add functions, knowledge bases, and test the agent.
5. Add the agent to app the [logic](#app-logic) of your specific use case.
6. Improve and [iterate on agent versions](#improve-agent).

### Configuring the Roles {#module-roles}

In the project **Security** of your app, assign the **AgentCommons.AgentAdmin** module role to user roles responsible for defining and refining agents, as well as selecting the active agent version used in the running app environment.

### Adding the Agent Builder UI to Your App {#ui-components} 

The module includes a set of reusable pages, layouts, and snippets, allowing you to add the agent builder to your app. 

#### Pages and Layouts

To define the agents at runtime, add the **Agent_Overview** page (**USE_ME** > **Agent Builder**) to your app **Navigation**, or include the **Snippet_Agent_Overview** in a page that is already part of your navigation.

From the overview, users can access the **Version_Details** page to edit prompts and run tests. For more customization, you can refer to the contents of **Snippet_Agent_Details**.

If you need to adjust the layout or apply other customizations, it is recommended to copy the relevant page into your own module and modify it to match your app styling or use case. 

For example, download and run the [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369) to see the pages in action.

### Configuring Deployed Models {#deployed-models}

To interact with LLMs using Agent Commons, you need at least one GenAI connector that adheres to the GenAI Commons principles. To test agent behavior, you must configure at least one [Deployed Model](/appstore/modules/genai/genai-for-mx/commons/#deployed-model) for your chosen connector. Refer to the specific connector’s documentation for detailed instructions on setting up the Deployed Model.

* For [Mendix Cloud GenAI](https://marketplace.mendix.com/link/component/239449) importing the **Key** from the Mendix portal automatically creates a MxCloud Deployed Model. This is part of the [configuration](/appstore/modules/genai/mx-cloud-genai/MxGenAI-connector/#configuration).
* For [Amazon Bedrock](https://marketplace.mendix.com/link/component/215042), the creation of Bedrock Deployed Models is part of the [model synchronization mechanism](/appstore/modules/aws/amazon-bedrock/#sync-models).
* For [OpenAI](https://marketplace.mendix.com/link/component/220472), the configuration of OpenAI Deployed Models is part of the [configuration](/appstore/modules/genai/reference-guide/external-connectors/openai/#general-configuration).

### Define the Agent {#define-prompt}

When the app is running, a user with the `AgentAdmin` role can set up agents, write prompts, link microflows as tools, and provide access to knowledge bases. Once an agent is associated with a deployed model, it can be tested in an isolated environment, separate from the rest of the app’s logic to validate its behavior effectively.

Users can create two types of agents:

* Conversational Agent: Intended for scenarios where the end user interacts through a chat interface, or where the agent is called in a conversational manner by another agent.

* Single-Call Agent: Designed for isolated agentic patterns such as background processes, subagents in an Agent-as-Tool setup, or any use case that doesn't require a conversational interface with historical context.

 {{< figure src="/attachments/appstore/platform-supported-content/modules/genai/agentcommons/agentbuilderUI.png" >}}

#### Define Context Entity

If a prompt text of your agent contains variables, your app must have an entity with attributes that match the variable names. An object of this entity functions as the context object, containing the context data and being passed when the **call agent** operation is triggered. For more details, see the [Use the agent in the app logic section below](#app-logic). This object contains the actual values that will be inserted into the prompt text(s) where the variables were defined. This entity needs to be linked to the agent in the Agent Commons UI. If you create a new entity, run the app locally first to ensure it appears in the selection list. The `AgentAdmin` will see warnings on the Agent Version details page if the attributes and variables do not match or if no entity has been selected for the prompt. Make sure that the attribute length of the context object is large enough to accommodate the actual values when logic is executed in the running app.

#### Add Microflows as Tools

To allow your agent to perform actions dynamically and autonomously, or to give it access to specific data based on its autonomously suggested function input, microflows can be added as tools. Invoking the agent will then make use of the function calling pattern to execute the required microflows with the input as specified in the model's response. See [Function Calling](/appstore/modules/genai/function-calling/) for more technical information.

#### Add Knowledge Bases

For supported knowledge bases that are registered in the app, you can connect the agent to allow it to do specified retrievals autonomously. Refer to the documentation of the connector of the knowledge base provider of choice to set up the connection from your app to the knowledge base. To allow the agent to perform semantic searches in the knowledge base, add the knowledge base to the agent definition and specify the parameters that determine how a retrieve should happen (e.g. metadata, desired number of chunks retrieved, threshold similarity).

#### Test and Refine the Agent

While writing the system prompt (for both conversational and single-call types) or the user prompt (only for the single-call type), the prompt engineer can include variables by enclosing them in double braces, for example, `{{variable}}`. The actual values of these placeholders are typically known at runtime based on the user's page context. 
To test the behavior of the prompts, a test can be executed. The prompt engineer must provide test values for all variables defined in the prompts. Additionally, multiple sets of test values for the variables can be defined and run in bulk. Based on the test results, the prompt engineer can add, remove, or rephrase certain parts of the prompt.

### Use the Agent in the App Logic {#app-logic}

After several quick iterations, the first version of the agent is typically ready to be saved and integrated into the application logic to be tested from the end-user perspective. For this, you can add one of the operations from this module to your logic.

#### Create a Version

New agents will be created in the draft status by default, meaning they are still being worked on and can be tested using the agent commons module only. When it is ready to be integrated into the actual app (i.e., the logic that end users trigger), the agent must be saved as a version. This will store a snapshot of the prompt texts and the configured microflows as tools and knowledge bases. To select the active version for the agent, use the three-dot ({{% icon name="three-dots-menu-horizontal" %}}) menu option on the agent overview and click  **Select Version in use**.

#### Call the Agent from a Microflow

For most use cases, the `Call Agent` microflow activity can be used. This operation can be found in the **Toolbox** in Studio Pro while editing a microflow, under the category **Agents Kit**. 

1. Create a Request object using the [GenAI Commons operation](/appstore/modules/genai/genai-for-mx/commons/#chat-create-request) or use [Default Preprocessing from ConversationalUI](/appstore/modules/genai/conversational-ui-module/).  
1. Make sure the Agent object is in scope in your microflow: e.g. retrieve your Agent object from database based on name.
1. Pass both objects to the mentioned `Call Agent` activity. 

This action calls the Agent with the specified request. It executes a Chat Completions (With History) operation based on the defined Agent. All agent configurations, such as the selected model, system prompt, tools, knowledge base or model parameter settings are used. A Response is returned that contains the final assistant's message, in the same fashion as the chat completions operations from GenAI Commons.

For more specific use cases, where a context object needs to be included for variable replacement, use `Get Prompt for Context Object`, which can also be found in the **Toolbox** in Studio Pro while editing a microflow, under the category **Agents Kit**. This operation returns both a system prompt and a user prompt strings, on a combined `PromptToUse` object. These string attributes can be passed to the chat completions operation. Once more, retrieve the Agent (e.g. by name) and pass it with your custom context object to the operation. In a similar way as the Call Agent activity works, you can use microflow `Request_AddAgentCapabilities` to make all the agent's properties have effect on the request that will be executed. Just place the required Chat Completions operation (with or without history) after it to call the agent in this case.

For a conversational agent, the chat context can be created based on the agent in one convenient operation. Use the `New Chat for Agent` operation from the **Toolbox** under the **Agents Kit** category. Retrieve the agent (e.g. by name) and pass it with your custom context object to the operation. Note that this sets the system prompt for the chat context, making it applicable to the entire (future) conversation. Similar to other chat context operations, an [action microflow needs to be selected](/appstore/modules/genai/conversational-ui-module/conversational-ui/#action-microflow) for this microflow action.



{{% alert color="info" %}}
Download the [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369) from the Marketplace for a detailed example of how to use the **Call Agent** activity in an action microflow of a chat interface.
{{% /alert %}}

#### Transport the agent to other environments
With the above microflow logic, the agent version is ready to be tested from the end-user flow (in a local or test environment). The agent can also be exported/imported for transport to other environments if needed. Use the export button on the page where the agent is edited, or use the export and import buttons on the overview page. 
In cases where context objects or functions have changed, make sure the right version of the project is deployed before importing the new agent definition, so that the domain model and microflows match with the new agent version. 

### Improve the Agent {#improve-agent}

When an agent version is saved, there is a button to create a new draft version. This new draft can be used as a starting point to make small changes or improvements based on feedback, either from testing or when the functionality is live for a certain amount of time and the necessity to cover additional scenarios arises.

#### Create Multiple Versions

The new draft version will initially have the same prompt texts, tools and linked knowledge bases as the latest version. The prompt texts can now be modified to cover the additional scenarios, and tools and knowledge bases can be added, removed and edited. When the improved agent is ready, it can be saved as a new version.

#### Manage In-use Version per Environment

Each time a new version of the agent is created, a decision needs to be made regarding which version to use in the end-user logic. Mendix recommends evaluating the in-use version as part of the test and release process. When importing the new agents into other environments, selecting the in-use version is always a manual step and, therefore, a conscious decision.
The user will be prompted to select the version used as part of the import user flow. At later moments, this can always be managed from the agent overview directly. 

 {{< figure src="/attachments/appstore/platform-supported-content/modules/genai/agentcommons/Select_in_use.png" >}}

## Technical Reference

The module includes technical reference documentation for the available entities, enumerations, activities, and other items that you can use in your application. You can view the information about each object in context by using the **Documentation** pane in Studio Pro.

The **Documentation** pane displays the documentation for the currently selected element. To view it, perform the following steps:

1. In the [View menu](/refguide/view-menu/) of Studio Pro, select **Documentation**.
2. Click the element for which you want to view the documentation.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/technical-reference/doc-pane.png" >}}
