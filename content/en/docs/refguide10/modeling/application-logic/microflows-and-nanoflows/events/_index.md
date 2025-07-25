---
title: "Events"
url: /refguide10/events/
weight: 90
---

## Introduction

Events are elements that are displayed as circles on a flow of your microflow and are usually placed at the end or the beginning of the flow:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/events/events.png"   width="200"  class="no-border" >}}

For example, they are used to start or end your microflow, to break an iteration in a loop, or continue this iteration, depending on the type of the event. All of the events can be used both in a microflow or nanoflow, except for the error event. 

You can add the following events to your flow:

* [Start Event](/refguide10/start-event/) – indicates the beginning of your microflow or nanoflow 
* [End Event](/refguide10/end-event/) – defines where the flow stops
* [Error Event](/refguide10/error-event/) – defines where the microflow will stop and throw an error
* [Continue Event](/refguide10/continue-event/) – used in loops to stop the current iteration and start the iteration of the next object
* [Break Event](/refguide10/break-event/) – used in loops to exit the loop and continue with the rest of the flow
