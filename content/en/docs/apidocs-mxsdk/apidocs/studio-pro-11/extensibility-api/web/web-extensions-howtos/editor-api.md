---
title: "Open a Document Editor Using Web API"
linktitle: "Open a Document Editor"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/editor-api/
---

## Introduction

This how-to describes how to open an existing document editor in Studio Pro from an extension.

## Prerequisites

This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/). Please complete that how-to before starting this one. You should also be familiar with creating menus as described in [Create a Menu Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu-api/).

## Opening a Document Editor

Create a menu item. This is done inside the `loaded` event in `Main`. For more information, see [Create a Menu Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu-api/).

This menu action will look for the `Home_Page` document in `MyFirstModule` and it will then open it with the `editor-api`. Of course you can use any module or any document in your app. For more information, please look at the [model api](/apidocs-mxsdk/apidocs/web-extensibility-api-11/model-api/)

In a listener event called `menuItemActivated`, write the following code. First we look for the page by its name and by the name of its containing module using the `studioPro.app.model.pages` api.
Then we call `studioPro.ui.editors.editDocument` to open the document by passing its ID.

```typescript
import { IComponent, studioPro, Menu, Primitives } from "@mendix/extensions-api";

const menuId = "open-home-page";

studioPro.ui.extensionsMenu.addEventListener("menuItemActivated", async args => {
    if (args.menuId === menuId) {
        const [page] = await studioPro.app.model.pages.loadAll(
            (info: Primitives.UnitInfo) => info.moduleName === "MyFirstModule" && info.name === "Home_Web"
        );

        await studioPro.ui.editors.editDocument(page.$ID);
    }
});

export const component: IComponent = {
    async loaded() {
        const menu: Menu = {
            caption: "Open Home Page",
            menuId
        };

        await studioPro.ui.extensionsMenu.add(menu);
    }
};
```

## Active Documents
The editor api also supports notifying the extension when the active document tab gets activated in Studio Pro. It also provides this information on demand, via the `studioPro.ui.editors.getActiveDocument` method. This method returns a `ActiveDocumentInfo` object, which contains the document's name, type, container module name and id.
To get this `ActiveDocumentInfo` object when the active document changes, you can subscribe to the `activeDocumentChanged`. Add the following code to your extension:
```typescript
studioPro.ui.editors.addEventListener("activeDocumentChanged", async ({ info }) => {
    if (info) {
        studioPro.ui.notifications.show({
            title: "Document Changed Notification",
            message: `Name: ${info.documentName}\nID: ${info.documentId}\nType: ${info.documentType}\nModule: ${info.moduleName}`,
            displayDurationInSeconds: 5
        });
    }
});
```

## Extensibility Feedback

If you would like to provide us with additional feedback, you can complete a small [survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback).

Any feedback is appreciated.
