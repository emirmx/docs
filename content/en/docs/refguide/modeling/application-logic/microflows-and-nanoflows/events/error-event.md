---
title: "Error Event"
url: /refguide/error-event/
weight: 3
---

{{% alert color="info" %}}
This event can only be used in **Microflows**.
{{% /alert %}}

## Introduction

An error event defines where a microflow will stop and re-throw an error that occurred earlier. If you call a microflow, you may want to know whether any errors occurred within the microflow or not. This event throws the error again, so the caller of the microflow can catch them.

When you use this event, it creates a new error of the type which occurred earlier. Because this is a new error, even if the error that occurred earlier was caught **without rollback**, all database actions within the current transaction will be rolled back (for more information, see [Error Handling in Microflows](/refguide/error-handling-in-microflows/)).

{{% alert color="warning" %}}
You can only use an error event if an error is in scope: Studio Pro does not allow you to connect the normal execution flow to an error event, because there would not be an error to pass back to the caller.
{{% /alert %}}

## Example of Error Event

In the example below, an error flow is defined when committing an object to the database. Any error is caught, and the flow continues to the error event where the error is passed back to the caller of the microflow. This allows you to implement your error handling on multiple levels.

{{< figure src="/attachments/refguide/modeling/application-logic/microflows-and-nanoflows/events/error-event/error-event.png" class="no-border" alt="A microflow with a parameter of 'MyEntity'. It has a single action committing 'MyEntity' which has an error flow ending in an error event and the normal flow ending in an end event" >}}

{{% alert color="info" %}}
When adding an error event, you need to add an [error handler](/refguide/error-handling-in-microflows/#errorhandlers) for an activity before the error event. Link an error event and an activity which has an error handlers set on it with a [sequence flow](/refguide/sequence-flow/) and select **Set as error handler** for the sequence flow.
{{% /alert %}}

## Read More

* [Error Handling in Microflows](/refguide/error-handling-in-microflows/)
* [Error Handling in Nanoflows](/refguide/error-handling-in-nanoflows/)
