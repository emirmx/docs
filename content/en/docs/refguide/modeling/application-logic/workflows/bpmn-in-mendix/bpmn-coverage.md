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
| Event Subprocess | [Event Subprocess](/refguide/workflow-elements/) |
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

> **n/a** — this combination does not exist in the BPMN 2.0 specification. It is not a Mendix limitation.
>
> **Not supported** — this combination exists in the BPMN 2.0 specification but is not currently supported in Mendix.

### None Events {#none-events}

| Variant | How |
|---|---|
| Start | Every workflow has one start event. Start a workflow by providing an object of the entity type that the workflow expects. Use the [Call Workflow](/refguide/on-click-event/#call-workflow) page action (for example, on a button with a data view) or the [Call Workflow](/refguide/workflow-call/) activity in a [microflow](/refguide/microflows/) where you pass the [context object](/refguide/workflow-call/#context-object). |
| Event Subprocess (Interrupting) | n/a |
| Event Subprocess (Non-Interrupting) | n/a |
| Intermediate Catch | n/a |
| Intermediate Boundary (Interrupting) | n/a |
| Intermediate Boundary (Non-Interrupting) | n/a |
| Intermediate Throw | n/a |
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
| Intermediate Throw | Use [Call Microflow](/refguide/call-microflow/) containing a [Notify Workflow](/refguide/notify-workflow/) activity to send the message and continue the flow. |
| End | Use [Call Microflow](/refguide/call-microflow/) before the end event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to send the message before the process completes. |

### Timer Events {#timer-events}

| Variant | How |
|---|---|
| Start | Use a [scheduled event](/refguide/scheduled-events/) to run a [microflow](/refguide/microflows/) that starts the workflow using the [Call Workflow](/refguide/workflow-call/) activity. |
| Event Subprocess (Interrupting) | Planned for Studio Pro 11.12 (requires Timer Event Subprocess Start). |
| Event Subprocess (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Timer Event Subprocess Start). |
| Intermediate Catch | [Timer](/refguide/timer/) |
| Intermediate Boundary (Interrupting) | [Interrupting Timer Event](/refguide/timer/) |
| Intermediate Boundary (Non-Interrupting) | [Non-Interrupting Timer Event](/refguide/timer/) |
| Intermediate Throw | n/a |
| End | n/a |

### Error Events {#error-events}

| Variant | How |
|---|---|
| Start | n/a |
| Event Subprocess (Interrupting) | Use a [Call Microflow](/refguide/call-microflow/) to detect the error condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity to trigger an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications). |
| Event Subprocess (Non-Interrupting) | n/a |
| Intermediate Catch | n/a |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | n/a |
| Intermediate Throw | n/a |
| End | Use a [Call Microflow](/refguide/call-microflow/) to detect the error condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity before the end event to signal the error before the process completes. |

### Signal Events {#signal-events}

| Variant | How |
|---|---|
| Start | Use a [microflow](/refguide/microflows/) to start multiple workflows using multiple [Call Workflow](/refguide/workflow-call/) activities. |
| Event Subprocess (Interrupting) | Use a [microflow](/refguide/microflows/) with the [Notify Workflow](/refguide/notify-workflow/) activity to send notifications to multiple running workflow instances, triggering an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) in each. |
| Event Subprocess (Non-Interrupting) | Use a [microflow](/refguide/microflows/) with the [Notify Workflow](/refguide/notify-workflow/) activity to send notifications to multiple running workflow instances, triggering a [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications) in each. |
| Intermediate Catch | Use a [microflow](/refguide/microflows/) with multiple [Notify Workflow](/refguide/notify-workflow/) activities to deliver notifications to multiple waiting [Wait for Notification](/refguide/wait-for-notification/) activities. |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | Use [Call Microflow](/refguide/call-microflow/) with multiple [Notify Workflow](/refguide/notify-workflow/) activities to send notifications to multiple workflow instances. |
| End | Use [Call Microflow](/refguide/call-microflow/) with multiple [Notify Workflow](/refguide/notify-workflow/) activities to send notifications to multiple workflow instances before the process completes. |

### Conditional Events {#conditional-events}

