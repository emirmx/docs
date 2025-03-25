---
title: "Tab Api"
url: /apidocs-mxsdk/apidocs/extensibility-api/web/tab-api/
weight: 8
---

# Prerequisites

This guide builds ontop of the [getting started guide](/apidocs-mxsdk/apidocs/extensibility-api/web/getting-started/). Please complete that guide before starting this one. You should also be familiar with creating menus.

# Opening a tab

In this example we'll learn how to open a tab in Studio Pro from an extension. This tab will contain your web content.<br />

Inside the `loaded` event in `Main`, we will create a menu to open this tab. This will create a menu, place it under `Extensions` in Studio Pro, and once clicked it will open your tab.<br />
First we need to import the `menuApi` from the mendix `extensibility-api` package.<br />
Inside the `menuItemActivated` event, we will call the tabs api in order to open our tab.<br />
The class `Main` should now look like below.

```typescript
import { IComponent, studioPro, TabHandle } from "@mendix/extensions-api";

class Main implements IComponent {
  tabs: { [menuId: string]: Promise<TabHandle> } = {};
  async loaded() {
    // Add menu items to the Extensions menu to open and close our tab
    await studioPro.ui.extensionsMenu.add({
      menuId: "myextension.MainMenu",
      caption: "MyExtension Menu",
      subMenus: [
        { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" },
        {
          menuId: "myextension.CloseTabMenuItem",
          caption: "Close tab",
        },
      ],
    });

    studioPro.ui.extensionsMenu.addEventListener(
      "menuItemActivated",
      async (args) => {
        // Open a tab when the menu item is clicked
        if (args.menuId === "myextension.ShowTabMenuItem") {
          const handle = studioPro.ui.tabs.open(
            {
              title: "My Extension Tab",
            },
            {
              componentName: "extension/myextension",
              uiEntrypoint: "tab",
            }
          );

          // Track the open tab
          this.tabs["myextension.MainMenu"] = handle;
        }

        // Close the tab opened previously
        if (args.menuId === "myextension.CloseTabMenuItem") {
          studioPro.ui.tabs.close(await this.tabs["myextension.MainMenu"]);
        }
      }
    );
  }
}

export const component: IComponent = new Main();
```

It is important that whenever the tabs api `open` method is called, the `TabHandle` returned is tracked by the extension, so that it can be closed later by calling the `close` method. In this example, we have a dictionary that uses the parent menu id as the key in order to track the open `TabHandle`.

## TabInfo and UISpec parameters

The parameters passed to the `open` method are a `TabInfo` object and a `UISpec` object.

### TabInfo

The `TabInfo` object requires the `title` of the tab, which will be shown in the title bar of your tab in Studio Pro.

### UISpec

The `UISpec` has two required properties, the `componentName` and the `uiEntryPoint`. The `componentName` has to match the name of the extension, but also must be prefixed with "extension/". So the `componentName` will look like "extension/myextension" in your example shown above.
The `uiEntryPoint` property must match the name mapped from the `manifest.json` file. Keep reading below for samples with multiple tabs to see a clearer example.

# Content of the tabs

You might have noticed the `uiEntryPoint` value "tab" in the `UISpec` object above. This value must match the one from the manifest. If you wanted to have more than one tab in your extension, you need to structure the folders in a particular way. You also need to set up the manifest file in the correct way.

In the sample code, add a new method `createTabSpec` in your `Main` class. This will allow us to have multiple tabs for the same extension.

```typescript
createTabSpec(tab: string, title: string): { info: TabInfo, ui: UISpec} {
        const info: TabInfo = { title };
        const ui: UISpec = {
            componentName: "extension/myextension",
            uiEntrypoint: tab,
        };

        return {info, ui};
    }
```

Inside the `ui` folder, add 3 separate folders, one for each tab you want to display contents for. Each subfolder for each tab should contain its own index file. In this example, we'll add 3 tabs.

![ui Folder Structure](/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/tabs/ui_folder_structure.png)

All our index files for this example look the same except for the header tag identifying our tab. Below is the index file for tab 3.

```typescript
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <h1>tab3</h1>
  </StrictMode>
);
```

Our `Main` class should now look like below. It will contain 3 submenus for each tab we want to open.

```typescript
import { IComponent, studioPro, TabInfo, UISpec } from "@mendix/extensions-api";

class Main implements IComponent {
  async loaded() {
    // Add a menu item to the Extensions menu
    await studioPro.ui.extensionsMenu.add({
      menuId: "myextension.MainMenu",
      caption: "Show Tabs",
      subMenus: [
        { menuId: "myextension.ShowTab1", caption: "Show tab 1" },
        { menuId: "myextension.ShowTab2", caption: "Show tab 2" },
        { menuId: "myextension.ShowTab3", caption: "Show tab 3" },
      ],
    });

    // Open a tab when the menu item is clicked
    studioPro.ui.extensionsMenu.addEventListener(
      "menuItemActivated",
      async (args) => {
        if (args.menuId === "myextension.ShowTab1") {
          const tab1Spec = this.createTabSpec("tab1", "Tab 1 Title");
          studioPro.ui.tabs.open(tab1Spec.info, tab1Spec.ui);
        }
        if (args.menuId === "myextension.ShowTab2") {
          const tab2Spec = this.createTabSpec("tab2", "Tab 2 Title");
          studioPro.ui.tabs.open(tab2Spec.info, tab2Spec.ui);
        }
        if (args.menuId === "myextension.ShowTab3") {
          const tab3Spec = this.createTabSpec("tab3", "Tab 3 Title");
          studioPro.ui.tabs.open(tab3Spec.info, tab3Spec.ui);
        }
      }
    );
  }

  createTabSpec(tab: string, title: string): { info: TabInfo; ui: UISpec } {
    const info: TabInfo = { title };
    const ui: UISpec = {
      componentName: "extension/myextension",
      uiEntrypoint: tab,
    };

    return { info, ui };
  }
}

export const component: IComponent = new Main();
```

Our `manifest.json` file will now look like below. Notice each of the tabs under the `ui` entry.

```json
{
  "mendixComponent": {
    "entryPoints": {
      "main": "main.js",
      "ui": {
        "tab1": "tab1.js",
        "tab2": "tab2.js",
        "tab3": "tab3.js"
      }
    }
  }
}
```

And our `vite.config` file looks like below. Notice each entry for each tab.

```typescript
import { defineConfig, ResolvedConfig, UserConfig } from "vite";

export default defineConfig({
  build: {
    lib: {
      formats: ["es"],
      entry: {
        main: "src/main/index.ts",
        tab1: "src/ui/tab1/index.tsx",
        tab2: "src/ui/tab2/index.tsx",
        tab3: "src/ui/tab3/index.tsx",
      },
    },
    rollupOptions: {
      external: ["@mendix/component-framework", "@mendix/model-access-sdk"],
    },
    outDir: "./dist/myextension",
  },
} satisfies UserConfig);
```

After building and installing the extension in our Studio Pro app, each tab will display the content they specify in their own individual index files.

# Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
