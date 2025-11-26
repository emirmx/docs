---
title: "Using the Documents API"
linktitle: "Documents API"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/documents-api/
---

## Introduction

This how-to describes how to create context menus for a document editor. In the example below, you create a menu which is shown for each entity in the domain model of Studio Pro.

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/). Please complete that how-to before starting this one. 
* Read up on how menus work in the Extensibility API [here](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu/)

## Creating a Context Menu

{{% alert color="info" %}}
Use the full name of the document type (for example, `DomainModels$Entity` for entities, `DomainModels$Annotation` for annotations, or  `DomainModels$DomainModel` for the editor canvas itself). For more information about these document type names, see [Access a Mendix Model Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/model-api/).
{{% /alert %}}

The code below does the following:

1. Create a menu object with a `DocumentContext`.
3. Uses the `documents` API's `addContextMenu` method to add the menu to an entity inside the domain model editor

```typescript
import { ComponentContext, DocumentContext, IComponent, Menu, getStudioProApi } from "@mendix/extensions-api";

const extensionId = "myextension";

export const component: IComponent = {
    async loaded(componentContext: ComponentContext) {
        const studioPro = getStudioProApi(componentContext);

        const menuId = `${extensionId}.entity.menu`;

        const action = async (args: { documentId: string }) => {
            await studioPro.ui.notifications.show({
                title: `Entity executed`,
                message: `You clicked a context menu for an Entity! (${args.documentId})`,
                displayDurationInSeconds: 4
            });
        };

        const entityMenu: Menu<DocumentContext> = { caption: `Entity menu`, menuId, action };

        await studioPro.ui.documents.addContextMenu(entityMenu, "DomainModels$Entity");
    }
};
```

As you can see from the example above, the expected payload of the menu action is `DocumentContext`, i.e. an object containing a document id (`{ documentId: string }`). The `documentId` will be the id of the document the menu is attached to, in this case, the exact entity in the Domain Model editor canvas.

## Extensibility Feedback

If you would like to provide additional feedback, you can complete a short [survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback).

Any feedback is appreciated.
