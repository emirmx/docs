---
title: "Continue Event"
url: /refguide10/continue-event/
weight: 4
---

## Introduction

{{% alert color="warning" %}}
Continue events can only be used inside [loops](/refguide10/loop/).
{{% /alert %}}

A continue event is used to stop the current iteration and start the iteration of the next object inside a loop. 

Normally, the iteration of the next object will be started automatically. So, a continue event is only necessary after a [decision](/refguide10/decision/) (when one of the paths does not have a destination yet), as both paths of a decision need a destination. However, for clarity you could choose to always include one.

For example, you have a list of objects of the *OrderLine* entity and you want to set the purchase date for only the paid order lines. This can be done using a loop with a decision, a continue event, and a change activity. The decision determines whether the order line is paid or not. If the order line is paid, the purchase date is set by a change activity and the looped activity starts with the next order line. If the order line is not paid, the loop starts with the next order line because of the continue event.

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/events/continue-event/continue-event.png" class="no-border" >}}

## Read More

* [Loop](/refguide10/loop/)
* [Break Event](/refguide10/break-event/)
