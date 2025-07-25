---
title: "Timer"
url: /refguide/timer/
weight: 90
aliases:
    - /refguide/wait-for-timer/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

**Timer** allows you to configure a certain duration or a specific date and time in a workflow.

It can be used in the following two ways:

* **Timer** can be used as a standalone activity on a workflow path. It suspends the workflow path for a configurable duration or until a set date and time. For example, when a new salary legislation is about to take effect, a timer can be set until the date of effect to actually adjust the values in the system.

    {{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/timer/standalone-timer-activity.png" alt="Standalone Timer activity" width="250" >}}

* **Timer** can also be attached to another workflow activity as a [Boundary Event](/refguide/workflow-boundary-events/):

    {{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/timer/timer-boundary-event.png" alt="Timer boundary event" width="300" >}}

## Properties

**Timer** properties consist of the following sections:

* [General](#general)
* [Boundary properties](#boundary-properties)
* [Timer](#timer)
* [Recurrence](#recurrence)
* [Common](#common)

### General Section {#general}

The **Caption** describes what happens in this element. It is displayed under the workflow element to make the **Timer** easier to read and understand without the need to add annotations.

### Boundary Properties Section {#boundary-properties}

{{% alert color="info" %}}
This section is only applicable when **Timer** is used as a timer boundary event.
{{% /alert %}}

The **Interrupting** property sets the timer boundary event to be either interrupting or non-interrupting.

By default, it is set to **No**, which means that the timer boundary event is non-interrupting. When it is set to **Yes**, the timer boundary event is interrupting. For more information, see [Boundary Events](/refguide/workflow-boundary-events/).

### Timer Section {#timer}

The **Timer** property can be configured in two ways: you can set a certain duration or a date and time with an expression. When the workflow path reaches the timer, the configured duration or date and time will be scheduled to take effect.

The **Timer** properties are described in the table below:

| Type | Description |
| --- | --- |
| Duration | You can set a certain duration for the timer. With the **Continue after** setting, you can indicate the number of seconds, minutes, hours, days, weeks or months the timer's duration is. Possible values for the setting are:<br /><ul><li>Seconds</li><li>Minutes</li><li>Hours</li><li>Days</li><li>Weeks</li><li>Months</li> </ul> |
| Expression | You can set a certain date and time for the timer by writing an expression via the **Continue at** setting.<br><br>For example, you can write `addDays([%CurrentDateTime%], 1)` to set tomorrow as the due date and time. To set a static date and time, you can use the expression `parseDateTimeUTC('2023-12-10T17:12:00.000', 'yyyy-MM-dd''T''HH:mm:ss.SSS')`.<br><br>You can also create a more complex timer. For example, you can set a timer based on a Boolean value (in this example, `isVIPUser`) from the provided workflow context entity: `if $WorkflowContext/isVIPUser then addDays([%CurrentDateTime%], 2) else addWeeks([%CurrentDateTime%], 2])`.<br><br>For more information on available expressions in Mendix, see [Expressions](/refguide/expressions/). |

### Recurrence Section {#recurrence}

{{% alert color="info" %}}
The **Recurrence** property is only applicable when **Timer** is used as a non-interrupting timer boundary event.
{{% /alert %}}

The **Recurrence** property allows a non-interrupting timer boundary event to run multiple times when the specified interval has elapsed. The following parameters can be set.

| Type | Description |
| --- | --- |
| Interval | You can set a certain duration for the timer. With the **Repeat every** setting, you can indicate the number of minutes, hours, days, weeks or months the timer interval duration is. Possible values for the setting are:<br /><ul><li>Minutes</li><li>Hours</li><li>Days</li><li>Weeks</li><li>Months</li> </ul> |
| Max occurrences | The maximum number of occurrences, including the first boundary event execution.|

{{% alert color="info" %}}
When recurrence is set, the non-interrupting boundary event is first executed after the specified timer duration. From that point onward, it recurs with the set interval.
{{% /alert %}}

### Common Section {#common}

{{% alert color="info" %}}
This section is only applicable when **Timer** is used as a standalone activity on a workflow path.
{{% /alert %}}

**Name** is the internal name of the **Timer**. When referring to the activity in an application, you will use this name. It must be unique within the workflow, but you can have two **Timer** activities with the same name in different workflows.

## Timer Expiration {#timer-expiration}

When a **Timer** expires, it behaves differently depending on the state of the workflow:

* When a timer is set on an in-progress workflow, the workflow continues when the timer expires.

* When a time is set on a paused workflow and when the timer expires, the workflow only continues after the workflow is in progress again.

### Specific Workflow State Cases

The following cases do not trigger a continuation of the workflow path when timer expires.

* Expiration in a workflow that is aborted.
* Expiration in a workflow that is incompatible - After the workflow resumes, the workflow path continues normally.
* Expiration in a workflow that is jumped from the timer to a different activity. 
* Expiration in a workflow that is completed - This can only occur when **Timer** is used as a [Boundary Event](/refguide/workflow-boundary-events/).
* A workflow is restarted and a previous timer was still scheduled.

### Workflow Incompatibility

{{% alert color="info" %}}
This section is only applicable when **Timer** is used as a standalone activity on a workflow path.
{{% /alert %}}

When a **Timer** activity is added to the workflow definition and the application is redeployed, a validation on already running workflow instances is performed. When the **Timer** activity has been added before the currently in-progress activity, the workflow becomes incompatible. The conflict/incompatibility validation is analogous to other activities added before an in-progress activity. For more information, see [Workflow Versioning and Conflict Mitigation](/refguide/workflow-versioning/).

When a **Timer** activity is removed from the workflow definition and the application is redeployed, on initiation of the application, it validates if there are any running timers (that is, active timers that are initiated but have not reached their defined date and time). In this case, the workflow becomes incompatible and a warning log is created. For information on how to resolve a conflict when an activity is removed, see [Workflow Versioning and Conflict Mitigation](/refguide/workflow-versioning/).

## Read More

* [Workflows](/refguide/workflows/)
* [Add Date Function Calls](/refguide/add-date-function-calls/)
* [Parse and Format Date Function Calls](/refguide/parse-and-format-date-function-calls/)
* [Workflow Versioning and Conflict Mitigation](/refguide/workflow-versioning/)
