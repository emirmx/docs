---
title: "Menus in the Extensibiity API"
linktitle: "Menus"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/menu/
---

# Menu Properties

A menu has the following properties:

| Property             | Description                                                                   |
|----------------------|-------------------------------------------------------------------------------|
| `caption`            | The text of the menu item                                                     |
| `menuId`             | A unique identifier for the menu item                                         |
| `subMenus`           | A list of sub-menu items                                                      |
| `hasSeparatorBefore` <br> (default: `false`)  | Adds a visual separator before the item              |
| `hasSeparatorAfter` <br> (default: `false`)  | Adds a visual separator after the item                |
| `enabled`  <br> (default: `true`)  | If disabled, the action of the menu will not be invoked when clicked |
| `action` | The action that executes when the menu is clicked |

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/menus/grouped_menus.png" width="300" >}}

{{% alert color="info" %}}
It is important to note that a menu can only have an action or submenus. It cannot have both. A parent menu is not a clickable menu, it can only contain sub menus that are clickable or more parent menus. If you try to add both submenus and an action to a menu, the code will not compile.
{{% /alert %}}

# Menu action payload
The menu can have a context or no context. When adding a context menu to a document or an entity through our [appExplorer](/apidocs-mxsdk/apidocs/web-extensibility-api-11/app-explorer-api/) or [documents](/apidocs-mxsdk/apidocs/web-extensibility-api-11/documents-api/) APIs, Studio Pro will send the document id back to the menu, which then will invoke the action passing this document id as the argument. Which means the menu will need to be a `Menu<DocumentContext>`. So when you create the menu with this context, the action needs to have a `DocumentContext` as its argument, or the action will not receive the document id when the context menu gets clicked.
**The code will not compile if your action has the wrong payload** (e.g. if you pass a number or a different object), but it is perfectly fine for your action to have no payload at all, since your context menu might not need to do anything with the ID of the document.

The `DocumentContext` type has only one property, the `documentId` string:
```typescript
type DocumentContext = { documentId: string };
```

Here you can see the different ways to use menus:

```typescript
import { DocumentContext, IComponent, Menu, getStudioProApi } from "@mendix/extensions-api";

export const component: IComponent = {
    async loaded(componentContext) {
        const studioPro = getStudioProApi(componentContext);
        const menuApi = studioPro.ui.extensionsMenu;
        const appExplorerApi = studioPro.ui.appExplorer;
        const messageBoxApi = studioPro.ui.messageBoxes;

        const menuId = "menu-without-context";

        const menu: Menu = {
            caption: "Menu Without Context",
            menuId,
            action: async () => await messageBoxApi.show("info", `My menu '${menuId}' was clicked`)
        };

        await menuApi.add(menu);

        const documentContextMenuId = "menu-with-context";

        const documentContextMenu: Menu<DocumentContext> = {
            caption: "Menu With Context",
            menuId: documentContextMenuId,
            action: async (arg: DocumentContext) =>
                await messageBoxApi.show("info", `My menu '${documentContextMenuId}' for microflow id ${arg.documentId} was clicked`)
        };

        await appExplorerApi.addContextMenu(documentContextMenu, "Microflows$Microflow");

        const documentContextMenuWithoutPayloadId = "menu-with-context-no-payload";

        const documentContextMenuNoPayload: Menu<DocumentContext> = {
            caption: "Menu With Context But No Payload",
            menuId: documentContextMenuWithoutPayloadId,
            action: async () =>
                await messageBoxApi.show("info", `My menu '${documentContextMenuWithoutPayloadId}' for a microflow was clicked`)
        };

        await appExplorerApi.addContextMenu(documentContextMenuNoPayload, "Microflows$Microflow");
    }
};
```