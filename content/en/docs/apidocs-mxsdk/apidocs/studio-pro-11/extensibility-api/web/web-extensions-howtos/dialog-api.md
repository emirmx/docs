---
title: "Open a Modal Dialog Using Web API"
linktitle: "Open a Modal Dialog"
url: /apidocs-mxsdk/apidocs/web-extensibility-api/dialog-api/
weight: 60
---

## Introduction

This how-to shows you how to open a modal dialog in Studio Pro from an extension. This dialog will contain your web content.

## Prerequisites

This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api/getting-started/). Please complete that how-to before starting this one. You should also be familiar with creating menus as described in [Create a Menu Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api/menu-api/).

## Opening a Modal Dialog

Firstly, create a menu item to open the dialog. This is done inside the `loaded` event in `Main`. For more information see [Create a Menu Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api/menu-api/).

In a listener event called `menuItemActivated` the `studioPro.ui.dialogs.showModal(<dialogInfo>, <uiSpec>)` call opens a new tab where:

- `<dialogInfo>` is an object containing the `title` of the dialog, which will be shown in the title bar of your dialog in Studio Pro. It also contains `contentSize` object where `height` and `width` dimensions for the dialog can be provided.
- `<uiSpec>` is an object containing two required properties and one optional:

  - `componentName` which is the name of the extension prefixed with "extension/". For example "extension/myextension" in the following example.
  - `uiEntryPoint` which is the name mapped from the `manifest.json` file.
  - `queryParams` (optional) is a key-value pair object for passing data to your web content inside the dialog.

{{% alert color="info" %}}
Whenever the dialogs API `showModal` method is called, a `Promise` of `unknown` or `null` is returned. This return value represents anything that the web content determines should be returned when the dialog gets closed. It is currently unknown by the api, since it can be anything. In this example the dialog will contain a form where an object is modified, and it is then returned at closing time.
{{% /alert %}}

An example of the class `Main` to open a modal dialog called **My Extension Dialog** looks similar to the following:

```typescript
import { IComponent, studioPro, TabHandle } from "@mendix/extensions-api";

class Main implements IComponent {
  menuId = "myextension.ShowModalDialog";

  async loaded() {
    // Add a menu item to the Extensions menu
    await studioPro.ui.extensionsMenu.add({
      menuId: menuId,
      caption: "Show modal dialog",
    });

    // Open a modal dialog when the menu item is clicked
    studioPro.ui.extensionsMenu.addEventListener(
      "menuItemActivated",
      async (args) => {
        if (args.menuId === this.menuId) {
          const result = await studioPro.ui.dialogs.showModal(
            {
              title: "Modal Dialog",
              contentSize: { height: 170, width: 400 },
            },
            {
              componentName: "extension/myextension",
              uiEntrypoint: "dialog",
            }
          );

          if (result !== null)
            await studioPro.ui.messageBoxes.show(
              "info",
              JSON.stringify(result)
            );
        }
      }
    );
  }
}

export const component: IComponent = new Main();
```

## Filling the Dialog With Content

In the previous example, the `uiEntryPoint` property of the `<uispec>` object had the value "dialog". This value must match the one from the manifest. Here is an example of the dialog under the `ui` property.

```json
{
  "mendixComponent": {
    "entryPoints": {
      "main": "main.js",
      "ui": {
        "dialog": "dialog.js"
      }
    }
  }
}
```

1. Update `vite.config` to match the manifest with an entry for each tab. For example:

   ```typescript
   import { defineConfig, ResolvedConfig, UserConfig } from "vite";

   export default defineConfig({
     build: {
       lib: {
         formats: ["es"],
         entry: {
           main: "src/main/index.ts",
           dialog: "src/ui/dialog.tsx",
         },
       },
       rollupOptions: {
         external: ["@mendix/component-framework", "@mendix/model-access-sdk"],
       },
       outDir: "./dist/myextension",
     },
   } satisfies UserConfig);
   ```

2. Add the `dialog.tsx` file for the web content

```typescript
import { studioPro } from "@mendix/extensions-api";
import { FormEvent, StrictMode } from "react";
import { createRoot } from "react-dom/client";

const dialogId = new URLSearchParams(location.search).get("dialogId")!;
const person: { firstName?: string; lastName?: string } = {
  firstName: undefined,
  lastName: undefined,
};

const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  studioPro.ui.dialogs.closeWithResult(dialogId, JSON.stringify(person));
};

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <form onSubmit={handleSubmit}>
      <p>
        {" "}
        <input
          placeholder="First Name"
          value={person.firstName}
          onChange={(e) => (person.firstName = e.target.value)}
        />
      </p>
      <p>
        {" "}
        <input
          placeholder="Surname"
          value={person.lastName}
          onChange={(e) => (person.lastName = e.target.value)}
        />
      </p>
      <p>
        <button type="submit">Submit</button>
        <button
          type="button"
          onClick={(_) => studioPro.ui.dialogs.close(dialogId)}
        >
          Cancel
        </button>
      </p>
    </form>
  </StrictMode>
);
```

Notice the `dialogId` property retrieved from the query parameters of the web page. This value is generated once the dialog api is first called and it is then passed back to the web content so that the `close` or `closeWithResult` methods can be called successfully. The dialogs api needs this id in order to close the correct dialog.
This simple form contains two textboxes for a `firstName` and `lastName` and on submitting of the form, it will then close the dialog by passing along the content of the object modified by the form.
It also contains a simple `Close` button which calls the api's `close` method, without any extra data apart from the required `dialogId`.

After building and installing the extension in our Studio Pro app, the dialog will open once the menu is clicked and display the web content from the `dialog.tsx` file.

## Modifying a Modal Dialog

We can also modify the dimensions of a dialog, using the dialog api's `update` method. Add a button to the form contained in `dialog.tsx` file, as follows:

```typescript
<button
  type="button"
  onClick={(_) => {
    studioPro.ui.dialogs.update(dialogId, {
      height: document.documentElement.scrollHeight + 20,
      width: document.documentElement.scrollWidth + 20,
    });
  }}
>
  Expand by 20 pixels
</button>
```

## Conclusion

You now know how to create dialogs and populate them with content and handle their result once closed. You can also modify the dialog's dimensions while it is open.

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
