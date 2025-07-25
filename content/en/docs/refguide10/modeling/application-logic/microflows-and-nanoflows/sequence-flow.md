---
title: "Sequence Flow"
url: /refguide10/sequence-flow/
weight: 30
---

## Introduction

A sequence flow is a flow that is shown an arrow that links elements (events, activities, decisions, etc.) with each other. Hereby it defines the order of execution. Flows always flow in one direction where elements follow each other up one by one. Decisions always lead to one direction, so it is not possible that multiple flows take place simultaneously.

When you have two activities that you want to link together a sequence flow is used.

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/sequence-flow/sequence-flow.png" class="no-border" >}}

## Condition Value

The **Condition value** describes which direction should be followed based on the outcome of a [decision](/refguide10/decision/) or an [object type decision](/refguide10/object-type-decision/).
