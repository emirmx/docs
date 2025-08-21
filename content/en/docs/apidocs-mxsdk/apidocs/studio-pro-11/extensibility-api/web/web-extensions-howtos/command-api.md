---
title: "Register a Command Using Web API"
linktitle: "Register Commands"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/command-api/
---

## Introduction

This how-to describes how to register a command in Studio Pro from an extension. This command can then be attached to a menu or context menus for a document or an editor.

## Prerequisites

This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/). Please complete that how-to before starting this one. You should also be familiar with creating menus as described in [Create a Menu Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu-api/).

## Register Commands

The Commands api allows users to register a reusable command. These commands can be attached to menus and context menus. Currently two api's make use of this feature please check out:

[Documents API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/)
[App Explorer API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/app-explorer-api/)

To create commands a user can call the commands api's `registerCommand`.
In the sample code below, we register a command and then attach it to a menu, by setting the property `commandId` to the `Menu` object. It is important to note that, when using a command in a menu it is no longer necessary to listen to the `menuItemActivated` event. Now, with commands, you can simply register your action and set the command id on the menu itself.

Also note that the `registerCommand` requires a generic type for the command payload once executed. For standalone menus that have a command without payload, you can simply register it with `<void>`. When using `void` the generic type declaration can also be left out, i.e. `registerCommand<void>(commandId...)` becomes `registerCommand(commandId...)`.
For commands that require payload, you must make sure you register the command with the exact expected payload object type. See the [App Explorer API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/app-explorer-api/) and [Documents API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/documents-api/) documentations for clear examples.

```typescript
import { ComponentContext, IComponent, Menu, StudioProApi, getStudioProApi } from "@mendix/extensions-api";

const extensionId = "myextension";

export const component: IComponent = {
    async loaded(componentContext: ComponentContext) {
        const studioPro = getStudioProApi(componentContext);

        await this.createMenuWithCommand(studioPro);
    }

    async createMenuWithCommand(studioPro: StudioProApi) {
        const commandId = `${extensionId}.menu-command`;
        const menuId = `${commandId}.menu`;

        await studioPro.app.commands.registerCommand<void>(
            commandId,
            async () => await studioPro.ui.messageBoxes.show("info", `This menu executed a command with id '${commandId}'`)
        );

        await studioPro.ui.extensionsMenu.add({ caption: "Menu with command", menuId, commandId });
    }
}
```

As mentioned, it is also possible to create a context menu that belongs to a document in the app explorer or a document editor, and that menu can have a registered command attached to it. To do so, we can use the `appExplorer` api, or the `documents` api. Please see their respective tutorials, [App Explorer API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/app-explorer-api/) and [Documents API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/documents-api/).
The command registration for commands that interact with documents are slightly different. They require a payload of type `{ documentId: string }`, which the backend will return the menu gets clicked. The `documentId` is the id of the **exact** document that was interacted with by the menu.

{{% alert color="info" %}}
It is important to remember that the command must be registered before creating any menus that might be attached to it.
{{% /alert %}}

## Conclusion

You have seen how to register commands that can be attached to menus in Studio Pro.

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.