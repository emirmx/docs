---
title: "Show a Popup Notification Using Web API"
linktitle: "Show Notification"
url: /apidocs-mxsdk/apidocs/web-extensibility-api/notification-api/
weight: 30
---

## Introduction

This how-to shows you how to show a simple popup notification in Studio Pro using the notificationsAPI from the web extension API.

## Prerequisites

This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api/getting-started/). Please complete that how-to before starting this one.

## Showing a Notification

This how-to explains how to show a popup notification when your extension gets loaded. The notification will disappear after 5 seconds.

1. Create an `assets` folder under your `src` folder.
2. Find an icon you want to use in your notification and copy it into the `assets` folder. (This example uses the file `check.png`)
3. Create an `Icons.ts` file inside that same `assets`.
4. Add the following code to the `Icons.ts` file, replacing `check.png` with the name of your icon and using an appropriate name in the `import`, `IIcons`, and `export` statements.

    ```typescript
    import Check from "./check.png";

    interface IIcons {
        Check: string;
    }

    export default { Check } as IIcons;
    ```

5. Create an `images.d.ts` files.

    This is a `declaration` file, as indicated by the `d` file extension.
    
6. Add the line `declare module "*.png";` to the `images.d.ts` file.

    This tells TypeScript that any import ending in `.png` should be treated as a module. This enables TypeScript to handle PNG files correctly when you import them in your code.

    Now you can use images in your extensions. 

7. Replace your `src/main/index.ts` file with the following, again using the appropriate icon name in place of `Check`:

    ```typescript
    import { IComponent, studioPro } from "@mendix/extensions-api";
    import Icons from "../assets/Icons";

    const notificationsApi = studioPro.ui.notifications;

    class Main implements IComponent {
        async loaded() {
            await notificationsApi.show({
                title: "Extension Loaded",
                message: "The extension was successfully loaded",
                displayDurationInSeconds: 5,
                icon: Icons.Check
            });
        }
    }

    export const component: IComponent = new Main();
    ```

    This code does the following:
    
    * Imports the `notificationsApi` from `studioPro.ui.notifications` to allow you to use the notifications API.
    * Implements a `loaded` event which calls the `show` method to show a popup notification for five seconds with the title `Extension Loaded`, a message, and the `check.png` icon you set up earlier. See [Full Reference for Show Method](#reference), below for more information.

Now, when the extension loads, your notification will show in the top right corner of Studio Pro:

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/notifications/notification.png" >}}

## Full Reference for Show Method {#reference}

The show method has the following parameters:

* `title` – the title of the notification
* `message` – the text content of the notification
* `displayDurationInSeconds` – an optional duration in seconds for the notification to remain visible. If no duration is provided, the popup will remain indefinitely until the user removes it themselves.
* `icon` – an optional icon which is displayed inside the notification

## Conclusion

You have seen how to use your extension to show a notification inside Studio Pro.

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.