---
title: "App Menu"
url: /refguide10/app-menu/
description: "Describes the App Menu in Studio Pro."
weight: 30
---

## Introduction

In the **App** menu, you can view and/or manipulate settings that are connected to your app and deployment. For example, you can create a deployment package.

{{< figure src="/attachments/refguide10/modeling/menus/app-menu/app-menu.png" alt="App Menu" class="no-border" >}}

## Tools

Under **App** > **Tools**, you can find settings on updating widgets, button icons, and layouts, checking widgets, and converting your classes to **Design** properties.

### Batch Update Button Icons {#batch-update-button-icons}

The **Batch Update Button Icons** option allows you to update many button icons in a single batch process.

### Batch Update Layouts

The **Batch Update Layouts** option allows you to update the layouts of many pages in a single batch process.

### Check Widgets

The **Check Widgets** option checks that the widgets you have implemented in the app have been built correctly.

### Update Widgets {#update-widgets}

The **Update Widgets** option presents the current versions of the widgets you are using in your app, what the latest versions of the widgets are, and an update option

### Convert Classes to Design Properties

The **Convert classes to design properties** option allows you to convert classes in widgets into design properties to assist in changing the widget styling. See [Native Styling](/refguide10/mobile/designing-mobile-user-interfaces/native-styling/) for more information.

### Generate Bill of Materials {#generate-bill-of-materials}

The **Generate Bill of Materials** option allows you to generate a bill of materials for the current project. See [SBOM Generation](/refguide10/sbom-generation/) for more information.

## Synchronize App Directory {#synchronize}

The **Synchronize App Directory** option creates folders inside the app directory (resources, widgets, theme, etc.), if necessary. It also reads the widget packages that are currently inside the widgets folders. For example, if you add widgets to the widgets folder, you needs to synchronize the app directory for them to appear in the **Toolbox**.

Shortcut key: <kbd>F4</kbd>

## Show App Directory in Explorer

The **Show App Directory in Explorer** option shows the directory that contains the app file (*.mpr*) and other assets such as resources and Java actions in Windows Explorer. By default, the directory is located in the **MyDocuments** section.

The following directories in the app directory are useful for customizing the app style and adding custom widgets and Java actions:

* **theme** – stores the *.css* files that can be used to style the application
* **javasource** – stores the JavaScript actions
* **widgets** – stores the widgets

## Deploy for Eclipse{#eclipse}

The **Deploy for Eclipse** option deploys the app to the deployment directory. The Java stubs are generated so that you can start editing them in Eclipse. This action does not compile the Java actions. Use this if you are writing Java actions and you want to compile and debug them through Eclipse.

Shortcut key: <kbd>F6</kbd>

For more information on how write Java actions from Eclipse, see [Using Eclipse](/refguide10/using-eclipse/).

## Create Deployment Package{#create-package}

The **Create Deployment Package** option creates a Mendix Deployment Archive package (*.mda*) that contains all necessary files to run the app. This can be used if you want to deploy your app on a Windows server or on a custom Mendix Cloud.

Shortcut key:  <kbd>F7</kbd>

For more information on settings displayed on the Create Deployment Package dialog box, see [Create Deployment Package](/refguide10/create-deployment-package-dialog/).

## Clean Deployment Directory {#clean-deployment-directory}

The **Clean Deployment Directory** option cleans the deployment directory.

## Deploy to Licensed Cloud Node {#deploy}

The **Deploy to Licensed Cloud Node** option deploys the latest committed revision of a Team Server app to the associated Mendix Cloud node.

Shortcut key:  <kbd>Ctrl</kbd> + <kbd>F5</kbd>

{{% alert color="warning" %}}
The [Mendix Studios Target](/developerportal/deploy/studio-deployment-settings/#target) needs to be set and the deploying user needs to have transport rights to the set target.
{{% /alert %}}

For more information on using this option, see [Deploy to the Cloud](/refguide10/deploy-to-the-cloud-dialog/).

## Build Native Mobile App

If you have an app with a [native profile](/refguide10/navigation/#native-phone), this option launches the [Mendix Native Mobile Builder](/releasenotes/mobile/mendix-native-mobile-builder/). This wizard will guide you through the required steps for configuring and publishing your app. 

For the first step in your native mobile journey, see [Build a Mendix Native App Locally](/refguide10/mobile/distributing-mobile-apps/building-native-apps/native-build-locally/).

## Synchronize Dependencies 

Dependency synchronization is automatically triggered to run in the background whenever changes are made to a specific dependency. It also occurs when you open your app in Studio Pro.

You can trigger manual synchronization by opening the **App** menu and selecting **Synchronize Dependencies**. 

For more information, see the [Dependency Sychronization](/refguide10/managed-dependencies/#dependency-synchronization) section of *Managed Dependencies*.

## Show Security Overview

The **Show Security Overview** option opens the [Security Overview](/refguide10/security-overview/), where you can view a unified overview of your app's security. 

## Read More

* [Studio Pro Overview](/refguide10/studio-pro-overview/)
* [Deploying Apps](/deployment/)
