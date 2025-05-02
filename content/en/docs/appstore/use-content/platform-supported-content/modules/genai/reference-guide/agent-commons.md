---
title: "Agent Commons"
url: /appstore/modules/genai/genai-for-mx/agent-commons/
linktitle: "Agent Commons"
description: "Describes the purpose, configuration and usage of the Agents Commons module from the Mendix Marketplace that allows developers to build, define and refine Agents, to integrate GenAI principles and Agentic patterns into their Mendix app."
weight: 20

---

## Introduction

The Agent Commons functionalities allows users to develop, test, and optimize their GenAI use cases by creating effective agents that interact with large language models (LLM). 
Using the [Agent Commons](https://marketplace.mendix.com/link/component/239450) module you can use the Agent Builder interface in your app to define agents at runtime and manage multiple versions over time. Wire up prompts, microflows as tools, knowledge bases together with large language models to build agentic patterns to support your business logic. Agent Builder also supports defining variables that serve as placeholders for data from the app session context which are replaced by actual values when the end user interacts with the app. The Agent Commons module contains the necessary data model, pages, and snippets to include an agent builder interface to your app and get started using Agents from your app logic.

### Typical Use Cases

Typical use cases for Agent Commons include the following:

* The app includes one or more agentic patterns that include interactions with an LLM. These patterns may include additionally microflows as tools, knowledge bases and guardrails.
* The prompts for agents to do the LLM interaction need to be updated or improved, often without changing the code of the LLM interaction or the traditional low-code app logic that calls the agents. This enables people outside the development team (for example, data scientists) to change prompts and iterate on agent configurations .
* The use case benefits from rapid iterations on prompts, microflows as tools, knowledge bases, models, and variable placeholders in a playground set-up, separately from app logic.

### Features

The Agent Commons functionality provides the following:

* Agent Builder UI components and a data structure to manage, store, and rapidly iterate on agent  versions at runtime—without requiring app deployment to change the agent.
* Drag-and-drop operations for calling both single-call and conversational agents from microflows and workflows.
* Support for placeholders in prompts of the agents. The values will be populated in the running app based on a user/context object.
* Logic to define and execute tests individually or in bulk, with result comparison.
* Export/import functionality for transporting agents across different app environments (local, acceptance, and production).
* The ability to manage the active agent version used by the logic in the running app, in the app environment itself without the need to re-deploy.

### Limitations 

The current scope of the module is focused on LLM invocations with a variety of prompts, optionally with placeholders (variables). The agents can be enriched by adding microflows with a single parameter as tools in a [Function Calling](/appstore/modules/genai/function-calling/) setup, and providing access to knowledge bases provided by [Mendix Cloud GenAI Resources](/appstore/modules/genai/mx-cloud-genai/resource-packs/#knowledge-bases).

### Dependencies {#dependencies}

The Agent Commons module requires Mendix Studio Pro version [9.24.2](/releasenotes/studio-pro/9.24/#9242) or above.

You must also download the [Community Commons](/appstore/modules/community-commons-function-library/) module, the [GenAI Commons](https://marketplace.mendix.com/link/component/239448) module, the [Mendix Cloud GenAI Connector](https://marketplace.mendix.com/link/component/239449) and the [Conversational UI](https://marketplace.mendix.com/link/component/239450).

## Installation

If you start from a blank app, or have an existing project where you want to include the agent building functionalities from Agent Commons, you must install the module manually from the Mendix Marketplace. First, make sure your project contains the latest versions of the following dependencies:

- [Community Commons](/appstore/modules/community-commons-function-library/) 
- [GenAI Commons](https://marketplace.mendix.com/link/component/239448)
- [Conversational UI](https://marketplace.mendix.com/link/component/239450)
- [Mendix Cloud GenAI Connector](https://marketplace.mendix.com/link/component/239449)

 Then follow the instructions in [How to Use Marketplace Content](/appstore/use-content/) to install the [Agent Commons](https://marketplace.mendix.com/link/component/240371) module.

## Configuration

To use the Agent Commons functionalities in your app, you must perform the following tasks in Studio Pro:

1. Add the relevant [module roles](#module-roles) to the applicable user roles in the project security.
1. Add the [Agent Builder UI to your app](#ui-components) by using the pages and snippets as a basis.
1. Make sure to have a [deployed model](#deployed-models) configured.
1. [Define](#define-prompt), the prompts, add functions, knowledge bases and test the agent.
1. Add the agent to the [logic](#app-logic) of the actual use case.
1. Improve and [iterate on agent versions](#improve-agent).

### Configuring the Roles {#module-roles}
In the project security of your app, add the module role **AgentCommons.AgentAdmin** to the user roles that are intended to define and refine Agents. They also decide which version is used in the running app environment.

### Adding the Agent Builder UI to Your App {#ui-components} 

The module includes a set of reusable pages, layouts, and snippets, allowing you to add the conversational UI to your app. 

#### Pages and Layouts {#pages-and-layouts}

Add the **Agent_Overview** page (USE_ME > Agent Builder) to your Navigation or add the **Snippet_Agent_Overview** to a page that is already part of your Navigation. Now the Agents can be defined at runtime.
If you need to change the layout or apply other customizations, Mendix recommends copying the page to your own module and modifying it to match your app styling or use case. The **Snippet_Agent_Overview** snippet includes the content of the same page.
From this overview, the user can reach the **Version_Details** page to edit the prompt and execute tests. If customization is needed, its contents can be found in **Snippet_Agent_Details**.

For example, download and run the [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369) to see the pages in action.

### Configure Deployed Models {#deployed-models}

You need at least one GenAI connector that follows the principles of GenAI commons to interact with LLMs from the Aggent Commons logic. To test the behavior of an agent, you must configure at least one Deployed Model for your chosen connector. Refer to the specific connector’s documentation for detailed setup instructions on configuring the Deployed Model.

* For [Mendix Cloud GenAI](https://marketplace.mendix.com/link/component/239449) importing the **Key** from the Mendix portal automatically creates a MxCloud Deployed Model. This is part of the [configuration](/appstore/modules/genai/mx-cloud-genai/MxGenAI-connector/#configuration).
* For [Amazon Bedrock](https://marketplace.mendix.com/link/component/215042), the creation of Bedrock Deployed Models is part of the [model synchronization mechanism](/appstore/modules/aws/amazon-bedrock/#sync-models).
* For [OpenAI](https://marketplace.mendix.com/link/component/220472), the configuration of OpenAI Deployed Models is part of the [configuration](/appstore/modules/genai/reference-guide/external-connectors/openai/#general-configuration).

### Define the Agent {#define-prompt}

When the app is running, a user with the `AgentAdmin` role can set up an agent, write the prompts, link microflows as tools and give it access to knowledge bases. When the agent is associated to a deployed model, it can be tested in an isolated set-up separate from the rest of the app's logic, to properly validate the behavior.
The user can create either a Conversational agent, intended for scenarios where the end-user interacts through a chat interface or the agent is called in a conversational style by another agent, as opposed to a Single-Call agent, designed for isolated agentic patterns, like agents that work in background processes, subagents that are part of an Agent-as-Tool pattern, or otherwise agentic patterns do not require a conversational interface with historical messages. 

#### Define Context Object

If a prompt text of your agent contains variables, your app must have an entity with attributes that match the variable names. An object of this entity functions as the context object, containing the context data and being passed when the **call agent** operation is triggered. For more details, see the [Use the agent in the app logic section below](#app-logic). This object contains the actual values that will be inserted into the prompt text(s) where the variables were defined. This entity needs to be linked to the agent in the Agent Commons UI. If you create a new entity, run the app locally first to ensure it appears in the selection list. The `AgentAdmin` will see warnings on the Agent Version details page if the attributes and variables do not match or if no entity has been selected for the prompt. Make sure that the attribute length of the context object is large enough to accommodate the actual values when logic is executed in the running app.

#### Add Microflows as Tools

To allow your agent to perform actions dynamically and autonomously, or to give it access to specific data based on its autonomously suggested function input, microflows can be added as tools. Invoking the agent will then make use of the function calling pattern to execute the required microflows with the input as specified in the model's response. See [Function Calling](/appstore/modules/genai/function-calling/) for more technical information.

#### Add Knowledge Bases

For supported knowledge bases that are registered in the app, you can connect the agent to allow it to do specified retrievals autonomously. Refer to the documentation of the connector of the knowledge base provider of choice to set up the connetion from your app to the knowledge base. To allow the agent to perform semantic searches in the knowledge base, add the knowledge base to the agent definition and specify the parameters that determine how a retrieve should happen (e.g. metadata, desired number of chunks retrieved, threshold similarity).

#### Test and Refine the Agent

While writing the system prompt (for both conversational and single-call types) or the user prompt (only for the single-call type), the prompt engineer can include variables by enclosing them in double braces, for example, `{{variable}}`. The actual values of these placeholders are typically known at runtime based on the user's page context. 
To test the behavior of the prompts, a test can be executed. The prompt engineer must provide test values for all variables defined in the prompts. Additionally, multiple sets of test values for the variables can be defined and run in bulk. Based on the test results, the prompt engineer can add, remove, or rephrase certain parts of the prompt.

### Use the Agent in the App Logic {#app-logic}

After several quick iterations, the first version of the agent is typically ready to be saved and integrated into the application logic to be tested from the end-user perspective. For this, you can add one of the operations from this module to your logic.

#### Create a Version

New agents will be created in the draft status by default, meaning they are still being worked on and can be tested using the agent commons module only. When it is ready to be integrated into the actual app (i.e., the logic that end users trigger), the agent must be saved as a version. This will store a snapshot of the prompt texts and the configured microflows as tools and knowledge bases. To select the active version for the agent, use the three-dot ({{% icon name="three-dots-menu-horizontal" %}}) menu option on the agent overview and click  **Select Version in use**.

#### Call the Agent from a Microflow

For most use cases, the `Call Agent` microflow activity can be used. This operation which can be found in the **Toolbox** in Studio Pro while editing a microflow, under the category **Agents Kit**. 

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


## Technical Reference {#technical-reference}

The technical purpose of the Agent Commons module is to define a common domain model for defining, testing, storing and refining Agents in Mendix applications.
For documentation on entities and microflow activities, please see the documentation fields in Studio Pro for the respective microflows and entities.

### Domain Model {#domain-model} 

The domain model in Mendix is a data model that describes the information in your application domain in an abstract way. For more general information, see the [Data in the Domain Model](/refguide/domain-model/) documentation. To learn about where the entities from the domain model are used and relevant during implementation, please refer to the documentation fields in Studio Pro for microflows and entities.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genaicommons/demain-model.png" alt="" >}}
    
