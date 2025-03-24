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

In order to open a dockable pane you must first register the dockable pane handle with the api. to do this we will add a call to register the pane to the extension loaded method

```typescript
        const paneHandle = studioPro.ui.panes.register(
            { title: "MyDockablePane", initialPosition: "right" },
            { componentName: "component", uiEntrypoint: "dockablepane", queryParams: { key: "value" } }            
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
                { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" }
            ],
        });

        const paneHandle = await studioPro.ui.panes.register(
            { title: "MyDockablePane", initialPosition: "right" },
            { componentName: "component", uiEntrypoint: "dockablepane", queryParams: { key: "value" } }            
        );

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
we will add a new submenu to the existing add menu method on line 10.

```typescript
        await studioPro.ui.extensionsMenu.add({
            menuId: "myextension.MainMenu",
            caption: "MyExtension Menu",
            subMenus: [
                { menuId: "myextension.ShowTabMenuItem", caption: "Show tab" },
                { menuId: "myextension.ShowDockablePane", caption: "Show dockable pane"}
            ],
        });
```

We will also need to add a event listener to open the dockable pane once the menu has been selected. Lets add that to the end of the loaded method

```typescript
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
                if (args.menuId === "myextension.ShowDockablePane") {
                    studioPro.ui.panes.open(
                        {
                            title: "My Extension pane",
                        },
                        {
                            componentName: "extension/myextension",
                            uiEntrypoint: "dockablepane",
                        }
                    );
                }
            }
        );
```


```typescript
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
                if (args.menuId === "myextension.ShowDockablePane") {
                    studioPro.ui.panes.open(paneHandle.id);
                }
            }
        );
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
                { menuId: "myextension.ShowDockablePane", caption: "Show dockable pane"}
            ],
        });

        const paneHandle = studioPro.ui.panes.register(
            { title: "MyDockablePane", initialPosition: "right" },
            { componentName: "component", uiEntrypoint: "dockablepane", queryParams: { key: "value" } }            
        );

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

        // Open a pane when the menu item is clicked
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
                if (args.menuId === "myextension.ShowDockablePane") {
                    studioPro.ui.panes.open(paneHandle.id);
                }
            }
        );

    }
}
```

# Specifying a webview endpoint

Next we will create a new webview endpoint where we can define what user interface we would like to render within our pane.
To do this lets first rename some the existing tab endpoint to something for appropriate.

Lets rename ui/index.tsx to ui/tabindex.tsx

Now lets add the new endpoint file as ui/dockablepaneindex.tsx

The last thin we will need to do is modify our vite.config.ts to register the endpoint
to do this we will add a new entry called dockablepane on line 9. 

```typescript
    dockablepane: "src/ui/dockablepane.tsx"
```

Our file should now look like this

```typescript
import { defineConfig, ResolvedConfig, UserConfig } from "vite";

export default defineConfig({
    build: {
        lib: {
            formats: ["es"],
            entry: {
                main: "src/main/index.ts",
                tab: "src/ui/tabindex.tsx",
                dockablepane: "src/ui/dockablepane.tsx"
            }
        },
        rollupOptions: {
            external: ["@mendix/component-framework", "@mendix/model-access-sdk"]
        },
        outDir: "./dist/myextension"
    }
} satisfies UserConfig);
```
# Closing the dockable pane

Now that we can register a pane and open it would also be a good idea to close it.
To close your pane we will again add a menu item that closes the pane. As before lets first
add a new sub menu item to the menu on line 11. 

```typescript
    { menuId: "myextension.HideDockablePane", caption: "Hide dockable pane"}
```

Lets also add another event handler for the new menu at the end of the loaded method:

```typescript
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
                if (args.menuId === "myextension.HideDockablePane") {
                    studioPro.ui.panes.close(paneHandle.id);
                }
            }
        ); 
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
                { menuId: "myextension.ShowDockablePane", caption: "Show dockable pane"}
                { menuId: "myextension.HideDockablePane", caption: "Hide dockable pane"}
            ],
        });

        const paneHandle = studioPro.ui.panes.register(
            { title: "MyDockablePane", initialPosition: "right" },
            { componentName: "component", uiEntrypoint: "dockablepane", queryParams: { key: "value" } }            
        );

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

        // Open a pane when the menu item is clicked
        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
                if (args.menuId === "myextension.ShowDockablePane") {
                    studioPro.ui.panes.open(paneHandle.id);
                }
            }
        );

        studioPro.ui.extensionsMenu.addEventListener(
            "menuItemActivated",
            (args) => {
                if (args.menuId === "myextension.HideDockablePane") {
                    studioPro.ui.panes.close(paneHandle.id);
                }
            }
        );        

    }
```

# Conclusion

You should now have a new dockable pane with its own user interface which you can modify as you like.
You can also open and close the dockable pane from a menu.

