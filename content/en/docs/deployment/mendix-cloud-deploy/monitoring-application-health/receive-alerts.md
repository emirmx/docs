---
title: "Receive Environment Status Alerts"
url: /developerportal/operate/receive-alerts/
description: "Describes how to receive environment status alerts."
#To update these screenshots, you can log in with credentials detailed in How to Update Screenshots Using Team Apps.
---

## Introduction

You can receive alerts for Mendix application environments in two ways: via email or by using webhooks. You can find detailed information for both of these options below.

Alerts are triggered by any of the following circumstances:

* The app goes offline unexpectedly
* The application logs a message with level **Critical**
* The health check fails
* An infrastructure problem occurs

## Email Alerts

You can select the **Email Alerts** checkbox on the **Alerts** page to set up email alerts for an environment. For this, you need an **Application Operator**, **Business Engineer**, or **Scrum Master** role. Or, if a [Mendix Admin](/control-center/members/) has defined a new team role for you, that role must have **Cloud Access**.

To enable email alerts, follow these steps:

1. From [Apps](https://sprintr.home.mendix.com), open your app.
1. Click **Alerts** in the navigation pane.
1. Select the environment for which you want to set the alerts from the drop-down list at the top of the page. It is possible to set this option separately for each environment.
1. Click **Settings** to open the **Settings** tab.
1. Select (or clear) the **Email Alerts** checkbox.

    {{< figure src="/attachments/deployment/mendix-cloud-deploy/monitoring-application-health/receive-alerts/email-alerts.png" >}}

## Alerts Using Webhooks 

As an alternative to alert emails, you can set a webhook to receive alerts.

Webhooks are set on the application level.

For details on webhooks, see the [Webhooks](/developerportal/deploy/webhooks/) page.

### Setting Alerts Webhooks

To set a new webhook in the Mendix Portal, follow these steps:

1. From [Apps](https://sprintr.home.mendix.com), open your app.
1. In the navigation pane, click **Webhooks**.
1. Click **New Webhook**.
1. Name your webhook and provide the webhook receiver URL and Validation Secret as described in the [Configuring a Webhook](/developerportal/deploy/webhooks/#setting-up) section of the *Webhooks* page.
1. Choose **Alerts** from **Available Events**.
1. Click **Save**.

After you set a new webhook, the receiver endpoint will start getting alerts when they are triggered. 

## Issue: Receiving Too Many Alerts

Under some circumstances, a critical alert in one instance of a multi-instance app can cause emails to be sent out constantly. The other instances of your app will continue to run without reporting issues, so it will appear to be working.

If you cannot resolve the issue in the instance reporting the critical error, you will need to restart your app to ensure that all instances are healthy. Otherwise, you will continue to receive regular email alerts.

## Read More

* [Alerts](/developerportal/operate/monitoring-application-health/)
* [Metrics](/developerportal/operate/metrics/)
* [Mendix Cloud](/developerportal/deploy/mendix-cloud-deploy/)
* [About Mendix Cloud](/developerportal/deploy/mxcloudv4/)
* [App Roles](/developerportal/general/app-roles/)
* [Node Permissions](/developerportal/deploy/node-permissions/) 
* [Technical Contact](/developerportal/general/app-roles/#technical-contact)
