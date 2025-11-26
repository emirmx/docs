---
title: "Connecting to External Resources"
url: /developerportal/deploy/connecting-to-external-resource/
weight: 80
description: "How to connect to external resource using private connectivity"
beta: true

#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="warning" %}}
This feature is in Public Beta. For more information, see [Release Status](/releasenotes/release-status/).
{{% /alert %}}

## Introduction

[Mendix Cloud Private Connectivity](/control-center/private-connectivity/) enables your Mendix applications to securely and privately connect to internal resources, whether they are on-premises or in the cloud. This ensures that the connection bypasses the public internet and instead goes through a private tunnel, enhancing security and compliance.

This document guides [Technical Contacts](/developerportal/general/app-roles/#technical-contact) through the process of requesting and managing connections to external resources using Mendix Cloud Private Connectivity.

## Prerequisites {#prerequisites}

Before requesting a connection, the following conditions must be met:

* You are the [Technical Contact](/developerportal/general/app-roles/#technical-contact) for the Mendix application.
* A Mendix Admin has created and configured the necessary private network in the [Control Center](/control-center/configure-private-connectivity/). This includes installing agents and exposing the specific external resource you want to connect to.

## Connections {#connections}

Mendix Cloud Connect Connections allow applications on Mendix Cloud to connect to Mendix Cloud Connect Resources over Mendix Cloud Connect Networks. 

Each connection request must be initiated by a Technical Contact through the [Mendix Support Portal](https://support.mendix.com/) before the application on Mendix Cloud can connect to the Resource. An application on Mendix Cloud can establish multiple connections to various resources.

{{% alert color="info" %}}
An application environment can only be connected to a single private network at a time. This means that all external resources you connect to from an application environment must be on the same private network.
{{% /alert %}}

### Requesting a New Connection {#connection-request}

As a Technical Contact, follow these steps to request a new connection from your application environment to an approved external resource:

1. Create a [support ticket](https://support.mendix.com/) with the following specifications:
    * **Ticket title** – Must contain the words "Mendix Cloud Private Connectivity Connection Request".
    * **Environment ID**  – Found in the [General Tab](/developerportal/deploy/environments-details/#application-status) of the **Environments Details** page.
    * **Resource ID** – Found on the **Resource Details** dialog on the [Private Connectivity](/control-center/configure-private-connectivity/#private-connectivity-resources) page in Control Center. Request this from your application's Mendix Admin.
    * **Restart timestamp** – Specify your preferred time for environment restart (or indicate "ASAP"). The scheduled time must be during the EMEA office hours (08:00  – 18:00 CET, Monday through Friday).
2. Submit the ticket and await confirmation from the support team.

Mendix Support will notify you when the connection request is added and ready to use.

