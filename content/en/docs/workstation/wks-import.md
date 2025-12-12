---
title: "Managing and Sharing Workspace and Station Data"
linktitle: "Managing and Sharing Data"
url: /mendix-workstation/import-export/
description: "Describes how to import and export workspace and station data across workspaces and environments."
weight: 30
---

## Introduction

You can manage and share station and workspace data across various workspaces and environments by exporting and importing the configuration of a single station, or of multiple stations at the same time. If performed by the workspace admin, the import includes applications and devices associated with the station. In this way, you can replicate or migrate of station setups, supporting efficient and consistent data handling.

## Importing and Importing Stations in Bulk

{{% alert color="info" %}}
This option is only available to licensed users. For more information, see [Mendix Workstation Client](/mendix-workstation/).
{{% /alert %}}

To transfer multiple station configurations, along with their associated applications and devices, between workspaces, perform the following steps:

1. Open the [Workspaces](https://workstation.home.mendix.com/) page as the workspace admin.
2. Click the workspace whose stations you want to export.
3. On the **Stations** page, click the three-dot menu in the top right corner of the screen, and then click **Export Stations**.

    The **Dowload Stations** dialog opens. You can either download all the stations created for the workspace, or select individual stations from the list.

4. Click **Download**.

    The export is saved to your computer in JSON format.

5. Go to the workspace where you want to import the stations.
6. On the **Stations** page, click the three-dot menu in the top right corner of the screen, and then click **Import Stations**.

After the import finishes, your target workspace has the same apps and devices as the source workspace.

## Importing and Exporting a Single Station

To transfer the contents of a single station, perform the following steps:

1. On the **Stations** page, click the three-dot menu in the top right corner of the screen, and then click **Copy Station to Clipboard**.

2. Click **Create Station > Copy from Clipboard**.
3. Click **Create Station**.
