---
title: "Connecting to External Resources"
url: /developerportal/deploy/connecting-to-external-resource/
weight: 80
description: "How to connect to external resource using private connectivity"
beta: true

#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

[Mendix Cloud Private Connectivity](/control-center/private-connectivity/) can help you with connecting your applications on Mendix Cloud to your internal resources (on-premises and in the cloud) securely and privately. That means that the connection will not go over the public internet, but through a private tunnel.

## Connections {#connections}

Mendix Cloud Connect Connections allow applications on Mendix Cloud to connect to Mendix Cloud Connect Resources over Mendix Cloud Connect Networks. A Connection has to be requested and approved, before an application on Mendix Cloud can connect to the Resource. An application on Mendix Cloud can have multiple Connections to multiple Resources.

The [Connections](/developerportal/deploy/environments-details/#connections) section on your application environment's Details page allows Technical Contacts to view all submitted connection requests and track request status.

### Requesting a New Connection {#connections-add}

Once a network has been created, agents have been added and installed, and Resources have been exposed and enabled, you can request a Connection from an application environment to of the approved Resources.

To request a new Connection for a specific application environment, follow these steps:
1. From [Apps](https://sprintr.home.mendix.com), go to the app's **Environments** page.
2. Click **Details** ({{% icon name="notes-paper-edit" %}}) on the desired environment.
3. Go to the **Network** tab.
4. The **Connections** section allows for managing connections for a single environment.
5. Click **Add** to request a new connection.
6. On the **Add Connection** dialog, select an available network to view the resources exposed on that network.
7. Select the resource you would like to connect to from the application environment.
8. All submitted connection requests appear in the Control Center for the Mendix Admin review. Click **Send Request**.
9. Track and manage your connection requests from the [Connections](/developerportal/deploy/environments-details/#connections) section on your application environment's Details page. 

###  Cancelling a Connection Request {#connections-cancel}

To cancel a pending connection request, follow these steps:
1. On the [Connections](/developerportal/deploy/environments-details/#connections) section on your application environment's Details page, click **Cancel**.

### Deleting a Connection

To delete an approved connection, follow these steps:
1. On the [Connections](/developerportal/deploy/environments-details/#connections) section on your application environment's Details page, click **Delete**.

Deleting a connection, will immediately break the connection between the application environment and the resource.