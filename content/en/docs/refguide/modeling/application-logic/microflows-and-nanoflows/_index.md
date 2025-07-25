---
title: "Microflows and Nanoflows"
url: /refguide/microflows-and-nanoflows/
weight: 10
description: "Presents an overview of microflows and nanoflows."
---

## Introduction

Microflows and nanoflows allow you to express the logic of your application. They can perform actions such as creating and updating objects, showing pages, and making choices. It is a visual way of expressing what traditionally ends up in textual program code.

Explore the documentation for details on microflow and nanoflow definitions, properties, and usages.

* [Microflows](/refguide/microflows/)
* [Nanoflows](/refguide/nanoflows/)
* [Sequence Flow](/refguide/sequence-flow/)
* [Activities](/refguide/activities/)
* [Decisions](/refguide/decisions/)
* [Annotation](/refguide/annotation/)
* [Parameter](/refguide/parameter/)
* [Loop](/refguide/loop/)
* [Events](/refguide/events/)
* [Common Properties](/refguide/microflow-element-common-properties/)
* [Debugging Microflows and Nanoflows](/refguide/debug-microflows-and-nanoflows/)

## Differences between Microflows and Nanoflows {#differences}

Microflows run in the runtime server and can therefore not be used in offline apps, while nanoflows run directly on the client side (that is, on the browser/device), and can be used in an offline app. Furthermore, most of the actions in nanoflows run directly on the device, so there is also a speed benefit for logic which does not need access to the server. 

Below presents a list of main differences between microflows and nanoflows:

* When a nanoflow steps through its actions, client actions are directly executed. For example, an open page action immediately opens a page instead of at the end of the nanoflow. This is different from client actions in a microflow, which only run when the client receives the result from the microflow.
* Nanoflows and microflows do not provide the same [activities](/refguide/activities/). Some activities available in microflows are not available in nanoflows, and vice versa.
* Because nanoflows use JavaScript libraries and microflows use Java libraries, there can sometimes be slight differences in the way [expressions](/refguide/expressions/) are executed.
* When used in nanoflow activities, expressions do not support the following objects and variables: `$latestSoapFault`, `$latestHttpResponse`, `$currentSession`, `$currentUser`, `$currentDeviceType`.
* Nanoflows are not run inside a transaction. So, if an error occurs in a nanoflow, it will not roll back any previous changes.
* <a id="list-changes-in-sub-nanoflows"></a>Changes done to the lists in a sub-nanoflow are not reflected in the original nanoflow.
* In nanoflows, when retrieving an `empty` attribute of an object, an empty string (`''`) is returned.
