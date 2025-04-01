---
title: "Create a Menu Using Web API"
linktitle: "Create Menu"
url: /apidocs-mxsdk/apidocs/web-extensibility-api/menu-api/
weight: 30
---

## Introduction

This guide will show you how to create a simple menu and submenus with the web extension api.

## Prerequisites

This guide builds ontop of the [getting started guide](/apidocs-mxsdk/apidocs/web-extensibility-api/getting-started/). Please complete that guide before starting this one.

## Creating a menu

In this example we'll learn how to add a menu to your extension.<br />
Replace your `src/main/index.ts` file with the following:

```typescript
import { IComponent, Menu, studioPro } from "@mendix/extensions-api";

const menuApi = studioPro.ui.extensionsMenu;

const messageBoxApi = studioPro.ui.messageBoxes;
const menuId = "my-menu-unique-id";
const caption = "My First Menu";

// Open a message box when the menu item is clicked
studioPro.ui.extensionsMenu.addEventListener("menuItemActivated", (args) => {
  if (args.menuId === "my-menu-unique-id") {
    messageBoxApi.show("info", `My menu '${args.menuId}' was clicked`);
  }
});
class Main implements IComponent {
  async loaded() {
    const menu: Menu = {
      caption: caption,
      menuId: menuId,
      subMenus: [],
      hasSeparatorBefore: false,
      hasSeparatorAfter: true,
      enabled: true,
    };

    await menuApi.add(menu);
  }
}

export const component: IComponent = new Main();
```

In the above `index.ts` file, a menu is created and once clicked it will show a dialog.<br />
We need to import the `menuApi` from the mendix `extension-api` package, and also the `messageBoxApi` to show our dialog.<br />
We also need to start listening to the `menuItemActivated` which will notify our extension that our menu was clicked. This is where we can handle the actual function of the menu. In this example below, it calls the `messageBoxApi` to show an information dialog.<br />
An important detail to notice is that the menuId in the if statement matches the menuId that was used when creating the menu item. In this example it is `"my-menu-unique-id"`.<br />

Your extensions should now appear like this:

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/menus/my_first_menu.png" >}}

## Menu Properties

For the menu properties it is not necessary to set `enabled` to true as it will be enabled by default. It is also not necessary to set `hasSeparatorBefore` and `hasSeparatorAfter`, as they will be set to false by default. They are used to add a visual separator between single or groups of menus.

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/menus/grouped_menus.png" >}}

The arguments `args` in the `menuItemActivated` event represents the data returned by Studio Pro to the extension, notifying it that the menu id contained in the arguments has been activated (clicked). So it is up to the extension developer to keep track of each menu and their id so that they can perform an action when Studio Pro lets them know it has been clicked.

## Creating a menu with submenus

You can also have a number of submenus that branch out your menu. To do so, add another menu (or more) to the `subMenus` array in your menu. These child menus can in turn have their own submenus, and so on. Only parent menus (menus that are not sub menus to any others) should be added through the `menuApi`, as shown in the code sample below. Also keep in mind that the `menuItemActivated` only gets sent when a leaf menu (a menu that does not have any submenus) gets clicked.<br />

Replace your `src/main/index.ts` file with the following:

```typescript
import { IComponent, Menu, studioPro } from "@mendix/extensions-api";

const menuApi = studioPro.ui.extensionsMenu;
const messageBoxApi = studioPro.ui.messageBoxes;

// Open a message box when the menu item is clicked
studioPro.ui.extensionsMenu.addEventListener("menuItemActivated", (args) => {
  messageBoxApi.show("info", `Child menu '${args.menuId}' was clicked`);
});
class Main implements IComponent {
  async loaded() {
    const grandChild: Menu = {
      caption: "Grandchild Menu",
      menuId: "grandChild",
    };

    const childMenu1: Menu = {
      caption: "Child Menu 1",
      menuId: "child_1",
      subMenus: [grandChild],
    };

    const childMenu2: Menu = {
      caption: "Child Menu 2",
      menuId: "child_2",
    };

    const menu1: Menu = {
      caption: "Menu 1",
      menuId: "menu1",
      subMenus: [childMenu1, childMenu2],
    };

    const menu2: Menu = {
      caption: "Menu 2",
      menuId: "menu2",
      subMenus: [],
    };

    await menuApi.add(menu1);
    await menuApi.add(menu2);
  }
}

export const component: IComponent = new Main();
```

The menu hierarchy will then be displayed like this:

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/menus/child_menus.png" >}}

## Updating a menu

Sometimes you might want to disable a menu or update its caption depending on a condition. You can do so by calling the menu api's `updateMenu` method.

If you replace your `src/main/index.ts` with the code below, you'll see how you can disable a menu after it gets clicked, as well as updating its caption.

```typescript
import { IComponent, Menu, studioPro } from "@mendix/extensions-api";

const menuApi = studioPro.ui.extensionsMenu;

const menuId = "my-menu-unique-id";
const caption = "My First Menu";

menuApi.addEventListener("menuItemActivated", (args) => {
  if (args.menuId !== menuId) return;
  menuApi.update(menuId, {
    caption: `${caption} (Disabled)`,
    enabled: false,
  });
});
class Main implements IComponent {
  async loaded() {
    const menu: Menu = {
      caption: caption,
      menuId: menuId,
      subMenus: [],
      hasSeparatorBefore: false,
      hasSeparatorAfter: true,
      enabled: true,
    };

    await menuApi.add(menu);
  }
}

export const component: IComponent = new Main();
```

You can see here that the state of the menu is now disabled and its caption has also been updated. Only caption and enabled state are currently supported for updating.

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/menus/disabled_menu.png" >}}

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
