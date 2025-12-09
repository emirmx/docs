---
title: "End Event"
url: /refguide/end-event-in-workflows/
weight: 50
---

## Introduction

An **End** event in workflows defines where a workflow process ends. When the workflow execution reaches this element, the process is completed and no further actions are taken.

A workflow can have multiple end events. Starting from Studio Pro 11.6, workflows can also terminate through any one of these end events. This means that activities with multiple outgoing paths, such as user tasks or decisions, can direct each path to its own **End** event, as shown in the example below:

{{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/end-event/multiple-end-events.png" alt="End Event Example" max-width=30% >}}

In the example above, there is a decision element where the workflow can take two different paths based on a condition. Each path leads to a different end event, either of which ends the process. Out of the two paths, only one path will be taken, and the process ends when it reaches the corresponding end event.

{{% alert color="info" %}}
In Studio Pro 11.6 and below, a workflow can also have multiple end events but it is required to terminate it with a single end event at the end of the main flow. 
{{% /alert %}}

## Properties

The **End** event does not have any configurable properties.
