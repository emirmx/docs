---
title: "Using the App Explorer API"
linktitle: "App Explorer API"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/app-explorer-api/
---

## Introduction

This how-to describes how to interact with the App Explorer in Studio Pro. In this example, you create a menu which will show for each microflow in the App Explorer.

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* Read up on how menus work in the Extensibility API [here](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu/)
* This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/). Complete that how-to before starting this one.

## Creating a Context Menu

{{% alert color="info" %}}
Use the full name of the document type to specify which type of document a menu should belong to (for example, `Microflows$Microflow` for microflows or `Pages$Page` for pages). For more information about these document type names, see [Access a Mendix Model Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/model-api/).
{{% /alert %}}

The code below uses the `appExplorer` API's `addContextMenu` method to add the menu to all `Microflow` document nodes. When this menu is clicked, the clicked document's ID will be sent as an argument through the `DocumentContext` argument parameter of the menu.

```typescript
import { ComponentContext, DocumentContext, IComponent, Menu, getStudioProApi } from "@mendix/extensions-api";

const extensionId = "myextension";

export const component: IComponent = {
    async loaded(componentContext: ComponentContext) {
        const studioPro = getStudioProApi(componentContext);

        const menuId = `${extensionId}.microflow.menu`;

        const action = async (args: DocumentContext) => {
            await studioPro.ui.notifications.show({
                title: `Microflow action executed`,
                message: `You clicked a context menu for a Microflow! (${args.documentId})`,
                displayDurationInSeconds: 4
            });
        };

        const microflowMenu: Menu<DocumentContext> = { caption: `Microflow menu`, menuId, action };

        await studioPro.ui.appExplorer.addContextMenu(microflowMenu, "Microflows$Microflow");
    }
};
```

The `DocumentContext` payload for the menu action is an object containing a document Id (`{ documentId: string }`). When creating a menu for the `appExplorer`'s `addContextMenu` method, the `DocumentContext` should be used as the context of your menu. The `documentId` will be the Id of the document the menu is attached to (in this scenario, the exact `Microflow` node in the App Explorer).

As explained in the [menu documentation](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu/), the `DocumentContext` is not necessary in order to add your menu to Studio Pro, but if it is not used, the menu will never receive the ID of the clicked document when it gets clicked.

## Extensibility Feedback

If you would like to provide additional feedback, you can complete a small [survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback).

Any feedback is appreciated.
