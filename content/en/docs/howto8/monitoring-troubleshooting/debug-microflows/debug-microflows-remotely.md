---
title: "Debug Microflows Remotely"
url: /howto8/monitoring-troubleshooting/debug-microflows-remotely/
---

## Introduction

In addition to debugging a local deployment of your app, it is also possible to debug applications that are already in a cloud environment.

{{% alert color="warning" %}}
**Debugging in a production environment is not recommended.**

If you are debugging in the cloud, be aware of other app end-users. Breakpoints in the debugger will pause processes for all users of the app in this environment.
{{% /alert %}}

This how-to teaches you how to do the following:

* Connect the debugger in Studio Pro to your Mendix Cloud environment

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* Have access as a team member to a Mendix app deployed to a [licensed](/developerportal/deploy/licensing-apps/) Mendix Cloud environment
* Have **TRANSPORT RIGHTS** to the app environment you want to debug in the app's [node permissions](/developerportal/deploy/node-permissions/)

## The Basics

In Mendix Cloud, the debugger is always listening for connections so you cannot turn it on or off. To debug your app in the cloud, you need to get a URL and a password from the app environment and provide that information to Studio Pro. The steps below explain how to do this.

{{% alert color="info" %}}
The debugger supports only debugging of single-instance environments. Multi-instance environments need to be scaled down to one instance before the debugger can be used. See [Scaling Your Environment in Mendix Cloud](/developerportal/deploy/scale-environment/) for more information.
{{% /alert %}}

### Obtain Debugging Credentials

#### Obtain Debugging Credentials from Mendix Cloud

When your application is in Mendix Cloud, follow these steps:

1. Open your app in [Apps](https://sprintr.home.mendix.com/).

2. Click **Environments** in the navigation pane.

3. In the **Deploy** tab, click **Details** ({{% icon name="notes-paper-edit" %}}) for the environment that you want to debug:

    {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/environment-details.png" class="no-border" >}}

4. In the **General** tab, click **Show Debugger Information**:

    {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/show-debugger-information.png" class="no-border" >}}

    This invokes the **Debugger settings** pop-up window, which provides a URL (such as `http://yourapp.mendixcloud.com/debugger/`) and a password:

    {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/debugger-settings.png" class="no-border" >}}

You will need to provide these credentials to Studio Pro to connect the debugger to the app running in the cloud.

#### Obtain Debugging Credentials from Mendix on Kubernetes Connected{#private-cloud}

If your application is on a connected Mendix on Kubernetes, you can get the credentials from the Mendix Portal:

{{% alert color="warning" %}}
You can only remotely debug apps deployed to Mendix on Kubernetes for Mendix if you are using Mendix Operator version 1.6.0 or above.
{{% /alert %}}

1. Open your app in [Apps](https://sprintr.home.mendix.com/).

2. Click **Environments** in the navigation pane.

3. Click **Details** for the environment that you want to debug.

4. Open the **Debugger** tab:

5. If the debugger is currently disabled, click **Enable Debugger**. You will be asked to confirm the generated strong password. Mendix recommends not changing this password.

    {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/pc-debugger-password.png" alt="Enter password for the Mendix on Kubernetes debugger" class="no-border" >}}

    You will receive a warning that you have made some changes. Click **Apply Changes** to restart the app and apply the changes.

Once the debugger is enabled, you will see the **URL** and **Password** which are the credentials you need to supply to Studio Pro. Use the **Copy to clipboard** links to simplify providing the credentials.

{{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/pc-debug-tab.png" class="no-border" >}}

When the debugger is enabled, you can click **Disable Debugger** to disable it. You will be asked to confirm your choice and will receive a warning that you have made some changes. Click **Apply Changes** to restart the app and apply the changes.

#### Obtain Debugging Credentials from SAP S/4 HANA Cloud

If your application is on the SAP S/4 HANA cloud, you will need to set the password in the SAP Cockpit:

1. Log in to the SAP Cockpit and go to your application's settings page.

2. Go to your application > **User-Provided Variables**.

3. Click on the button 'Add variable' and add 'DEBUGGER_PASSWORD' and the password. Both are case-sensitive.

    {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/debugger-settings-saps4hana.png" alt="SAP Cockpit showing user-provided variables" class="no-border" >}}

4. Restart your application.

### How to Enable Cloud Debugging in Studio Pro

Once you have the unique URL and password, there are two methods for connecting Studio Pro to the cloud environment. 

{{% alert color="warning" %}}
If you do cannot connect the debugger, then you do not have sufficient permissions to your app. Ask the Technical Contact or the project Scrum Master to provide the correct permissions.
{{% /alert %}}

1. Open the **Connect Debugger** dialog box – you can do this in two ways within Studio Pro:

    * Go to the **Run** menu and select **Connect Debugger…**:

        {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/18580048.png" class="no-border" >}}
        
    * Click **Connect…** in the **Debugger** pane:

        {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/debugger-pane.png" class="no-border" >}}

2. In the **Connect Debugger** dialog box set the following:

    * **Connect to** – select the option *An app running in Mendix Cloud or on another remote server.*
    * **URL** – the *URL* from the **Debugger Settings** for your app environment
    * **Password** – the *Password* from the **Debugger Settings** for your app environment

        {{< figure src="/attachments/howto8/monitoring-troubleshooting/debug-microflows/debug-microflows-remotely/18580047.png" class="no-border" >}}

3. Click **OK**.

The debugger is now connected to your app running in the cloud.

## Read More

* [Find the Root Cause of Runtime Errors](/howto8/monitoring-troubleshooting/finding-the-root-cause-of-runtime-errors/)
* [Clear Warning Messages](/howto8/monitoring-troubleshooting/clear-warning-messages/)
* [Test Web Services Using SoapUI](/howto8/integration/testing-web-services-using-soapui/)
* [Monitor Mendix Using JMX](/howto8/monitoring-troubleshooting/monitoring-mendix-using-jmx/)
* [Debug Java Actions Remotely](/howto8/monitoring-troubleshooting/debug-java-actions-remotely/)
* [Log Levels](/howto8/monitoring-troubleshooting/log-levels/)
* [Debug Microflows](/howto8/monitoring-troubleshooting/debug-microflows/)
* [Debug Java Actions](/howto8/monitoring-troubleshooting/debug-java-actions/)
* [The Ultimate Debugger](https://www.mendix.com/tech-blog/the-ultimate-debugger/) 
