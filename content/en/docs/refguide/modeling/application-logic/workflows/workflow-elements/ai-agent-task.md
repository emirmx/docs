---
title: "AI Agent Task"
url: /refguide/ai-agent-task/
weight: 85
---

## Introduction

**AI agent task** lets you assign a task to an AI Agent as a step in your workflow, instead of assigning that step to a human. 

When the workflow reaches the AI agent task, the agent performs a predefined task autonomously. The workflow then continues based on that result, for example, by routing to the next step, triggering an action, or escalating to a human reviewer.

**AI agent task** can be used across a wide range of use cases. Some examples include assessing requests, classifying data, extracting information from unstructured text, summarizing content, and drafting outputs such as notifications or reports.

## Prerequisites

Before using the AI Agents Task, you need a configured agent ready to call. See How to Configure Studio Pro Agents for a step-by-step guide.

{{% alert color="info" %}}
The Agent Editor needs to be set up in Studio Pro first. See How to Setup Studio Pro Agents to get started.
{{% /alert %}}

## Properties

AI agent task properties consist of the following sections:

* [General](#general)
* [Parameters](#Parameters)
* [Outcomes](#outcomes)
* [Boundary events](#boundary-events)
* [Common](#common)

### General Section {#general}

#### Caption

The **Caption** describes what happens in this element. It is displayed under the workflow element to make the workflow easier to read and understand.

#### Microflow

The microflow that handles execution for this AI agent task. Use it to prepare and pass the right information to the agent, call the agent, and define what happens next.

### Parameters {#parameters}

Parameters of the selected microflow. Depending on the selected microflow, you will see a list of its parameters. Parameters pass data to the element. To view Parameters, click the ellipsis icon next to the property name.

### Outcomes {#outcomes}

The outcomes depend on the return type of the selected microflow:

* **No return type**: A single outcome. The workflow moves to the next step.
* **Boolean**: Two outcomes: true and false. The workflow moves to the next step based on the returned value.
* **Enumeration**: One outcome for each enumeration value, plus an empty outcome for when the value is unassigned. The workflow moves to the next step based on the returned value.

### Boundary Events Section {#boundary-events}

For more information, see [Boundary Events](/refguide/workflow-boundary-events/).

### Common Section {#common}

**Name** is the internal name of the element. When referring to the element in the app, you will use this name. It must be unique within the workflow, but you can have two elements with the same name in different workflows.

## Read More

* [Workflow Properties](/refguide/workflow-properties/)
* [How to Setup Studio Pro Agents](AddLink)
* [How to Configure Studio Pro Agents](AddLink)
* [GenAI Capabilities in Mendix](/appstore/modules/genai/)
