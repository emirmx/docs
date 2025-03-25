---
title: "Get Started with the Web Extensibility API"
linktitle: "Get Started"
url: /apidocs-mxsdk/apidocs/extensibility-api/web/getting-started/
weight: 2
---

## Introduction

Welcome to web extensibility api. This guide will help you setup a basic development environment for building an extension.
Extensions are developed using typescript and use standard web development technologies to extend the Studio Pro Development environment. To get started you will need to install some prerequisites.

### Prerequisites

Mendix Studio Pro version 10.21.0 or higher [Mendix Studio Pro](https://marketplace.mendix.com/link/studiopro). <br />
Install the latest Studio Pro version from the Mendix [Marketplace](https://marketplace.mendix.com/link/studiopro/).<br />
We recommend using Visual Studio Code to develop your extensions. [VS Code](https://code.visualstudio.com/).<br />
Install the latest version 22.x.x of Node: https://nodejs.org/en/download.<br />

{{% alert color="info" %}}
Make sure to keep this Studio Pro installation up-to-date to benefit from new features and fixes.
{{% /alert %}}

{{% alert color="info" %}}
Extensions can be built on any operating system as the underlying framework is cross-platform.
{{% /alert %}}

### Create a Test App

We recommend you create a Test app in mendix where you will develop your extension. For this example we will create an app using the blank app template in Studio Pro.

From the App Selector click `Create New App` or alternatively from inside Studio Pro select `File -> New App` from the menu. <br />
From the template selection screen select blank app. Once asked to confirm select use template.

### Create a development extension

The simplest way to get started is to use our [Web extension template](https://github.com/mendix/web-extension-template)<br />
Download or install via Git.

Once installed or extracted to a folder of your choosing you now have a starter extension that you can use as a starting point.

### Building your extension

From within Visual Studio Code:

- Select File -> Open Folder
- Navigate to the folder you just extracted your extension source code in.
- Click Select Folder
- You might get prompted that you trust this folder. Select Yes that you do trust it.
- Now open a Terminal by selecting Terminal -> New Terminal from the top menu.
- From the Terminal type `npm install`. This will install all dependancies for the extension
- Once completed you should now be able to build your extension. From the terminal type `npm run build`.

Once completed you should now have a build artifact which we can deploy to your mendix app.
Before we start with that step though it might be worth while to explore our extension a bit more to understand what it will do
when it is installed.

- From the Explorer window navigate to `src/main/index.ts` select it to open the file.

Reading through the source code you should see the following:

- We are adding a menu on line 7

```typescript
await studioPro.ui.extensionsMenu.add({
  menuId: "myextension.MainMenu",
  caption: "MyExtension Menu",
  subMenus: [{ menuId: "myextension.ShowTabMenuItem", caption: "Show tab" }],
});
```

- We are opening a tab on line 14

```typescript
// Open a tab when the menu item is clicked
studioPro.ui.extensionsMenu.addEventListener("menuItemActivated", (args) => {
  if (args.menuId === "myextension.ShowTabMenuItem") {
    studioPro.ui.tabs.open(
      {
        title: "My Extension Tab",
      },
      {
        componentName: "extension/myextension",
        uiEntrypoint: "tab",
      }
    );
  }
});
```

This means that when we install our extension we expect to see a new menu item within Studio Pro.

### Testing your extension

From within your file explorer.

- Navigate to the folder where we extracted your extension source code into.
- Open the dist folder
- Copy the myextension folder
- Navigate to the folder where you created your app in step 1.
- Create a new folder. Call it `webextensions`.
- Paste the myextension folder into the webextensions folder you just created.

The App now has the needed files to load the extension however because this extension is being executed from a folder we will need to start Studio Pro with some special command line parameters.

Run Studio Pro withe following command line parameters `--enable-extension-development --enable-webview-debugging`

These flags will instruct Studio Pro to do the following:

- Load extensions from the webextensions folder
- Enable web debugging tools which will be useful when developing your extension

Once Studio Pro has loaded open the app that we created in Step 1. If you followed the Guide correctly you should now see a new `Extensions` item in the Top Menu.

## Conclusion

Using this guide we have:

- Created a New App where we will develop an extension
- Created a new extension using the starter sample [Web extension template](https://github.com/mendix/web-extension-template)
- Built the extension and installed it within our App
- Tested our extension from within Studio Pro.

# Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
