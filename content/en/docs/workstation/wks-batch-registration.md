---
title: "Registering Workstation Clients"
url: /mendix-workstation/register/
description: "Describes how to register and pre-configurer multiple Workstation Clients."
weight: 30
---

## Introduction

Installed on each local workstation, the Workstation Client acts as a bridge between the Mendix app and local hardware. The Workstation Client is installed on a computer in order to establish a connection with the Workstation Connector, and through it, your Mendix app. 

Mendix Workstation supports both individual registration of local Workstation clients, and bulk rollouts for large production environments.

## Registering a Single Workstation Client

If you are developing or testing Workstation configurations, you can register a single Workstation Client for your local computer by performing the following steps:

1. Navigate to the **Workspaces** page in [Workstation Management](https://workstation.home.mendix.com/).
2. Click **Create Workspace**, or select an existing workspace from the overview.
3. Click **Create Station**.
4. Enter a name for the station and optionally select or create a group to categorize it, such as *Assembly*.
5. Add devices in the **Devices** section.
6. Click **Register Computer** to register your computer.
7. Click **Download** to navigate to the Workstation Client listing in the Marketplace, download the Client installer for Windows, install it, and launch it.
8. Copy the registration token and paste it into the [Workstation Client](/mendix-workstation/installation/) registration field.

## Bulk-Registering Workstation Clients

In a production environment, you can register multiple Workstation clients and their host computers in a single rollout. This enables large-scale deployments on production floors (for example, factory shop floors) where dozens to hundreds of machines require setup.

{{% alert color="info" %}}
This feature is only available to licensed Mendix Workstation users. For more information about obtaining a Workstation license, see [Mendix Workstation](/mendix-workstation/).
{{% /alert %}}

To bulk-register Workstation Clients, perform the following steps:

1. Open the [Workspaces](https://workstation.home.mendix.com/) page.
2. Click the workspace where you want to register the clients.
3. On the **Stations** page, click the three-dot menu in the top right corner of the screen, and then click **Bulk Register**.

    The **Create Bulk Registration Token** dialog opens. You can use it to activate a time-limited token which can then be entered into the registration field of multiple Workstation Clients.

4. Specify the timeframe during which the token is valid.
5. Copy the token to a clipboard and save it in a secure location. For security reasons, the token is only displayed once.
6. Click **Activate Token**. The **Stations** page displays the timeframe during which the bulk registration is scheduled.
7. Use an automated script to distribute the token to client computers during the allowed timeframe. For example, on Windows machines, you can use the following script: `Start-Process -FilePath {path where the Workstation Client is installed} -ArgumentList "--registration-token {bulk registration token}" -Wait`

    After the command runs, the Workstation Clients display the status **Waiting for station assignment**. This indicates that the clients are registered, but not yet associated with a specific station. On the **Stations** page in Workstation Management, you can now see placeholder stations also named **Waiting for station assignment**. These stations correspond to computers which were registered during bulk import.

8. On the **Stations** page, click **View** by a placeholder **Waiting for station assignment** station.
9. Review the configuration and click **Assign to station** to reassign the computer to an existing station, or **Accept** to create a new station.

### Automatically Assigning Computers to Stations

Instead of reassigning computers to stations manually after the bulk import, you can configure stations to automatically accept computers with a specific name.

1. On the **Stations** page, click the three-dot menu by the station where you want to automatically register a computer.
2. Click **Edit Station**.
3. In the **Auto-Accepted Computer Name** field, enter a computer name.

Computers with this name are automatically assigned to the station during the bulk import.