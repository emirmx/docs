---
title: "Get Started with the Web Extensibility API"
linktitle: "Get Started"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/
weight: 2
---

## Introduction

Studio Pro extensions can be developed using TypeScript and use standard web development technologies to extend the Studio Pro development environment. This guide shows you how to set up a basic development environment for building an extension using the web extensibility API.

For more information, see the [Mendix Studio Pro Web Extensibility API](http://apidocs.rnd.mendix.com/11/extensions-api/index.html).

### Prerequisites

You will need the following prerequisites:

* Mendix Studio Pro version 11.2.0 or higher [Mendix Studio Pro](https://marketplace.mendix.com/link/studiopro). 
* A development IDE to develop your extensions. We recommend using [Visual Studio Code](https://code.visualstudio.com/).
* Install the latest version 22.x.x of Node: https://nodejs.org/en/download.

{{% alert color="info" %}}
Extensions can be built on any operating system as the underlying framework is cross-platform.
{{% /alert %}}

{{% alert color="info" %}}
Please note that extension development is only possible by starting Studio Pro with the `--enable-extension-development` feature flag.
{{% /alert %}}

## Creating Your First Extension

This section will show you how to build and test an extension.

### Create a Test App

1. Create a new app using the **Blank Web App** template.
1. Make sure that you take note of the application directory (more precisely the application .mpr file)
   where the application is stored on disk. 
   {{% alert color="info" %}}
   You can always open the application directory by choosing
   **App** -> **Show App Directory in Explorer** (or **Show App Directory in Finder**) in Studio Pro.
   {{% /alert %}}

### Creating the Extension

To accelerate your extension development, we provide an extension generator that creates a customizable sample extension.

To use the generator, navigate to your desired source code directory and run the command `npm create @mendix/extension`. You may be prompted by `npm` to grant permission to install the generator. After installation, you will be guided through a series of questions to help configure your extension.

You will be asked to select the programming language (TypeScript is used in our tutorials), specify the extension name, and choose whether to use React for the extension’s UI. The next two questions, while optional, are highly recommended, as they enable direct debugging and deployment from Visual Studio Code. These questions request the path to the Studio Pro executable and the application `.mpr` package. The final question allows you to select the Studio Pro version; please ensure you choose version 11.

{{% alert color="info" %}}
The Studio Pro executable is typically located at `C:\Program Files\Mendix\<version>\modeler\studiopro.exe`. To find the exact path, launch Studio Pro, right-click its taskbar icon, then right-click `Mendix Studio Pro 11.2.0` (your version may differ) and select **Properties**. The 'Target' field displays the executable path.
{{% /alert %}}

Once you have completed the setup, a new directory named after your extension will be created,
containing the source code of the extension.

### Exploring the Created Extension

In the following, we assume the name of your extension is `myextension`:

1. From the Explorer window navigate to `src/main/index.ts` select it to open the file.

    Reading through the source code you should see the following:

1. Line 8 adds a menu

    ```typescript
    await studioPro.ui.extensionsMenu.add({
        menuId: "myextension.MainMenu",
        caption: "MyExtension Menu",
        subMenus: [
            { menuId: "myextension.ShowMenu", caption: "Show tab" },
        ],
    });
    ```

1. Line 17 opens a tab

    ```typescript
    // Open a tab when the menu item is clicked
    studioPro.ui.extensionsMenu.addEventListener(
        "menuItemActivated",
        (args) => {
            if (args.menuId === "myextension.ShowMenu") {
                studioPro.ui.tabs.open(
                    {
                        title: "MyExtension Tab"
                    },
                    {
                        componentName: "extension/myextension",
                        uiEntrypoint: "tab",
                    }
                );
            }
        }
    );
    ```

  1. If you navigate to `build-extension.mjs` you can choose the directory to which the extension will be installed to
     after being built in line 6:
     ```typescript
     const appDir = "C:\\TestApps\\AppTestExtensions"
     ```

  1. The file `.vscode\launch.json` specifies the launch configuration and enables debugging. Lines 8-9 specify
     how the Studio Pro will be run:
     ```json
     "runtimeExecutable": "C:\\Users\\petar.vukmirovic\\source\\repos\\appdev\\modeler\\Mendix.StudioPro.Windows\\bin\\Debug\\studiopro.exe",
     "runtimeArgs": ["C:\\TestApps\\AppTestExtensions\\AppTestExtensions.mpr", "--enable-extension-development", "--enable-web-extensions"],
     ```

When you install the extension you will see a new menu item within Studio Pro.

### Building and Debugging the Extension

From within Visual Studio Code:

1. Select **File** -> **Open Folder**
1. Navigate to the folder you just extracted your extension source code to.
1. Click **Select Folder**.
1. Select **Yes** if you are asked whether you trust this folder.
1. Now open a Terminal by selecting **Terminal** -> **New Terminal** from the top menu.
1. From the Terminal type `npm install`. This installs all dependencies for the extension
1. Build your extension using the command `npm run build` in the terminal.
   If you provided the path to `.mpr` file in the previous step, this will install the extension into
   the application directory.

If the last two questions of the extension generator were answered, and you have built and installed the extension, you can debug it as follows:

1. Open the extension source code in Visual Studio Code and set breakpoints.
2. Select 'Run and Debug' side panel.
3. Click on the play button on the top of the panel (or press F5)

This will run Studio Pro in the extension development mode and open the configured application. You will see a new `Extensions` item in the top menu.

## Conclusion

Using this guide we have:

* Created a new app
* Used extension generator to get started with extension developmnt
* Built the extension and installed it in our app
* Tested and debugged our extension from within Visual Studio Code.

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
