---
title: "Mendix Runtime"
url: /refguide8/runtime/
weight: 40
no_list: false
description_list: true 
---

## Introduction

The Mendix Runtime is effectively an interpreter which "runs" a Mendix model and serves pages to the user.

Each patch version of Mendix comes with its own version of the runtime which implements the features which are available in that version of Mendix. For example, runtimes for Mendix 8.4.1 and 8.4.2 are different and can only run Mendix apps built for that version.

## Runtime Overview

The Mendix runtime consists of two parts: the [Runtime Server](/refguide8/runtime-server/) and the [Mendix Client](/refguide8/mendix-client/). The relationship between the two is shown in the chart below.

The Runtime Server is launched on a cloud platform, executes microflows, and connects to files, the relational database, and any other required services. It waits to be contacted by the Mendix Client. The Runtime Server is described in more detail in [Runtime Server](/refguide8/runtime-server/).

The Mendix Client is started by the end-user. This can be within a web browser, or on another supported device. If it is in online mode, it starts a session with the Runtime Server which may or may not require authentication. The Runtime Server records the session details in the database so that the Mendix Client can make requests. The Mendix Client is described in more detail in [Mendix Client](/refguide8/mendix-client/).

The end-user interacts with the Mendix Client which then makes requests to the Runtime Server to process data or perform server-side functions (for example, microflows). At the end of the request, all state (including uncommitted data) is passed back to the Mendix Client. You can find more details of how this communication takes place in [Communication Patterns in the Mendix Runtime](/refguide8/communication-patterns/).

Passing state from the Runtime Server to the Mendix Client enables the Runtime Server to be stateless, which means that any Runtime Server instance can respond to a request from the Mendix Client. A load balancer decides which Runtime Server instance will respond to a request. When an end-user session ends, the Runtime Server removes references to that session.

Where there is more than one instance of an app, one of the instances will be the *Cluster Leader*. The Runtime Server in that instance will be responsible for a number of activities which cannot easily be distributed. These include:

* Session cleanup handling
* Cluster node expiration handling
* Background job expiration handling
* Unblocking blocked users
* Executing scheduled events
* Performing database synchronization tasks
* Clearing persistent sessions after a new deploy

More information on multiple instances is in [Clustered Mendix Runtime](/refguide8/clustered-mendix-runtime/).

{{< figure src="/attachments/refguide8/runtime/runtime-overview.png" alt="An overview of the Mendix Runtime" class="no-border" >}}

Each of the components of the chart is described below:

### External Services

External services provide data and other functions from outside your Mendix app. These can be external data sources like SAP, external display widgets like Google maps, or external data processing like IBM Watson machine learning. The Runtime Server communicates with these over HTTP(S) connections.

### Infrastructure

This is the hardware on which the Mendix app will be deployed. It is usually provided as Infrastructure as a Service (IaaS) which provides virtual machines in the public or private cloud. However, the infrastructure can also be physical machines running on-premises. Examples of infrastructure are Amazon Web Services (AWS), Microsoft Azure, or Windows Server machines.

### Files

This is where files are stored which are part of the data used by the app. In particular it contains the value of *FileDocument* objects, including images, which are binary objects that are stored outside the database to avoid size and performance restrictions.

### Relational Database

This is the database (or sometimes the schema of a shared database) which holds the objects as defined in the domain model(s) in the app.

### Platform

This is the operating system on which the Mendix app is running plus additional services, such as a database, which have been bound to the app.

### Instance

Also called the **App Container**. This launches and exposes the Runtime Server. There may be only one instance, but to provide high availability and better performance there can be many instances.

### Runtime Server

This is the server side of the Mendix runtime. It is described in [Runtime Server](/refguide8/runtime-server/).

### Load Balancer

The load balancer takes incoming requests from the Mendix Client and forwards them to a Runtime Server instance. It balances the load by making sure that requests are distributed evenly to the different instances.
The Mendix Client communicates with the load balancer using HTTPS. Communication on the server side of the load balancer, to environment instances, is performed using HTTP.

### Static Content

Every Mendix app needs static content to be served in order to load the Mendix client in the browser. This includes the scripts needed to start the Mendix Client, CSS files which define the app's theme, and JavaScript files which define client-side logic.

### Mendix Client

This is the browser or device which allows the end-user to interact with the app. This can be a web browser, such as Chrome, or a mobile device, such as an iPhone. It typically has a screen, pointer device, and input device to allow end-users to use the app. The Mendix Client is described in [Mendix Client](/refguide8/mendix-client/).

## Licensing

You need a license to run an application in production mode. Without a license, the Runtime Server goes to sleep after a couple of hours. Information on licensing Mendix apps can be found in [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/).

## APIs

You can extend the functionality of the Runtime Server by writing Java actions. For more information, see the [Mendix Runtime API](/apidocs-mxsdk/apidocs/runtime-api-9/).

{{% alert color="info" %}}
Links to available API documentation such as WSDLs for published web services are available on the URL path `/api-doc` (for example: `http://localhost:8080/api-doc/`).
{{% /alert %}}

## Documents in This Category
