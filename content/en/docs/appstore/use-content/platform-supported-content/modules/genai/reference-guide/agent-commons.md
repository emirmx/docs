---
title: "Agent Commons"
url: /appstore/modules/genai/genai-for-mx/agent-commons/
linktitle: "Agent Commons"
description: "Describes the purpose, configuration and usage of the Agents Commons module from the Mendix Marketplace that allows developers to build, define and refine Agents, to integrate GenAI principles and Agentic patterns into their Mendix app."
weight: 20

---

## Introduction

The Agent Commons functionalities allows users to develop, test, and optimize their GenAI use cases by creating effective agents that interact with large language models (LLM). 
Using the [Agent Commons](https://marketplace.mendix.com/link/component/239450) module you can use the Agent Management interface in your app to define agents at runtime and manage multiple versions over time.  It also supports defining variables that serve as placeholders for data from the app session context which are replaced by actual values when the end user interacts with the app. The module contains the necessary data model, pages, and snippets to include a prompt management interface to your app and get started.

### Typical Use Cases

Typical use cases for prompt management include the following:

* The app includes one or more agentic patterns that include interactions with an LLM. 
* The prompts for agents to do the LLM interaction need to be updated or improved without changing the code of the LLM interaction. This enables people outside the development team to change prompts (for example, data scientists).
* The use case benefits from rapid iterations on prompts, microflows as tools, knowledge bases, models, and variable placeholders in a playground set-up, separately from app logic.

### Features

The Agent Commons functionality provides the following:

* UI components and a data structure to manage, store, and rapidly iterate on agent  versions at runtime—without requiring app deployment to change the agent.
* Support for both single-call and conversational agents.
* Includes placeholders in prompts of the agents. The values will be populated in the running app based on a user/context object.
* Logic to define and execute tests individually or in bulk, with result comparison.
* Export/import functionality for transporting agents across different app environments (local, acceptance, and production).
* The ability to manage the active agent version used by the running app's logic.

### Limitations 

The current scope of the module is focused on prompts with placeholders (variables), adding microflows with a single parameter as tools in a [Function Calling](/appstore/modules/genai/function-calling/) setup, and providing your agents access to knowledge bases provided by [Mendix Cloud GenAI Resources](/appstore/modules/genai/mx-cloud-genai/resource-packs/#knowledge-bases).

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

### General Project Setup
1. In the project security of your app, add the module role **AgentCommons.AgentAdmin** to the user roles that are intended to define and refine Agents.
1. Add the **Agent_Overview** page (USE_ME > Agent Builder) to your Navigation or add the **Snippet_Agent_Overview** to a page that is already part of your Navigation. Now the Agents can be defined at runtime.

### Define Agents
1. Run the app (locally or in the cloud).
1. Create and edit agents at runtime using the Agent overview. For an agent, write the prompts, use variable placeholders, add microflows as tools and connect knowledge bases to define the tasks and cabapilities of the agents.
1. Configure the deployed model (LLM) that the agent should use, and connect the agent to it.
1. Create various versions of the agents and test in an isolated setup to rapidly iterate, compare and evaluate the agentic behavior
1. From the Agent overview, set a version as *in use* to allow it to be called from the actual logic in the app.

### Call an Agent form the app logic

1. In the microflow where the Agent needs to be called, make sure the Agent object is available, e.g. retrieve from database by name.
1. Create a Request object using the [GenAI Commons operation](/appstore/modules/genai/genai-for-mx/commons/#chat-create-request) or use [Default Preprocessing from ConversationalUI](/appstore/modules/genai/conversational-ui-module/conversational-ui/chatcontext-operations) in case of a conversational interface.
1. Use toolbox operations **Call Agent** from this module to use an agent in the microflow and make it part of your app logic. 
1. Optionally see **Create Chat for Agent** to initiate a Chat Context for a Conversational Interface.

{{% alert color="info" %}}
Download the [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369) from the Marketplace for a detailed example of how to use the **Call Agent** activity in an action microflow of a chat interface.
{{% /alert %}}


## Technical Reference {#technical-reference}

The technical purpose of the GenAI Commons module is to define a common domain model for generative AI use cases in Mendix applications. To help you work with the **GenAI Commons** module, the following sections list the available [entities](#domain-model), [enumerations](#enumerations), and [microflows](#microflows) to use in your application. 

### Domain Model {#domain-model} 

The domain model in Mendix is a data model that describes the information in your application domain in an abstract way. For more general information, see the [Data in the Domain Model](/refguide/domain-model/) documentation. To learn about where the entities from the domain model are used and relevant during implementation, see the [Microflows](#microflows) section below.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genaicommons/demain-model.png" alt="" >}}
    
