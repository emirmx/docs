---
title: "BPMN Coverage"
url: /refguide/bpmn-coverage/
description: "Describes how Mendix implements BPMN 2.0 constructs through workflows, activities, and microflows."
weight: 7
---

Mendix supports a broad range of BPMN constructs, either directly on the Workflow canvas or through the wider Mendix platform. 

## Gateways {#gateways}

| Element | How |
|---|---|
| Exclusive Gateway (XOR) | [Decision](/refguide/decision-in-workflows/) |
| Parallel Gateway (AND) | [Parallel Split](/refguide/parallel-split/) |
| Inclusive Gateway (OR) | Use a [Parallel Split](/refguide/parallel-split/) with a path for each possible condition. On each path, add a [Decision](/refguide/decision-in-workflows/) that checks if the condition is true. If true, execute the activities on that path. If false, the path continues directly to the merge. The workflow waits for all paths to complete before continuing. Note: There is no default or catch-all path — if you need one, model it explicitly with an additional decision. |
| Event-Based Gateway | Not supported. |
| Complex Gateway | Not supported. |

## Tasks {#tasks}

| Element | How |
|---|---|
| User Task | [User Task](/refguide/user-task/) |
| User Task (Multi-instance Parallel) | [Multi-User Task](/refguide/multi-user-task/) |
| Service Task | [Call Microflow](/refguide/call-microflow/) |
| Script Task | [Call Microflow](/refguide/call-microflow/) |
| Business Rule Task | Mendix does not support decision tables. Use [Call Microflow](/refguide/call-microflow/) to build your own decision logic in a [microflow](/refguide/microflows/). The Call Microflow element has built-in branching based on the [microflow return type](/refguide/call-microflow/#outcomes). |
| Send Task | Use [Call Microflow](/refguide/call-microflow/) with a [Notify Workflow](/refguide/notify-workflow/) activity inside to send the message. |
| Receive Task | [Wait for Notification](/refguide/wait-for-notification/) |
| Manual Task | Use [Call Microflow](/refguide/call-microflow/) with no logic inside. It acts as a pass-through and continues automatically when the workflow instance arrives. |

## Subprocesses {#subprocesses}

| Element | How |
|---|---|
| Embedded Subprocess | Not supported. |
| Call Activity (Reusable Subprocess) | [Call Workflow](/refguide/call-workflow/) |
| Event Subprocess | [Event Subprocess](/refguide/event-sub-processes/) |
| Transaction Subprocess | Not supported. |
| Ad-hoc Subprocess | Not supported. |

## Swimlanes {#swimlanes}

| Element | How |
|---|---|
| Pool | The workflow itself acts as the process boundary — one workflow equals one pool. |
| Lane | Use [User Task](/refguide/user-task/) assignments with Roles and Workflow Groups to define who is responsible for each step. |

## Data {#data}

| Element | How |
|---|---|
| Data Object, Data Input, Data Output, Data Store | Mendix does not have visual equivalents for these concepts on the Workflow canvas. You manage all data through entities defined in the [domain model](/refguide/domain-model/). Pass one entity into the workflow via the [workflow parameter](/refguide/workflow-parameters/) to create `$WorkflowContext`, which gives you access to business data throughout the entire workflow. `$WorkflowInstance` is always available alongside it for workflow engine data (`System.Workflow`). Because the context entity is always persistable, there is no distinction between temporary and persistent data. You do not need separate Data Object, Data Input, Data Output, or Data Store constructs. |

## Artifacts {#artifacts}

| Element | How |
|---|---|
| Text Annotation | Use the **Annotation** element on the Workflow canvas to add descriptive notes. |
| Group | Not supported. |

## Events {#events}

Mendix supports a broad range of BPMN event types. Some are available directly as elements on the Workflow canvas. Others are achieved through [microflows](/refguide/microflows/) that contain workflow-related activities.

> **N/A** — this combination does not exist in the BPMN 2.0 specification. It is not a Mendix limitation.
>
> **Not supported** — this combination exists in the BPMN 2.0 specification but is not currently supported in Mendix.

### None Events {#none-events}

| Variant | How |
|---|---|
| Start | Every workflow has one start event. Start a workflow by providing an object of the entity type that the workflow expects. Use the [Call Workflow](/refguide/on-click-event/#call-workflow) page action (for example, on a button with a data view) or the [Call Workflow](/refguide/workflow-call/) activity in a [microflow](/refguide/microflows/) where you pass the [context object](/refguide/workflow-call/#context-object). |
| Event Subprocess (Interrupting) | N/A |
| Event Subprocess (Non-Interrupting) | N/A |
| Intermediate Catch | N/A |
| Intermediate Boundary (Interrupting) | N/A |
| Intermediate Boundary (Non-Interrupting) | N/A |
| Intermediate Throw | N/A |
| End | Not supported. Mendix's [End Event](/refguide/end-event/) terminates the entire workflow and aborts any ongoing parallel paths, which is equivalent to a Terminate End Event, not a normal End Event. |

### Message Events {#message-events}

| Variant | How |
|---|---|
| Start | Same as None Start — process the message data in a [microflow](/refguide/microflows/), create or populate an object of the entity type that the workflow expects, and start the workflow using the [Call Workflow](/refguide/workflow-call/) activity by passing that [context object](/refguide/workflow-call/#context-object). Alternatively, use the [Call Workflow](/refguide/on-click-event/#call-workflow) page action on a page. |
| Event Subprocess (Interrupting) | [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) |
| Event Subprocess (Non-Interrupting) | [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) |
| Intermediate Catch | Planned for Studio Pro 11.12 (Notification Event). Use [Wait for Notification](/refguide/wait-for-notification/) activity as alternative. |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to send the message and continue the flow. |
| End | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to send the message before the process completes. |

### Timer Events {#timer-events}

| Variant | How |
|---|---|
| Start | Use a [scheduled event](/refguide/scheduled-events/) to run a [microflow](/refguide/microflows/) that starts the workflow using the [Call Workflow](/refguide/workflow-call/) activity. |
| Event Subprocess (Interrupting) | Planned for Studio Pro 11.12 (requires Timer Event Subprocess Start). |
| Event Subprocess (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Timer Event Subprocess Start). |
| Intermediate Catch | [Timer](/refguide/timer/) |
| Intermediate Boundary (Interrupting) | [Interrupting Timer Event](/refguide/timer/) |
| Intermediate Boundary (Non-Interrupting) | [Non-Interrupting Timer Event](/refguide/timer/) |
| Intermediate Throw | N/A |
| End | N/A |

### Error Events {#error-events}

| Variant | How |
|---|---|
| Start | N/A |
| Event Subprocess (Interrupting) | Use an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to handle the error logic and send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity at the moment the error occurs. |
| Event Subprocess (Non-Interrupting) | N/A |
| Intermediate Catch | N/A |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | N/A |
| Intermediate Throw | N/A |
| End | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to throw the error before the process completes. |

### Signal Events {#signal-events}

| Variant | How |
|---|---|
| Start | Use a [microflow](/refguide/microflows/) to start multiple workflows using multiple [Call Workflow](/refguide/workflow-call/) activities. |
| Event Subprocess (Interrupting) | Use an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to handle the signal condition and send notifications using a [Notify Workflow](/refguide/notify-workflow/) activity to multiple running workflow instances. |
| Event Subprocess (Non-Interrupting) | Use a [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to handle the signal condition and send notifications using a [Notify Workflow](/refguide/notify-workflow/) activity to multiple running workflow instances. |
| Intermediate Catch | Use a [Wait for Notification](/refguide/wait-for-notification/) activity on each workflow instance, and use a microflow with multiple [Notify Workflow](/refguide/notify-workflow/) activities to deliver the signal to all waiting instances. |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing multiple [Notify Workflow](/refguide/notify-workflow/) activities to send the signal to multiple workflow instances. |
| End | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing multiple [Notify Workflow](/refguide/notify-workflow/) activities to send the signal to multiple workflow instances before the process completes. |

### Conditional Events {#conditional-events}

| Variant | How |
|---|---|
| Start | Use a [microflow](/refguide/microflows/) that checks the condition and starts the workflow using the [Call Workflow](/refguide/workflow-call/) activity only when the condition is met. |
| Event Subprocess (Interrupting) | Use an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to evaluate or create the condition and send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity at the moment the condition is met. |
| Event Subprocess (Non-Interrupting) | Use a [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to evaluate or create the condition and send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity at the moment the condition is met. |
| Intermediate Catch | Use [Call Microflow](/refguide/call-microflow/) to evaluate the condition and return a result, then use a [Decision](/refguide/decision-in-workflows/) to route the workflow based on that result. |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | N/A |
| End | N/A |

### Escalation Events {#escalation-events}

| Variant | How |
|---|---|
| Start | N/A |
| Event Subprocess (Interrupting) | Use an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to evaluate the escalation condition and send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity at the moment the escalation needs to happen. |
| Event Subprocess (Non-Interrupting) | Use a [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to evaluate the escalation condition and send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity at the moment the escalation needs to happen. |
| Intermediate Catch | N/A |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to throw the escalation and continue the flow. |
| End | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to throw the escalation before the process completes. |

### Compensation Events {#compensation-events}

| Variant | How |
|---|---|
| Start | N/A |
| Event Subprocess (Interrupting) | Use an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) to the workflow, and use a microflow to evaluate the compensation condition and send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity at the moment the compensation needs to happen. |
| Event Subprocess (Non-Interrupting) | N/A |
| Intermediate Catch | N/A |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | N/A |
| Intermediate Throw | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to throw the compensation and redirect the flow to the compensating activity. |
| End | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to throw the compensation before the process completes. |

### Cancel Events {#cancel-events}

| Variant | How |
|---|---|
| Start | N/A |
| Event Subprocess (Interrupting) | N/A |
| Event Subprocess (Non-Interrupting) | N/A |
| Intermediate Catch | N/A |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | N/A |
| Intermediate Throw | N/A |
| End | Use a [Call Microflow](/refguide/call-microflow/) acting as the throw event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to throw the cancellation before the process completes. |

### Terminate Events {#terminate-events}

| Variant | How |
|---|---|
| Start | N/A |
| Event Subprocess (Interrupting) | N/A |
| Event Subprocess (Non-Interrupting) | N/A |
| Intermediate Catch | N/A |
| Intermediate Boundary (Interrupting) | N/A |
| Intermediate Boundary (Non-Interrupting) | N/A |
| Intermediate Throw | N/A |
| End | [End Event](/refguide/end-event/) — When the workflow reaches an End Event, the entire workflow terminates and any ongoing parallel paths are aborted. |

### Link Events {#link-events}

| Variant | How |
|---|---|
| Start | N/A |
| Event Subprocess (Interrupting) | N/A |
| Event Subprocess (Non-Interrupting) | N/A |
| Intermediate Catch | [Jump Activity](/refguide/jump-activity/) |
| Intermediate Boundary (Interrupting) | N/A |
| Intermediate Boundary (Non-Interrupting) | N/A |
| Intermediate Throw | [Jump Activity](/refguide/jump-activity/) |
| End | N/A |

### Multiple Events {#multiple-events}

| Variant | How |
|---|---|
| Start | Not supported. |
| Event Subprocess (Interrupting) | Not supported. |
| Event Subprocess (Non-Interrupting) | Not supported. |
| Intermediate Catch | Not supported. |
| Intermediate Boundary (Interrupting) | Not supported. |
| Intermediate Boundary (Non-Interrupting) | Not supported. |
| Intermediate Throw | Not supported. |
| End | Not supported. |

### Multiple Parallel Events {#multiple-parallel-events}

| Variant | How |
|---|---|
| Start | Not supported. |
| Event Subprocess (Interrupting) | Not supported. |
| Event Subprocess (Non-Interrupting) | Not supported. |
| Intermediate Catch | Not supported. |
| Intermediate Boundary (Interrupting) | Not supported. |
| Intermediate Boundary (Non-Interrupting) | Not supported. |
| Intermediate Throw | N/A |
| End | N/A |