| Variant | How |
|---|---|
| Start | Use a [microflow](/refguide/microflows/) that checks the condition and starts the workflow using the [Call Workflow](/refguide/workflow-call/) activity only when the condition is met. |
| Event Subprocess (Interrupting) | Use a [Call Microflow](/refguide/call-microflow/) to evaluate the condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity only when the condition is met to trigger an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications). |
| Event Subprocess (Non-Interrupting) | Use a [Call Microflow](/refguide/call-microflow/) to evaluate the condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity only when the condition is met to trigger a [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications). |
| Intermediate Catch | Use [Call Microflow](/refguide/call-microflow/) to evaluate the condition and return a result, then use a [Decision](/refguide/decision-in-workflows/) to route the workflow based on that result. |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | n/a |
| End | n/a |

### Escalation Events {#escalation-events}

| Variant | How |
|---|---|
| Start | n/a |
| Event Subprocess (Interrupting) | Use a [Call Microflow](/refguide/call-microflow/) to detect the escalation condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity to trigger an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications). |
| Event Subprocess (Non-Interrupting) | Use a [Call Microflow](/refguide/call-microflow/) to detect the escalation condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity to trigger a [Non-Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications). |
| Intermediate Catch | n/a |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Throw | Use a [Call Microflow](/refguide/call-microflow/) containing a [Notify Workflow](/refguide/notify-workflow/) activity to raise the escalation signal and continue the flow. |
| End | Use a [Call Microflow](/refguide/call-microflow/) before the end event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to raise the escalation signal before the process completes. |

### Compensation Events {#compensation-events}

| Variant | How |
|---|---|
| Start | n/a |
| Event Subprocess (Interrupting) | Use a [Call Microflow](/refguide/call-microflow/) to detect the compensation condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity to trigger an [Interrupting Notification Event Subprocess Start](/refguide/workflow-event-sub-processes/#triggers-and-notifications). |
| Event Subprocess (Non-Interrupting) | n/a |
| Intermediate Catch | n/a |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | n/a |
| Intermediate Throw | Use a [Call Microflow](/refguide/call-microflow/) containing a [Notify Workflow](/refguide/notify-workflow/) activity to raise the compensation signal and redirect the flow to the compensating activity. |
| End | Use a [Call Microflow](/refguide/call-microflow/) before the end event, containing a [Notify Workflow](/refguide/notify-workflow/) activity to raise the compensation signal before the process completes. |

### Cancel Events {#cancel-events}

| Variant | How |
|---|---|
| Start | n/a |
| Event Subprocess (Interrupting) | n/a |
| Event Subprocess (Non-Interrupting) | n/a |
| Intermediate Catch | n/a |
| Intermediate Boundary (Interrupting) | Planned for Studio Pro 11.12 (requires Notification Boundary Event). |
| Intermediate Boundary (Non-Interrupting) | n/a |
| Intermediate Throw | n/a |
| End | Use a [Call Microflow](/refguide/call-microflow/) to detect the cancellation condition, then send a notification using a [Notify Workflow](/refguide/notify-workflow/) activity before the end event to signal the cancellation before the process completes. |

### Terminate Events {#terminate-events}

| Variant | How |
|---|---|
| Start | n/a |
| Event Subprocess (Interrupting) | n/a |
| Event Subprocess (Non-Interrupting) | n/a |
| Intermediate Catch | n/a |
| Intermediate Boundary (Interrupting) | n/a |
| Intermediate Boundary (Non-Interrupting) | n/a |
| Intermediate Throw | n/a |
| End | [End Event](/refguide/end-event/) — When the workflow reaches an End Event, the entire workflow terminates and any ongoing parallel paths are aborted. |

### Link Events {#link-events}

| Variant | How |
|---|---|
| Start | n/a |
| Event Subprocess (Interrupting) | n/a |
| Event Subprocess (Non-Interrupting) | n/a |
| Intermediate Catch | [Jump Activity](/refguide/jump-activity/) |
| Intermediate Boundary (Interrupting) | n/a |
| Intermediate Boundary (Non-Interrupting) | n/a |
| Intermediate Throw | [Jump Activity](/refguide/jump-activity/) |
| End | n/a |

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
| Intermediate Throw | n/a |
| End | n/a |