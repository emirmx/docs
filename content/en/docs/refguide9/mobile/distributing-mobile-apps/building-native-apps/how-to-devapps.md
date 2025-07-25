---
title: "Creating a Custom Developer App"
url: /refguide9/mobile/distributing-mobile-apps/building-native-apps/how-to-devapps/
weight: 40
description: A tutorial for creating custom developer apps.
aliases:
    - /howto9/mobile/how-to-devapps/
---

## Introduction

As your Mendix app matures, you may want to expand its functionality (such as by introducing custom widgets or logic that will require new native dependencies). One such customization could be adding a near-field communication (NFC) module to your app. While the Make It Native app suffices for testing basic apps, as your app adds custom dependencies—like custom native widgets or fonts—you will need a more tailored developer app.

A custom developer app helps you by serving as a replacement for the Make It Native app, and should be used when you have custom widgets and logic which are not supported by the Make It Native app. Custom developer apps are apps you can generate yourself using your current app structure, your custom modules, and any other requirements to test your evolving app. Custom developer apps feature the same functionality as the Make It Native app but are tailored to your needs.

## Prerequisites

* Complete [Get Started with Native Mobile](/refguide9/mobile/getting-started-with-mobile/)
* Complete the Mendix Native Mobile Builder wizard as found in [Build a Mendix Native App Locally](/refguide9/mobile/distributing-mobile-apps/building-native-apps/native-build-locally-manually/)

## Building Your Developer App {#build-your-developer-app}

1. Run Mendix Native Mobile Builder from your app: 

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/deploying-native-app/start-nbui.png" alt="Start Mendix Native Mobiler Builder"   width="350"  class="no-border" >}}

1. When Mendix Native Mobile launches you are greeted with the home screen:

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/deploying-native-app/home-screen.png" alt="Mendix Natve Mobile Builder Home Screen"   width="350"  class="no-border" >}} 

1. Choose *Build app for local development*

1. Given you already went through the initial wizard at least once, you should be greeted with the configuration screen for *Building an app for local development*: 

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/how-to-devapps/build-custom-dev-app.png" alt="Mendix Natve Mobile Builder Home Screen"   width="350"  class="no-border" >}} 

1. Click the *Build developer app* button

1. The tool will set up your GitHub repository commit your changes, one for iOS and one for Android and continue with building the apps.

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/deploying-native-app/build-release-app-build-step1.png" alt="Building"   width="350"  class="no-border" >}}{{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/deploying-native-app/build-release-app-build-step1.png" alt="Building"   width="350"  class="no-border" >}}
    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/deploying-native-app/build-release-app-build-step2.png" alt="Build release app" width="350" class="no-border" >}}

1. When the build completes, you can scan the QR code provided to install the app to your device. Currently the QR code service is only supported for Android devices.

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/deploying-native-app/build-release-app-build-done-both.png" alt="Build release app"   width="350"  class="no-border" >}}

## Installing Your Custom Developer App manually

### Android

For Android the output of the build is an *APK* file. *APK* files can be directly installed on devices or emulators.

#### Installing on an Emulator

With your emulator running, install your app in your emulator by doing the following:

1. Drag the *APK* onto the emulator's window.
2. Wait for the installation to be done.
3. Open the app from the launcher.

#### Installing on a Device

There are various ways install an app on a device. Installing using a USB is detailed below, but you can use a different method if it suits you. Do the following to install your *APK* onto a device:

1. Connect your device to your machine via USB.
2. Enable file transfer on your device (differs per device).
3. Open **This PC** in File Explorer; your device should be listed as an external device.
4. Drag your *APK* onto your device.
5. Wait for it to finish transferring.
6. Open your device's file manager.
7. Navigate to the root of the file system.
8. Tap the *APK* to install.
9. Go through the installation steps.
10. Open the app from the launcher.

### iOS

By default your custom developer app will be unsigned. To get a signed *IPA*, follow the steps in [Distributing Native Apps](/refguide9/mobile/distributing-mobile-apps/distributing-native-apps/). Your custom developer app branch is named **developer**.

The unsigned output of an iOS build is an *XCArchive* file. *XCArchive* files require manual signing before they are ready to be installed on a device.

The signed output of iOS build is an *IPA* file. If correctly signed, *IPA* files can be installed on physical devices.

#### Installing on an Emulator

Before installing, make sure you have completed the following prerequisites:

* Have a Mac OSX machine
* Install LTS builds of Node.js and NPM (download [here](https://nodejs.org/en/))
* Install Cocoapods ([installation instructions](https://cocoapods.org/#install))
* Install the latest Xcode version

Builds with the Mendix Native Mobile Builder are stripped of simulator artifacts. Therefore, to run on Xcode's Simulator you will have to build the developer branch locally from source by completing these steps:

1. Navigate to your GitHub repo.
2. Switch to your **developer** branch:

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/how-to-devapps/github-branch-switching.png" alt="Switch branch on GitHub"   width="350"  class="no-border" >}}
   
3. Click **Clone or Download** and then click **Download ZIP**:

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/how-to-devapps/github-download-branch.png" alt="Download repository"   width="350"  class="no-border" >}}

4. Unzip the downloaded archive.
5. Open a terminal and change directory into the folder.
6. Run this command:

    ```shell
    npm i && cd ios && pod install
    ```

    This will install the node module dependencies and the iOS Dependencies
7. In the **ios** folder, open the **NativeTemplate.xcworkspace** file:

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/how-to-devapps/ios-folder.png" alt="iOS folder structure" class="no-border" >}}

8. In Xcode select the **Dev** target and the emulator you want to build your developer app for:

    {{< figure src="/attachments/howto9/mobile/native-mobile/distribution/build-native-apps/how-to-devapps/xcode-target-selection.png" alt="Dev target selection" class="no-border" >}}

9. Click **Play**.

#### Distributing the Custom Developer App to the Apple App Store

To run your custom developer app on a device which is not registered as a test device on the Apple Developer Portal, you will have to sign the developer app with your certificates manually and distribute it via TestFlight.

Read more on TestFlight in the [official documentation](https://testflight.apple.com/).
