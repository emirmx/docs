---
title: "Download from Version Control Server"
url: /refguide/download-from-version-control-dialog/
weight: 60
aliases:
    - /refguide/download-from-team-server-dialog.html
    - /refguide/download-from-team-server-dialog
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Use the **Download from Version Control Server** menu item to download an app from a version control server (for example, [Team Server](/developerportal/general/team-server/)). If you are currently editing an app, the app will be closed (after prompting to save any changes) and the newly downloaded app will be opened using the current version of Studio Pro.

{{< figure src="/attachments/refguide/modeling/menus/version-control-menu/download-from-version-control-dialog/download-from-version-control-server.png" alt="Download from Version Control Server dialog box" class="no-border" width="600" >}}

{{% alert color="info" %}}
If the downloaded app was created with a different version of Mendix, you will be asked if it can be converted to the current version.

You can also use the [Open App Dialog](/refguide/open-app-dialog/) to download and open an app from Team Server. However, you will need to use this option if you want to download a second copy of an app (and development line) you already have on disk.
{{% /alert %}}

{{% alert color="info" %}}
Either a full or partial clone will be downloaded, depending on the [Clone type](/refguide/clone-type/) set for your app.
{{% /alert %}}

## Where Is Your App Stored?

If **Enable private version control with Git** is set in the app [Preferences](/refguide/preferences-dialog/#enable-with-Git), you can choose between the **Mendix Team Server** or a **Private server**. If it is not enabled, you will only be able to choose an app from the Mendix Team Server.

### Mendix Team Server

Use the **Team Server App** dropdown to choose the Git app you want to download.

For more information about the Mendix Team Server, see [Team Server](/developerportal/general/team-server/).

### Private Server

Enter the URL of your private Git server in **App repository address** and click **Connect**.

{{< figure src="/attachments/refguide/modeling/menus/version-control-menu/download-from-version-control-dialog/download-from-private-server.png" alt="Download from Version Control Server dialog box" class="no-border" width="600"  >}}

## Development Line

Choose the **Development line** you want to download.

For more information about development lines, see [Version Control](/refguide/version-control/).

## App Directory

Choose the **App directory** to which want to download the app. The suggested name includes the name of the development line (*main* or the name of the branch line), but you can change this if you want.
