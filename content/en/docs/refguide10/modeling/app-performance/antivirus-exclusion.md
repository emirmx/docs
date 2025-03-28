---
title: "Antivirus Exclusion"
url: /refguide10/antivirus-exclusion/
description: "Describes how to configure antivirus exclusion in Studio Pro."
---

## Introduction

Antivirus software can sometimes interfere with the performance of Studio Pro. To mitigate these issues, you can configure antivirus exclusions for Studio Pro. For more information, see the [Antivirus Software](/refguide10/performance-tips/#antivirus-software) section of the *Performance Tips*.

This guide describes how to set up Microsoft Defender exclusions to ensure optimal performance.

## Antivirus Exclusion Overview

If you are using Microsoft Defender, Studio Pro recommends adding the app folder to the Microsoft Defender exclusions after loading an app:

{{< figure src="/attachments/refguide10/modeling/app-performance/exclude-folder.png" class="no-border" >}}

### Excluding the App Folder

To exclude the app folder, click on **Exclude Folder** in the notification.

This operation requires running with administrator privileges. After completion, a confirmation popup appears:

{{< figure src="/attachments/refguide10/modeling/app-performance/exclude-folder-success.png" class="no-border" >}}

If the operation fails, a failure popup appears:

{{< figure src="/attachments/refguide10/modeling/app-performance/exclude-folder-failure.png" class="no-border" >}}

### Disabling the Antivirus Exclusion Notification

Studio Pro displays a notification when an app is loaded, prompting you to exclude the app folder. To disable the notification, open the **Edit** menu and click **Preferences** > **Advanced** and check the **Antivirus Exclusion** > **Do not show antivirus exclusion notifications** option:

{{< figure src="/attachments/refguide10/modeling/app-performance/antivirus-exclusion/disable-antivirus-exclusion.png" class="no-border" >}}