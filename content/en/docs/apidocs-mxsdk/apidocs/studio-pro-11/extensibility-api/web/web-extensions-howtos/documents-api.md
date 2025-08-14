---
title: "Using the Documents API"
linktitle: "Documents API"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/documents-api/
---

## Introduction

This how-to describes how to interact with document editors in Studio Pro.

## Prerequisites

This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/). Please complete that how-to before starting this one. You should also be familiar with command registration as described in [Register a Command Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/command-api/).

## Creating a Context Menu

In this example, we'll create a menu which will show for each `Entity` in the `Domain Model Editor` of Studio Pro. In order to specify which type of document a menu should belong to, we need to use the full name of the document type, i.e. `DomainModels$Entity` for Entities, `DomainModels$Annotation` for Annotations, or even `DomainModels$DomainModel` for the editor canvas itself. The documentation for these document type names can be found here [Access a Mendix Model Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/model-api/).

First, a command must be registered through the [Commands API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/command-api/), then the `commandId` can be attached to the new menu. Then, using the `documents` api's `addContextMenu` method, the menu can be added to an `Entity` inside the `Domain Model Editor`.

```typescript
import { ComponentContext, IComponent, Menu, StudioProApi, getStudioProApi } from "@mendix/extensions-api";

const extensionId = "myextension";

export const component: IComponent = {
    async loaded(componentContext: ComponentContext) {
        const studioPro = getStudioProApi(componentContext);

        const commandId = `${extensionId}.entity.command`;
        const menuId = `${commandId}.menu`;

        await studioPro.app.commands.registerCommand<{ documentId: string }>(commandId, async (args: { documentId: string }) => {
            await studioPro.ui.notifications.show({
                title: `Entity command executed`,
                message: `You clicked a context menu for an Entity! (${args.documentId})`,
                displayDurationInSeconds: 4
            });
        });

        const microflowMenu: Menu = { caption: `Entity command menu`, menuId, commandId };

        await studioPro.ui.documents.addContextMenu(microflowMenu, "DomainModels$Entity");
    }
}
```

As you can see, the expected payload of the command will be an object containing a document id (`{ documentId: string }`). Registering the command requires the exact type of the payload, or your extension will not compile. The `documentId` will be the id of the document the menu is attached to, in this case, the exact `Entity` in the Domain Model Editor canvas.

{{% alert color="info" %}}
It is important to remember that the command must be registered before creating the menu.
{{% /alert %}}

## Conclusion

You have seen how to create context menus for a document editor which execute a previously registered command.

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.