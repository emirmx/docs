---
title: "Dockable Pane Api"
url: /apidocs-mxsdk/apidocs/extensibility-api/web/dockable-pane-api/
weight: 4
---

# Introduction

This guide will show you how to create and manage a dockable pane within the web extensions api. a Dockable pane allows you to create a webview that can be docked and moved within the Studio Pro user interface. Other examples of dockable panes are:

- The marketplace
- The errors pane
- The stories pane
- The toolbox

# Prerequisites

This guide builds ontop of the [getting started guide](/apidocs-mxsdk/apidocs/extensibility-api/web/getting-started/). Please complete that guide before starting this one.

# Creating a dockable pane.

In order to open a dockable pane you must first register the dockable pane handle with the api. To do this we will add a call to register the pane to the extension loaded method in the `src/main/index.ts`.

```typescript
const paneHandle = await studioPro.ui.panes.register(
  {
    title: "My Extension Pane",
    initialPosition: "right",
  },
  {
    componentName: "extension/myextension",
    uiEntrypoint: "dockablepane",
  }
);
```

Whenever you need to interact with the dockable pane you will need the paneHandle that we just registered.

After adding this call you should now have a loaded method that looks like this:

```typescript
    async loaded() {
        // Add a menu item to the Extensions menu
        await studioPro.ui.extensionsMenu.add({
            menuId: "myextension.MainMenu",
            caption: "MyExtension Menu",
            subMenus: [
                { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" },
            ],
        });

        const paneHandle = await studioPro.ui.panes.register(
            {
                title: "My Extension Pane",
                initialPosition: "right",
            },
            {
                componentName: "extension/myextension",
                uiEntrypoint: "dockablepane",
            });

        // Open a tab when the menu item is clicked
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
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
            }
        );
    }
```

# Adding a Menu and then opening the dockable pane

Next we will add a menu that will open the pane when we select it.
We will add a new submenu to the existing add menu method on line 10.

```typescript
// Add a menu item to the Extensions menu
await studioPro.ui.extensionsMenu.add({
  menuId: "myextension.MainMenu",
  caption: "MyExtension Menu",
  subMenus: [
    { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" },
    { menuId: "myextension.ShowDockMenuItem", caption: "Show dock pane" },
  ],
});
```

We will also need to alter the addEventListener call to also handle opening the dockable pane once the menu has been selected. Lets add that to the end of the loaded method

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
  } else if (args.menuId === "myextension.ShowDockMenuItem") {
    studioPro.ui.panes.open(paneHandle);
  }
});
```

Your loaded method should now look like this:

```typescript
    async loaded() {
        // Add a menu item to the Extensions menu
        await studioPro.ui.extensionsMenu.add({
            menuId: "myextension.MainMenu",
            caption: "MyExtension Menu",
            subMenus: [
                { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" },
                { menuId: "myextension.ShowDockMenuItem", caption: "Show dock pane" },
            ],
        });

        const paneHandle = await studioPro.ui.panes.register(
            {
                title: "My Extension Pane",
                initialPosition: "right",
            },
            {
                componentName: "extension/myextension",
                uiEntrypoint: "dockablepane",
            });

        // Open a tab when the menu item is clicked
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
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
                else if (args.menuId === "myextension.ShowDockMenuItem") {
                    studioPro.ui.panes.open(paneHandle);
                }
            }
        );
    }
```

# Specifying a webview endpoint

## Adding new endpoint handlers

Next we will create a new webview endpoint where we can define what user interface we would like to render within our pane.
To do this let's first rename some the existing tab endpoint to something for appropriate.

- Lets rename `ui/index.tsx` to `ui/tab.tsx`
- Now lets add the new endpoint file as `ui/dockablepane.tsx` and for the content use the `ui/tab.tsx` as a guide.

We will also need to alter the `vite.config.ts` and `manifest.json` files to bind to the correct endpoint

## Altering vite.config.js

From your `vite.config.js` lets replace the entry section with the following:

```typescript
            entry: {
                main: "src/main/index.ts",
                tab: "src/ui/tab.tsx",
                dockablepane: "src/ui/dockablepane.tsx"
            }
```

this instructs vite that we have the tab endpoint connected to `src/ui/tab.tsx` and the dockable pane endpoint connected to `src/ui/dockablepane.tsx`

The `vite.config.js` file should now look like this

```typescript
import { defineConfig, ResolvedConfig, UserConfig } from "vite";

export default defineConfig({
  build: {
    lib: {
      formats: ["es"],
      entry: {
        main: "src/main/index.ts",
        tab: "src/ui/tab.tsx",
        dockablepane: "src/ui/dockablepane.tsx",
      },
    },
    rollupOptions: {
      external: ["@mendix/component-framework", "@mendix/model-access-sdk"],
    },
    outDir: "./dist/myextension",
  },
} satisfies UserConfig);
```

# Altering public/manifest.json

We will also need to instruct Studio Pro to load the endpoint that we just created. To do this we will need to modify the `manifest.json` file which is located in `public/manifest.json`

We will alter the ui section by changing the `tab` endpoint and adding the `dockablepane` endpoint.

```typescript
      "ui": {
        "tab": "tab.js",
        "dockablepane": "dockablepane.js"
      }
```

Our `manifest.json file` should now look like this:

```typescript
{
  "mendixComponent": {
    "entryPoints": {
      "main": "main.js",
      "ui": {
        "tab": "tab.js",
        "dockablepane": "dockablepane.js"
      }
    }
  }
}
```

# Closing the dockable pane

Now that we can register a pane and open it would also be a good idea to close it.
To close your pane we will again add a menu item that closes the pane. As before let's first
add a new sub menu item to the menu on line 11.

```typescript
                { menuId: "myextension.HideDockMenuItem", caption: "Hide dock pane" },
```

We will also need to alter the event handler for the new menu at the end of the loaded method:

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
  } else if (args.menuId === "myextension.ShowDockMenuItem") {
    studioPro.ui.panes.open(paneHandle);
  } else if (args.menuId === "myextension.HideDockMenuItem") {
    studioPro.ui.panes.close(paneHandle);
  }
});
```

Our loaded method should now look like this:

```typescript
    async loaded() {
        // Add a menu item to the Extensions menu
        await studioPro.ui.extensionsMenu.add({
            menuId: "myextension.MainMenu",
            caption: "MyExtension Menu",
            subMenus: [
                { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" },
                { menuId: "myextension.ShowDockMenuItem", caption: "Show dock pane" },
                { menuId: "myextension.HideDockMenuItem", caption: "Hide dock pane" },
            ],
        });

        const paneHandle = await studioPro.ui.panes.register(
            {
                title: "My Extension Pane",
                initialPosition: "right",
            },
            {
                componentName: "extension/myextension",
                uiEntrypoint: "dockablepane",
            });

        // Open a tab when the menu item is clicked
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
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
                else if (args.menuId === "myextension.ShowDockMenuItem") {
                    studioPro.ui.panes.open(paneHandle);
                }
                else if (args.menuId === "myextension.HideDockMenuItem") {
                    studioPro.ui.panes.close(paneHandle);
                }
            }
        );
    }
```

# Conclusion

You should now have a new dockable pane with its own user interface which you can modify as you like.
You can also open and close the dockable pane from a menu.

# Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
