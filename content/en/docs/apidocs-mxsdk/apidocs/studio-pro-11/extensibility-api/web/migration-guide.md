---
title: "Migration Guide for Web Extensibility API Older Versions"
linktitle: "Migration Guide"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/migration-guide/
weight: 2
---

## Introduction

A breaking change was introduced in version 11.6 of the Extensibility API which changed the way [menus](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu/) are created. Here we will explain how to fix your extension code if you have upgraded to version 11.6 from an older version.

## MenuItemActivated Event
If your extension created menus using the `menuId` and the `menuItemActivated` event in order to trigger actions, you can now simply use the action that was called when the event was triggered as the actual `action` property of your menu.

So if your code looked like this:
```typescript

const menuId = "myextension.menu";

await studioPro.ui.extensionsMenu.add({
            menuId: menuId,
            caption: "My Menu",
        });

studioPro.ui.extensionsMenu.addEventListener(
    "menuItemActivated",
    async (args) => {
        if (args.menuId === menuId) {
            await studioPro.ui.messageBoxes.show("info", "My Menu was Clicked!")
        }
    }
);
```

The action called when the event triggers for your menu can now be used directly as the menu action:

```typescript
await studioPro.ui.extensionsMenu.add({
        menuId: "myextension.menu",
        caption: "My Menu",
        action: async () => await studioPro.ui.messageBoxes.show("info", "My Menu was Clicked!")
    });
```

The `menuItemActivated` event no longer exist so you cannot listen to it anymore.

## Registering Commands
If your extension created menu by using a command id of a pre-registered command, the action that is sent to the command registration api when registering the command can now be used directly as the action of the menu.

So if your code looked like this:
```typescript
const commandId = "myextension.menu-command";

await studioPro.app.commands.registerCommand(
    commandId,
    async () => await studioPro.ui.messageBoxes.show("info", "My Menu was Clicked!")
);
await studioPro.ui.extensionsMenu.add({ caption: "My Menu", menuId: "myextension.menu", commandId });
```

The same action sent to the command registration api can now instead become the `action` property value on the menu:

```typescript
await studioPro.ui.extensionsMenu.add({
        menuId: "myextension.menu",
        caption: "My Menu",
        action: async () => await studioPro.ui.messageBoxes.show("info", "My Menu was Clicked!")
    });
```

The command registration API has also been removed and it is no longer available.

# Action Arguments
Action arguments are also possible in the new Menu API. Please read our [menus documentation](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu/) for an in-depth explanation.