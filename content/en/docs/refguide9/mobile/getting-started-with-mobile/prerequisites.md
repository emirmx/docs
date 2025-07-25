---
title: "Native App Prerequisites and Troubleshooting" 
linktitle: "Prerequisites and Troubleshooting"
url: /refguide9/mobile/getting-started-with-mobile/prerequisites/
weight: 10
description: Troubleshoot common issues associated with building and running native mobile apps.
aliases:
    - /refguide9/getting-the-make-it-native-app/
    - /howto9/mobile/common-issues/
---

## Introduction

Developing mobile apps with Mendix requires some additional considerations. This guide explains the prerequisites of mobile development and helps you get started:

* The [Prerequisites](#prerequisites) section explains how to prepare for developing, deploying, and accessing mobile apps with Mendix
* The [Getting the Make It Native App](#get-min-app) section explains how to download the Make It Native App, which you can use to test your native mobile apps during development
* The [Troubleshooting Common Mobile Issues](#troubleshooting) section explains port forwarding, WiFi settings, and other common troubleshooting issues

## Prerequisites {#prerequisites}

### Progressive Web Apps

There are not special requirements for developing, deploying, and accessing Progressive Web Apps (PWAs) with Mendix.

### Native Mobile Apps

Native mobile apps are not run in the device browser like web-based Mendix apps. Instead, a native app is built for each mobile operating system it should run on resulting in an APK or IPA file, which are then installed on the device. This introduces several special considerations:

* **Developer Preview**: When developing a native mobile app, a special app called [Make It Native](/refguide/mobile/getting-started-with-mobile/prerequisites/) is required to preview the app. It is not possible to preview it in the browser or directly in Studio Pro using Design Mode.
* **Build Native App**: For building native apps, we provide a tool called Build Native App. This tool must be configured and run each time a new build is created. Build Native App is not supported on Studio Pro for Mac. Build Native App updates itself automatically by downloading new versions from AWS.
* **Native Template**: Build Native App needs to download the Native Template from [GitHub](https://github.com/mendix/native-template). This template contains the React Native project that is used to build the native app for iOS and Android.
* **Dependencies**: The Native Template makes use of several open-source projects, including React Native. These must be downloaded from several external services including npm, gradle, and Cocoapods.
* **Building**: Once prepared using Build Native App, the Native Template can be built for each target mobile operating system. Building can be done [locally](/refguide/mobile/distributing-mobile-apps/building-native-apps/native-build-locally/) by downloading the mobile operating system's IDE or [remotely](/refguide/mobile/distributing-mobile-apps/building-native-apps/bitrise/) using a third-party continuous integration and delivery (CI/CD) platform. For building remotely, a [GitHub account](https://github.com/) is required.
* **Deploying**: Most mobile devices refuse to install APK/IPA files without additional steps. At minimum, the file must be signed to identify its developer and prevent manipulation. Furthermore, for iOS and recent versions of Android, the app must be published via the official mobile operating system's store or a mobile device management (MDM) system.
* **Connectivity**: When running a Mendix native mobile app, it must connect to the Mendix Runtime at least once during startup. If no connection can be established, an error is shown. Connectivity is also needed to run microflows and to synchronize data.

### Air-Gapped Development

Developing and building native mobile apps requires access to several online resources. Without safelisting these resources, a native mobile build will fail.

For development:

* To preview their apps, developers need to be able to connect their mobile device running Make It Native to their Laptop running Studio Pro on ports 8080 and 8083.

For building:

* Download updates from AWS: `https://appdev-mx-cdn.s3.amazonaws.com/native-builders/latest.exe`
* Determine which native template version to download: `https://raw.githubusercontent.com/mendix/native-template/master/mendix_version.json`
* Download the native template from GitHub: `https://github.com/mendix/native-template/archive/refs/tags/*.zip`
* Upload the project to GitHub (optional): `https://api.github.com/`

In some situations, it can be beneficial to designate a single machine for building native mobile apps or outsourcing the process to a partner.

{{% alert color="warning" %}}
Building native mobile apps is not supported on the [Private Mendix Platform](/private-mendix-platform/).
{{% /alert %}}

## Getting the Make It Native App {#get-min-app}

The Make It Native app allows developers to preview, test, and debug native mobile apps in conjunction with Mendix Studio Pro. Use the **Make It Native 9 app** for Studio Pro 9.24.0 and above.

This app is available for both Android and iOS devices.

For information on which mobile operating systems are supported by the Make It Native app, see the [Mobile Operating Systems](/refguide9/system-requirements/#mobile) section of *System Requirements*.

### Direct Download Links {#direct-links}

For Make it Native 9 apps, download the following Android or iOS Make It Native apps directly using these QR codes:

|                                  Android                                  |                                iOS                                |
| :-----------------------------------------------------------------------: | :---------------------------------------------------------------: |
| {{< figure src="/attachments/refguide9/mobile/native-mobile/getting-the-make-it-native-app/android-min-qr-code.png" alt="Android QR Code" class="no-border" >}} | {{< figure src="/attachments/refguide9/mobile/native-mobile/getting-the-make-it-native-app/ios-min-qr-code.png" alt="iOS QR Code" class="no-border" >}} |
|   [Link](https://play.google.com/store/apps/details?id=com.mendix.developerapp.mx9&hl=en_US&gl=US)    |        [Link](https://apps.apple.com/us/app/make-it-native-9/id1542182000)         |

## Troubleshooting Common Mobile Issues {#troubleshooting}

Mendix strives to make building and running native mobile apps as simple as possible. But because some complexity is inherent in making apps, problems can come up. If you are having issues while building or running native mobile apps, please consult the sections below to see if your issue has already been solved.

### Make It Native App

To troubleshoot issues related to the Make it Native app, see the sections below.

#### Port Issues

Mendix recommends keeping the **Runtime port** in your [configuration](/refguide9/configuration/#server) on **8080**. If you change it, do not change it to **8083**, because that is designated for app packaging.

#### Wifi Network Settings

If you are using Windows, make sure your WiFi network is set to **Private**. Windows often sets WiFi to **Public** by default, which blocks incoming connections.

#### Error: Unable to Load Script {#unable-load-script}

Depending on your device settings and network characteristics, the Make it Native app can fail to connect to the runtime. If so, the Make it Native app can show the following error messages:

* **Unable to load script**:

    {{< figure src="/attachments/howto9/mobile/native-mobile/get-started/common-issues/unabletoloadscript.png" alt="unable to load script"   width="250"  class="no-border" >}}

* **Cannot detect your runtime**:

    {{< figure src="/attachments/howto9/mobile/native-mobile/get-started/common-issues/min-error-firewall.png" alt="cannot detect runtime"   width="250"  class="no-border" >}}

These failures are often caused by a firewall blocking your device from accessing your laptop. In such cases, attempts to open the runtime URL from a mobile browser will also fail. To mitigate these issues, please make sure your firewall allows incoming traffic to your laptop on the runtime and native packing ports (8080 and 8083 by default). Instructions on how to do this differ per firewall. Mendix recommends consulting your firewall administrator.

For the Windows Defender firewall, the most common firewall, do the following:

1. Make sure that your computer and the mobile device are connected to the same network.
1. Make sure that incoming connections are allowed by doing the following:<br />
    1. Open **Firewall & Network Protection** settings in Windows.<br />
    1. Go to **Advanced Settings**.<br />
    1. Select the **Inbound Rules** and scroll to the **Mendix Native Mobile** entries.<br />
    1. For each Node.js entry, note their values in the **Program** column. They should all have a green check mark in front of them.<br /> 
    1. If the **Program** column shows a Mendix installation directory, then there should be a green icon in front of the entry. If this is not the case, double-click the entry and select **Allow the connection**:

    {{< figure src="/attachments/howto9/mobile/native-mobile/get-started/common-issues/inboundrules.png" alt="inbound rules"   width="350"  class="no-border" >}}

1. Windows distinguishes between two types of networks: private and public. Windows Defender Firewall applies stricter regulations for public networks. If, and only if, you are connected to a trusted network, configure the network as **Private** on your computer.

#### Error: Unable to Detect Studio Pro

If your port forwarding settings are correct but you still get an error that the Make It Native app **cannot detect Studio Pro**, please reinstall the Make It Native app on your mobile device.

#### Strict Company Policies Prevent Your Connection

If your company has strict network policies which do not allow you to open the ports Mendix requires, here are 3 alternate approaches you can try:

* Connect your PC via Wifi to a personal hotspot on your mobile phone, then look up your PC's IP address and connect to `http://{YOUR PC'S IP ADDRESS}:8080` from the mobile phone
* Install the Make It Native app or a custom developer app on an Android emulator (for example [BlueStacks](https://www.bluestacks.com)), then connect to `http://localhost:8080`
* Tunnel the ports from your desktop to your Android device via USB by executing the following commands from your shell (requires [Android Studio](https://developer.android.com/studio)):

    ```
    adb reverse tcp:8080 tcp:8080
    adb reverse tcp:8083 tcp:8083
    ```

#### Use Make It Native 9 with an Older Version of Mendix 9 {#use-MIN-older}

The latest version of Make It Native 9 is only compatible with versions of Mendix 9.24.0 and above. To develop with older versions of Mendix 9, you can create a custom developer app by following [this guide](/refguide9/mobile/distributing-mobile-apps/building-native-apps/how-to-devapps/). Note that a custom developer app can be used to develop multiple older Mendix apps as long as no custom dependencies are introduced.

### Configure Parallels

To use Studio Pro on a Mac device, you will first need to install and configure Parallels. For more information, see [Configuring Parallels](/refguide9/using-mendix-studio-pro-on-a-mac/).
