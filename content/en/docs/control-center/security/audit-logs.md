---
title: "Audit Logs"
url: /control-center/audit-logs/
description: "Describes the Audit Logs page in the Mendix Control Center."
weight: 30
beta: true
---

{{% alert color="warning" %}}
This feature is in Public Beta. For more information, refer to [Release Status](/releasenotes/release-status/).
{{% /alert %}}

## Introduction

## Audit Logs

The **Audit Logs** tab displays a list of all activities over the past 90 days. It includes the following details:

* **Date** – The date when the activity was logged.
* **Email** – The email of the user who performed the activity.
* **Action** – The activity that was performed.
* **Service** – 
* **Description** – 
* **Outcome** – 
* **Resource Name** –
* **Resource Type** –
* **Details** – The following information is displayed and can be downloaded for each audit log item:

    * **Log ID** – The unique identifier of the log.
    * **Date** – The date when the log was generated.
    * **Owner ID** –  
    * **Owner ID Type** – 
    * **User Email** – The email address of the user who performed the action that generated the log.
    * **User Name** – The name of the user who performed the action that generated the log.
    * **User ID** – The unique identifier of the user who performed the action that generated the log.
    * **IP Address** – The IP address corresponding to the user who performed the action that generated the log.
    * **Action** – The action that generated the log.
    * **Description** – 
    * **Outcome** – 
    * **Error Message** –
    * **Old Value** – The state of the entity prior to the change that generated the log.
    * **New Value** – The state of the entity after the change that generated the log.
    * **Resource Name** – The name of the resource that was changed.
    * **Resource Type** – The type of resource that was changed.   

You can use the **Filters** option to only display the audit logs that meet the criteria you are interested in, or you can use the search field to search for a specific log.

### Exporting Audit Logs

You can export audit logs in a CSV format. This is available for logs that are no older than 90 days old.     
You can choose between these options:

* Export all logs. To do that, click **Export All**.
* Export a selection of logs. To do that, select the log items you are interested in, then click **Export Selection**.
* Export logs from a specific timeframe. To do that, click **Export from Selected Date Range**, then select the start and end dates and times.
* Export filtered logs. To do that, filter by the criteria you are interested in, then click **Export All**. 
* Export from archive.

Once the logs are ready for export, a download link is displayed on the [Downloads](#downloads) tab.

## Downloads {#downloads}

When you export audit logs, a download link is displayed on this tab once the export CSV is ready for download.    
These are details available on the this tab:

* **Submitted on** – 
* **Requester** – The user who requested the export.
* **Expires in** – The number of days that the exported CSV is available for download.
* **Status** – The status of the CSV generation. Once the CSV export is fully generated, the status becomes **Completed**.
* **Time Frame of Export** – 
* **Download** – This link is displayed if the export CSV is ready for download.

## Retrieving Audit Logs via API

You can use the [Audit Logging API](/apidocs-mxsdk/apidocs/apis-for-audit-logs/) to manage and download audit logs. This API is particularly useful in scenarios where you want to download audit logs that are over 90 days old, but no more than one year old.
