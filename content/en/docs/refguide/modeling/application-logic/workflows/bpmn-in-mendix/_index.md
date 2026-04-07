---
title: "BPMN in Mendix"
url: /refguide/bpmn-in-mendix/
weight: 6
---

## What is BPMN?

Business Process Model and Notation (BPMN) is a visual language for mapping out business processes. It uses graphical flowcharts that business users, analysts, developers, and data architects can all read and agree on. Written descriptions get interpreted differently by different people. BPMN diagrams don't have that problem.

The standard is maintained by the [Object Management Group (OMG).](https://www.omg.org/) The current version, [BPMN 2.0.2](https://www.omg.org/spec/BPMN/2.0.2), is published by ISO as an international standard ISO/IEC 19510.

BPMN diagrams are organized into four categories of elements:

- **Tasks:** A unit of work in the process. For example, a user filling out a form, or a service being called automatically.
- **Events:** Something that happens during the process. Events either kick it off, occur in the middle, or mark the end.
- **Gateways:** Control how the flow splits or merges. For example, a gateway takes one path based on a condition or fires multiple paths in parallel.
- **Sequence Flows:** The arrows connecting everything. They define the order in which elements execute.

BPMN gives teams a shared language for describing processes. Analysts model, developers build, and stakeholders review — all from the same diagram. Pure BPMN platforms execute that diagram directly. Mendix takes a different approach.


## How Mendix Supports BPMN

Real processes need user interfaces, validation logic, Application Programming Interface (API) calls, and data. In a pure BPMN platform, you build those things separately and connect them.

In Mendix, they're part of the same model.

- **[Pages](/refguide/pages/)** are your forms.
- **[Microflows](/refguide/microflows/)** run your business rules.
- **[Domain model](/refguide/domain-model/)** holds your data.

When a workflow step needs custom behavior, you attach a [microflow](/refguide/microflows/) or open a [page](/refguide/pages/) for the user to act on. The user completes their task, and the workflow continues from where it left off.


### Same Process. Different Canvas.

Below is a Leave Request process built twice: once as a BPMN diagram, and once in the Mendix Workflow editor.

**Process in BPMN**
{{< figure src="/attachments/refguide/modeling/application-logic/workflows/bpmn-in-mendix/example-process-bpmn.png" alt="Example process BPMN" class="no-border" >}} 

**Process in Mendix**
{{< figure src="/attachments/refguide/modeling/application-logic/workflows/bpmn-in-mendix/example-process-mendix.png" alt="Example process Mendix" class="no-border" >}} 

## BPMN Import

Mendix does not provide native BPMN XML import. However, you can use [Maia](/refguide/maia/) to help translate BPMN diagrams into Mendix workflows.

Upload your BPMN diagram (as an image or PDF) to Maia and ask it to create a workflow in Mendix. Maia analyzes the diagram and generates the corresponding workflow elements, including activities, gateways, and events. You may need to configure properties like user assignments, expressions, and microflow logic after the initial conversion.


{{% alert color="info" %}}
Maia only accepts images and PDFs. If you have a BPMN XML file, convert it to an image or PDF first using [bpmn.io](https://bpmn.io/) or [bpmn-to-image](https://github.com/bpmn-io/bpmn-to-image).
{{% /alert %}}

## BPMN Coverage Overview

### Gateways

- Exclusive Gateway (XOR) — [🟢 Supported](/refguide/bpmn-coverage/#gateways)
- Parallel Gateway (AND) — [🟢 Supported](/refguide/bpmn-coverage/#gateways)
- Inclusive Gateway (OR) — [🟢 Supported](/refguide/bpmn-coverage/#gateways)
- Event-Based Gateway — 🔴 Not Supported
- Complex Gateway — 🔴 Not Supported

### Tasks

- User Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)
- Service Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)
- Script Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)
- Business Rule Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)
- Send Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)
- Receive Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)
- Manual Task — [🟢 Supported](/refguide/bpmn-coverage/#tasks)

### Subprocesses

- Embedded Subprocess — 🔴 Not Supported
- Call Activity – Reusable Subprocess — [🟢 Supported](/refguide/bpmn-coverage/#subprocesses)
- Event Subprocess — [🟢 Supported](/refguide/bpmn-coverage/#subprocesses)
- Transaction Subprocess — 🔴 Not Supported
- Ad-hoc Subprocess — 🔴 Not Supported

### Swimlanes

- Pool — [🟢 Supported](/refguide/bpmn-coverage/#swimlanes)
- Lane — [🟢 Supported](/refguide/bpmn-coverage/#swimlanes)

### Data

- Data Objects / Data Store — [🟢 Supported](/refguide/bpmn-coverage/#data)

### Artifacts

- Text Annotation — [🟢 Supported](/refguide/bpmn-coverage/#artifacts)
- Group — 🔴 Not Supported

### Events

<table border="1" cellspacing="0" cellpadding="6" style="border-collapse: collapse; width: 100%;">
  <thead>
    <!-- Row 1: Group headers -->
    <tr>
      <th style="background-color:#d9d9d9;"></th>
      <th colspan="3" style="background-color:#d9d9d9; text-align:center;">Start</th>
      <th colspan="4" style="background-color:#d9d9d9; text-align:center;">Intermediate</th>
      <th style="background-color:#d9d9d9; text-align:center;">End</th>
    </tr>
    <!-- Row 2: Column sub-headers -->
    <tr>
      <th style="background-color:#d9d9d9;"><strong>Type</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Normal</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Event Subprocess</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Event Subprocess Non-Interrupting</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Catch</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Boundary</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Boundary Non-Interrupting</strong></th>
      <th style="background-color:#d9d9d9;"><strong>Throw</strong></th>
      <th style="background-color:#d9d9d9;"></th>
    </tr>
  </thead>
  <tbody>
    <!-- NONE -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>None</strong></td>
      <td><a href="/refguide/bpmn-coverage/#none-events">🟢 Supported</a></td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#none-events">🔴 Not Supported</a></td>
    </tr>
    <!-- MESSAGE -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Message</strong></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#message-events">🟢 Supported</a></td>
    </tr>
    <!-- TIMER -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Timer</strong></td>
      <td><a href="/refguide/bpmn-coverage/#timer-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#timer-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#timer-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#timer-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#timer-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#timer-events">🟢 Supported</a></td>
      <td>n/a</td>
      <td>n/a</td>
    </tr>
    <!-- ERROR -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Error</strong></td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#error-events">🟢 Supported</a></td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#error-events">🔵 Planned (11.12)</a></td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#error-events">🟢 Supported</a></td>
    </tr>
    <!-- SIGNAL -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Signal</strong></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#signal-events">🟢 Supported</a></td>
    </tr>
    <!-- CONDITIONAL -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Conditional</strong></td>
      <td><a href="/refguide/bpmn-coverage/#conditional-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#conditional-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#conditional-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#conditional-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#conditional-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#conditional-events">🔵 Planned (11.12)</a></td>
      <td>n/a</td>
      <td>n/a</td>
    </tr>
    <!-- ESCALATION -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Escalation</strong></td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#escalation-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#escalation-events">🟢 Supported</a></td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#escalation-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#escalation-events">🔵 Planned (11.12)</a></td>
      <td><a href="/refguide/bpmn-coverage/#escalation-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#escalation-events">🟢 Supported</a></td>
    </tr>
    <!-- COMPENSATION -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Compensation</strong></td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#compensation-events">🟢 Supported</a></td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#compensation-events">🔵 Planned (11.12)</a></td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#compensation-events">🟢 Supported</a></td>
      <td><a href="/refguide/bpmn-coverage/#compensation-events">🟢 Supported</a></td>
    </tr>
    <!-- CANCEL -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Cancel</strong></td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#cancel-events">🔵 Planned (11.12)</a></td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#cancel-events">🟢 Supported</a></td>
    </tr>
    <!-- TERMINATE -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Terminate</strong></td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#terminate-events">🟢 Supported</a></td>
    </tr>
    <!-- LINK -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Link</strong></td>
      <td>n/a</td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#link-events">🟢 Supported</a></td>
      <td>n/a</td>
      <td>n/a</td>
      <td><a href="/refguide/bpmn-coverage/#link-events">🟢 Supported</a></td>
      <td>n/a</td>
    </tr>
    <!-- MULTIPLE -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Multiple</strong></td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
    </tr>
    <!-- MULTIPLE PARALLEL -->
    <tr>
      <td style="background-color:#d9d9d9;"><strong>Multiple Parallel</strong></td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>🔴 Not Supported</td>
      <td>n/a</td>
      <td>n/a</td>
    </tr>
  </tbody>
</table>