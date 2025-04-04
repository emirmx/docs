---
title: "Show a Message Box Using Web API"
linktitle: "Message Box"
url: /apidocs-mxsdk/apidocs/web-extensibility-api/messagebox-api/
weight: 50
---

## Prerequisites

This guide builds on top of the [getting started guide](/apidocs-mxsdk/apidocs/web-extensibility-api/getting-started/). Please complete that guide before starting this one.

## Showing a message box

In this example we'll learn how to show a message box to the Studio Pro user.

Inside the `loaded` event in `Main`, we will create a small menu which will display a dialog with text.

First we need to import the `menuApi` and also the `messageBoxApi` to show our dialog. To see how to create menus, please see the menu API tutorial [here].

The class `Main` should now look like below.

```typescript
import { IComponent, Menu, studioPro } from "@mendix/extensions-api";
const messageBoxApi = studioPro.ui.messageBoxes;
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

class Main implements IComponent {
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

export const component: IComponent = new Main();
```

As you can see below, you can add extra information which will show up in the expandable area shown by the `Details` button in the dialog. This is optional, and collapsed by default.

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/messageBoxes/info.png" >}}

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/messageBoxes/error.png" >}}

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/messageBoxes/warning.png" >}}

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
