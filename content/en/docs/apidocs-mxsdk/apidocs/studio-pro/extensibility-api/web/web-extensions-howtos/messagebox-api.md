---
title: "Message Box Api"
url: /apidocs-mxsdk/apidocs/extensibility-api/web/messagebox-api/
weight: 7
---

# Showing a message box

In this example we'll learn how to show a message box to the Studio Pro user.

Create a new class `MyComponent.ts` that implements the Mendix's extensibility api `IComponent`.

```typescript
import { studioPro, IComponent } from "@mendix/extensions-api";

export class MyComponent implements IComponent {
  async loaded() {
    console.log("my extension was loaded");
  }
}
```

In your `main.ts` file, make sure the component is exported.

```typescript
import { IComponent } from "@mendix/component-framework";

import { MyComponent } from "./MyComponent";

export const myComponent: IComponent = new MyComponent();
```

Now, inside the `loaded` event in `MyComponent`, we will create a small menu which will display a dialog with text.

First we need to import the `menuApi` from the mendix `ide-foundation` package, and also the `messageBoxApi` to show our dialog. To see how to create menus, please see the menu api tutorial [here].

The class `MyComponent` should now look like below.

```typescript
import { IComponent, Menu, studioPro } from "@mendix/extensions-api";

const messageBoxApi = studio.ui.messageBoxes;
const menuApi = studioPro.ui.extensionsMenu;

const show_info_menu_id = "show-info-id";
const show_error_menu_id = "show-error-id";
const show_warning_menu_id = "show-warning-id";

menuApi.addEventListener("menuItemActivated", (args) => {
  if (args.menuId === show_info_menu_id)
    messageBoxApi.show("info", "This is information.", "Extra info");
  if (args.menuId === show_error_menu_id)
    messageBoxApi.show("error", "This is an error.", "Extra error details");
  if (args.menuId === show_warning_menu_id)
    messageBoxApi.show(
      "warning",
      "This is a warning.",
      "Extra warning details"
    );
});

export class MyComponent implements IComponent {
  async loaded() {
    const infoMenu: Menu = {
      caption: "Show Info",
      menuId: show_info_menu_id,
    };

    const errorMenu: Menu = {
      caption: "Show Error",
      menuId: show_error_menu_id,
    };

    const warningMenu: Menu = {
      caption: "Show Warning",
      menuId: show_warning_menu_id,
    };

    await menuApi.add(infoMenu);
    await menuApi.add(errorMenu);
    await menuApi.add(warningMenu);
  }
}
```

As you can see below, you can add extra information which will show up in the expandable area shown by the `Details` button in the dialog. This is optional, and collapsed by default.

![Info](/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/messageBoxes/info.png)

![Error](/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/messageBoxes/error.png)

![Warning](/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/messageBoxes/warning.png)

# Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
