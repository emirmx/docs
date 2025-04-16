---
title: "Show a Popup Notification Using Web API"
linktitle: "Show Notifications"
url: /apidocs-mxsdk/apidocs/web-extensibility-api/notifications-api/
weight: 30
---

## Introduction

This how-to shows you how to show a simple popup notification in Studio Pro using the web extension API.

## Prerequisites

This how-to uses the results of [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api/getting-started/). Please complete that how-to before starting this one.

## Showing a Notification

In this section, you will show a popup notification when your extension gets loaded. It will disappear after 5 seconds.

Before we begin, find an icon of your choosing and copy it inside your extension solution. We recommend creating an `assets` folder under your `src` folder.
You will then need to create an `Icons.ts` file inside that same `assets` folder, which will look like below (the png image used in this example is called `check.png`):

```typescript
import Check from "./check.png";

interface IIcons {
    Check: string;
}

export default { Check } as IIcons;
```

In order for this code to compile, you will need another file, `images.d.ts`. The `d` file extension means this is a `declaration` file, and the `declare module "*.png";` line of code tells TypeScript that any import ending in `.png` should be treated as a module. This helps TypeScript understand how to handle PNG files when you import them in your code.

Now we are able to use images in our extensions. Replace your `src/main/index.ts` file with the following:

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

The code imports the `notificationsApi` from `studioPro.ui.notifications` to allow you to use the notifications API.
Inside the `loaded` event, it calls the `show` method to show a popup notification with the title `Extension Loaded`, a message, and the `check.png` icon you set up earlier. The `displayDurationInSeconds` means that the popup will go away after 5 seconds. If no duration is provided, the popup will remain indefinitely until the user removes it themselves.

Now, when the extension loads, the notification will show in the top right corner of the Studio Pro app, as shown below.

{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/notifications/notification.png" >}}

## Notification Api Show Method

The show method has the following parameters:

* `title` – the title of the notification
* `message` – the text content of the notification
* `displayDurationInSeconds` – an optional duration in seconds for the notification to remain visible
* `icon` – an optional icon inside the notification

## Conclusion

You have seen how to show a notification inside a Studio Pro app.

## Extensibility Feedback

If you would like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

Any feedback is much appreciated.
