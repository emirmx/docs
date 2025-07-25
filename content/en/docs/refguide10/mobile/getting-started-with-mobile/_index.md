---
title: "Getting Started with Mobile"
url: /refguide10/mobile/getting-started-with-mobile/
weight: 10
description: Create a native mobile Mendix app and view it on a mobile device using the Make It Native test app.
aliases:
    - /howto10/mobile/get-started/
    - /howto10/mobile/getting-started-with-native-mobile/
---

## Introduction

To use Mendix Studio Pro's native mobile app capabilities, you can use the [Blank Native Mobile App](https://marketplace.mendix.com/link/component/109511/) app from the Mendix Marketplace. This app is optimized to quickly build a native mobile app. Out of the box, this app gives you a native page, a native phone profile to enable native device navigation, a native layout with menus, and native widgets and actions which leverage device capabilities.

The Blank Native Mobile App also includes six modules:

* Administration: helps you manage users
* Atlas Core: enables app styling
* Atlas Native Mobile Content: allows you to style native mobile apps with page templates and building blocks
* Atlas Web Content: allows you to style web apps and progressive web apps with page templates and building blocks
* [Nanoflow Commons](/appstore/modules/nanoflow-commons/): contains generic useful nanoflow actions
* [Native Mobile Resources](/appstore/modules/native-mobile-resources/): contains various native widgets and nanoflow actions that leverage device capabilities

## Prerequisites {#prerequisites}

Before starting this guide, make sure you have completed the following prerequisites:

* Have a mobile device to test your native mobile app 
* For information on device requirements, see [System Requirements](/refguide10/system-requirements/)
* If you wish to use an emulator for Android mobile testing, install a product such as [Bluestacks](https://www.bluestacks.com/nl/index.html) or [Genymotion](https://www.genymotion.com/) (your emulator must have Google Play services supported)

{{% alert color="warning" %}}
Android devices running MIUI (Xiaomi/Redmi devices) are not supported as Mendix test devices. They can run a production version of your Mendix app, but not the development version based on the [Make It Native App](/releasenotes/mobile/make-it-native-9/) or a [custom developer app](/refguide10/mobile/distributing-mobile-apps/building-native-apps/how-to-devapps/).
{{% /alert %}}

## Creating a New App Based on the Quickstarter App {#quickstartapp}

For more information on building native mobile apps, see the [Build a Native Mobile Inspection App](https://academy.mendix.com/link/path/66) learning path (you must be signed in to the Mendix Platform to see this learning path).

### Starting a Quickstarter App 

To start a new app based on a template, follow these steps:

1. Open Mendix Studio Pro. Select **File** > **New App**, and then select the **Blank Native Mobile App**.
2. Click **Use this starting point**.
3. Click **Create app** to close the dialog box.
4. Click **Run Locally** ({{% icon name="controls-play" %}}) to see the app in action. Please note that starting a native mobile app for the first time can take a bit longer (about one minute total) than subsequent instances.
5. After running your app, you may see a Windows Security Alert dialog box. Accept the permissions selected by default and click **Allow access** to close the dialog box.
6. If asked to create database **'default'**, click **Yes**.

At this point you have a running native mobile app. To view your app on a mobile device, however, you need to download the Make It Native app.

### Downloading and Installing the Make It Native App {#download-min}

Depending on the Mendix version your app is developed in and the device you want to run on, you need a different Make It Native app. For more information on how to download the correct version, see the [Getting the Make It Native App](/refguide10/mobile/getting-started-with-mobile/prerequisites/#get-min-app) section in *Native App Prerequisites and Troubleshooting*.

### Viewing Your App on Your Testing Device

Viewing your app on a mobile device will allow you to test native features and other aspects of your app. This section is written for mobile devices, but you may use an Android emulator mentioned in the [Prerequisites](#prerequisites) section above. To view your app, follow these steps:

1. Locate your app's QR code in Mendix Studio Pro by clicking the drop-down menu next to the **View App** button, then selecting **View on your device** and navigating to the **Native mobile** tab. Here you will see your test app's QR code.
2. Start the Make It Native app by tapping its icon on your device.
3. Tap the  **Scan a QR Code** button:

    {{< figure src="/attachments/howto10/mobile/native-mobile/get-started/getting-started-with-native-mobile/scan-qr.png" alt="Scan QR Code"   width="500"  class="no-border" >}}

4. If prompted, grant the app permission to access your device's camera.
5. Point your mobile device's camera at the QR code. It will automatically launch your test app on your mobile device.

{{% alert color="warning" %}}
Your mobile device has to be on the same network as your development machine for the Make It Native app to work. If this is the case and the connection still fails, make sure that communication between devices is allowed in the Wi-Fi access point. Also, Mendix recommends keeping the **Runtime port** in **App Settings** > **Edit** on **8080**. If you change it, do not change it to **8083**, because that is designated for app packaging.
{{% /alert %}}

Now you can see your app on your device. While this is just a template app, whenever you make changes you will be able to view them live on your Make It Native app.

You may notice an **Enable dev mode** toggle on the Make It Native app home page. Turning this toggle on will give you more detailed warning messages during error screens, as well as additional functionality on the developer app menu:

{{< figure src="/attachments/howto10/mobile/native-mobile/get-started/getting-started-with-native-mobile/enable-dev-mode.png" alt="enable dev mode"   width="500"  class="no-border" >}}

### Viewing Changes to Your App on Your Testing Device {#viewingchanges}

To see how changes made in Mendix Studio Pro are displayed live on your testing device, make a small change to your app.

1. Put a text widget on your app's home page. Then, write some text into it. In this example, "Native rules!" has been added: 

    {{< figure src="/attachments/howto10/mobile/native-mobile/get-started/getting-started-with-native-mobile/new-text-studiopro.png" alt="new studio pro text" class="no-border" >}}

2. Click **Run Locally** ({{% icon name="controls-play" %}}) to automatically update the running app on your device, and see your new text. When you click **Run Locally**, your app will automatically reload while keeping state. 

If you get an error screen while testing your app, there are easy ways to restart it: 

* Tap your test app with three fingers to restart your app
* With the **Enable dev mode** toggle turned on, hold a three-fingered tap to bring up the developer app menu—here you can access **ADVANCED SETTINGS** and **ENABLE REMOTE JS DEBUGGING** 

For more detailed instructions on debugging a native mobile app, see [Debug Native Mobile Apps (Advanced)](/howto10/mobile/native-debug/).

## Read More

* [Native App Prerequisites and Troubleshooting](/refguide10/mobile/getting-started-with-mobile/prerequisites/)
* [How to Build Pluggable Widgets](/howto10/extensibility/pluggable-widgets/)
* [Native Mobile Styling Guide](/refguide10/native-styling-refguide/)
* [How to Debug Native Mobile Apps (Advanced)](/howto10/mobile/native-debug/)
