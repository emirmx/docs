---
title: "Backups"
url: /developerportal/deploy/mendix-on-azure/backups/
weight: 13
description: "Describes the Backups page of Apps."
#To update these screenshots, you can log in with credentials detailed in How to Update Screenshots Using Team Apps.
---

## Introduction

Backups allows to create/restore database/file documents back ups for apps running in Mendix on Azure.

{{% alert color="warning" %}}
Features like Upload backups, Nightly backups and Download backups are not yet available for Mendix on Azure.
{{% /alert %}}

Backup snapshots contain both the database and file documents referred to in the database.

{{% alert color="info" %}}
This page describes backups for apps deployed to Mendix on Azure. Hence, creating/restoring the backups is only possible for **Mendix on Azure** apps currently. If you would like to use this feature, **Try new Backup and Restore** needs to be enabled in Backups page.
{{% /alert %}}

## Backups{#backups}

The **Backups** page presents options for managing your backups. These are described below.

{{< figure src="/attachments/deployment/mx-azure/backups/backup-controls.png" alt="" >}}

### Create Backup

You can enable the **Try new Backup and Restore** option in order to enable this new feature.

#### Prerequisites

You have Access to Backups permission (**Manage Apps Backups**) for the namespace.

#### Creating a Backup {#creating-backup}

1. Go to [Apps](https://sprintr.home.mendix.com) and select the app.

2. Click **Backups** in the navigation pane.

3. On the right side of the page, select the environment from the dropdown for which you want to create a backup. 

{{% alert color="info" %}}
Its not allowed to create backups for environments with below condition:

	* Environment creation is in progress
	* Environment creation failed
	* A deployment package is being deployed in the environment
	* Any environment in transition state (where runtime is processing) will not appear in the list
{{% /alert %}}

The environment with above status will not be visible in the environment selection dropdown.

4. Click **Create Backup**. 

5. You can check the status of the backup under Status column. 

{{% alert color="info" %}}
If you want to restart your environment after creating a backup archive, wait until the backup completes. Tables are locked while the database is in the process of creating a backup, so you may receive a timeout error if you try to start your environment while the backup is being created.
{{% /alert %}}


#### Details {#backups-details}

You can view details of a backup by clicking **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) and then **Details**. You will see the following details:

| Backup Details                      | Description                                                                                   |
| :---------------------------------- | :-------------------------------------------------------------------------------------------- |
| **Status**                          | The status of the backup (**Processing**,**Failed**, or **Finished**) |
| **Snapshot id**                     | A unique identifier for the backup snapshot                                                   |
| **Created on**                      | The creation date and time of the backup                                                      |
| **Deployment Package**              | The version of the deployment package used during backup creation                             |
| **Comment**                         | A comment added to the backup                                                                 |

{{< figure src="/attachments/deployment/mx-azure/backups/backup-details.png" alt="Backup Details" max-width=60% class="no-border" >}}

#### Delete Backup {#backups-delete}

You can delete the backup snapshots by clicking **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) and then **Delete**. If not deleted, the backup file will stay until you explicitly delete it from the portal. 


### Restore Backup {#restore-backup}

To restore a backup, click **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) > **Restore** on the backup you want to restore.

{{< figure src="/attachments/deployment/mx-azure/backups/backup-restore.png" alt="Backup Restore" max-width=60% class="no-border" >}}

{{% alert color="info" %}}
You can restore a backup that has been stored in Mendix on Azure environment. Back up from other cloud providers is not supported.
{{% /alert %}}

{{% alert color="info" %}}
You can only restore a backup if you have **Manage Apps Backups and Stop Apps** permissions in the respective namespace.
{{% /alert %}}

You can choose the destination environment to which you want to restore the backup snapshot. This allows you to, for example, restore a production environment backup to an acceptance environment.

If you restore a backup snapshot that was originally deployed with a different Mendix version, you will get a warning. You can still restore the data, but you will have to deploy the same model later on.

{{% alert color="info" %}}
If the app is still running, you have to stop it by clicking Stop Application. Then click Restore Backup again. No updates should be done to the environment while restore process is under progress. 
{{% /alert %}}

The environment activity log will indicate status as **FINISHED** when the restore has completed. Your environment details page will display a message while the backup is being restored
 
If a backup restore fails, the failure is logged in your app’s Backup Activity log, which you can view on the Backups page of your app. If this happens, all data that was restored until the point of failure will be present in your database. This will leave the database only partially restored; not all data from the backup file will be present in your database. The failed restore process will log **FAILED** status in activity logs.

{{% alert color="info" %}}
There is currently no API support to perform the backup/restore.
{{% /alert %}}

## Limitations

* Please note: While the portal interface might allow you to restore backups across different namespaces, this operation is unsupported. Backup and restore operations must be performed within the same namespace only.