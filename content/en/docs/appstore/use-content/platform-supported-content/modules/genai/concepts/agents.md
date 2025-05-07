---
title: "Agents"
url: /appstore/modules/genai/agents/
linktitle: "Agents"
weight: 40
description: "Describes Agents and Agentic Patterns as used with generative AI in Mendix"
---

## Introduction

AI agents are autonomous computational systems that execute actions based on triggers such as user input or system events. These agents utilize tools, functions, and knowledge bases to determine appropriate responses. They can be either adaptive (learning-based) or task-specific, designed to automate processes and enhance operational efficiency.

If you are new to the topic and would like to build your own agent, check out our guide on [how to build your first agent in Mendix](https://docs.mendix.com/appstore/modules/genai/how-to/howto-single-agent/). It covers how to combine prompt engineering, function calling, and knowledge bases—all within a Mendix app.

## Multi-Agent systems

Sometimes, one agent is not enough for more complex use cases. Then a multi-agent solution is needed. Multi-agent architectures supersede single-agent implementations when task complexity exceeds individual agent capabilities. While single-agent systems demonstrate efficacy in well-defined, discrete operations, complex or ambiguous scenarios necessitate distributed agent collaboration. Multi-agent frameworks enable specialized task allocation and coordinate execution protocols, resulting in enhanced operational outcomes and improved system performance.

## Pattern Overview

When building agents, it is necessary to choose a pattern so that the task allocation and coordination can be executed as desired. Some examples for patterns can be found below. For practical examples on the following patterns, check the GenAI Showcase App. 

### Prompt Chaining

This is a linear chain of multiple LLM calls. The output of one LLM call is the basis for the input of the next LLM call. It can be passed directly as-is, or as a part of the user prompt with some additional instructions. Each LLM call will have its own system prompt and forms a discrete step in the bigger process that has an overarching goal. It is not necessary that the model used for each call is the same: the choice of model can be optimized for the task of each LLM step.

The input of this system is a user prompt, either typed directly by the user, or constructed using prompt engineering techniques. The output of the system is typically the plain output of the last LLM call.

 {{< figure src="/attachments/appstore/platform-supported-content/modules/genai/agents/Linear-Chaining.svg" >}}

### Prompt Chaining with Gatekeeper

This is an extension of the linear chain of multiple LLM calls. Now, the gatekeeper LLM call is part of the linear flow as well. The difference with the other elements in the flow is that the output of the gatekeeper is not always blindly passed to the next. The task is to break out of the flow in certain (typically unhappy) scenarios, which is determined based on the input it receives. If the flow, however, can be continued according to the gatekeeper, the input of the next LLM call will be the same as the input the gatekeeper received. 

Like in the previous pattern, the input of this system is a user prompt, either typed directly by the user, or constructed using prompt engineering techniques. The output of the system is typically the plain output of the last LLM call in the happy flow. In an unhappy scenario, developers can choose to use the response of the Gatekeeper Agent or to return a static response as output.

 {{< figure src="/attachments/appstore/platform-supported-content/modules/genai/agents/Linear-Chaining-Gatekeeper.svg" >}}

### Evaluator-Optimizer

In the evaluator-optimizer workflow, one LLM interaction generates a text, then another evaluates the text and provides feedback. This happens in a loop until the evaluation criteria are met, or a certain maximum number of attempts has been reached. 

Alternative names for this pattern include:
- LLM as a judge (this term is also used for a test/evaluation framework, not to be confused)
- Generator-Evaluator

The input of this system is a user prompt, either typed directly by the user, or constructed using prompt engineering techniques. The output of the system is the plain output of the last iteration of the Generator Agent LLM call as approved by the Evaluator Agent.

 {{< figure src="/attachments/appstore/platform-supported-content/modules/genai/agents/Evaluator-Optimizer.svg" >}}

### Routing

This pattern is powerful if the desired system must handle a variety of concrete tasks. For each of the different tasks the system should handle, a dedicated agent is created with focus on its specific task. When the system is triggered, the router agent will determine (i.e. classify) which of the supported tasks is semantically most similar to the intent of the input that was given. When a match is found, the input that was given to the system (which can include chat history) will be passed to the corresponding agent. This is often referred to as "hand-off", which means that the chosen agent is now fully responsible for processing the input and generating output, most often without knowing the router agent was there in the first place.

The input of this system is a user prompt, either typed directly by the user, or constructed using prompt engineering techniques. The output of the system is the plain output of the Agent chosen by the Router Agent. Variations exist where the Router Agent has the option to not hand off the input (or conversation) to any of the given agents if it considers the input to be out of the scope for the system. The output of the system can then be the Router Agents response or a static message for the end-user explaining why the system could not successfully process the request.

 {{< figure src="/attachments/appstore/platform-supported-content/modules/genai/agents/Routing.svg" >}}

## Learn More

### Agent Builder

Start from the [Agent Builder Starter App](https://marketplace.mendix.com/link/component/240369) from the Marketplace or add the [Agent Commons module](https://marketplace.mendix.com/link/component/240371) to your existing app and get started with agents and agentic patterns in Mendix.

Read more about [Agent Commons](/appstore/modules/genai/genai-for-mx/agent-commons/) in the GenAI reference guide.

### Showcases

Check out the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) in the Marketplace to see the patterns that were mentioned above in action.

### Additional Information

 Read our [Blogpost about multi-agent systems in a Mendix app](https://www.mendix.com/blog/how-multi-agent-ai-systems-in-mendix-can-train-you-for-a-marathon/)