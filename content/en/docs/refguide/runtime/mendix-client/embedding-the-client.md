---
title: "Embedding the Client"
url: /refguide/mendix-client/embedding-the-client/
description: "Describes how to load a Mendix web app into another web application by using the embedded client."
weight: 30
---

## Introduction

The embedded client lets you load a Mendix web app inside another web application without using the standard Mendix shell page.

In this setup, the host application owns the surrounding page and the Mendix app owns the region where it is mounted.

This page describes how to do the following:

* Configure an Embedded navigation profile in your Mendix app
* Load the embedded client from a host application
* Pass page parameters to the embedded home page
* Mount and unmount the client at the correct lifecycle moment
* Handle common integration issues

## Prerequisites

Before you start, make sure you have the following:

* A Mendix version that supports the embedded client
* A Mendix web app that you can run locally or deploy
* A host web app that can create a DOM element and load JavaScript by using a dynamic import

## How the Embedded Client Works

When your app contains an Embedded navigation profile, the Mendix runtime exposes an embedded entry bundle at `/dist/embedded-index.js`.

Your host application is responsible for the following:

* Choosing the Mendix runtime URL
* Loading the embedded bundle with a dynamic import
* Creating a DOM element that Mendix can render into
* Calling `render(...)` on the embedded bundle
* Calling the returned unmount function when the host component is removed
* Showing any loading or error state in the host UI

The same integration pattern works in React, Vue, plain JavaScript, and other frontend frameworks.

## Configuring the Embedded Navigation Profile

To enable the embedded client for your Mendix app, do the following:

1. Open your app in Studio Pro.
2. Open **App** > **Navigation**.
3. Click **Add navigation profile**.
4. Select **Embedded**.
5. Configure the **Default home page** for the Embedded profile.
6. Configure the error page for the Embedded profile (optional).
7. Run or deploy the app.

After you add the Embedded profile, the Mendix runtime serves the following bundle:

```text
<runtime-url>/dist/embedded-index.js
```

For example, if your runtime URL is `http://localhost:8081`, the embedded bundle is served from `http://localhost:8081/dist/embedded-index.js`.

The Embedded profile defines the starting page for the embedded app. It also defines which page is shown if the embedded app reaches an error state during startup or navigation.

## Configuring the Embedded Home Page

The Embedded profile uses its own home page. This is the first page shown when the host calls `render(...)`.

If your embedded home page requires page parameters, pass those values from the host application by using the `parameters` object in the `render(...)` configuration.

## Passing Parameters to the Embedded Home Page

You can pass page parameters for the embedded home page in the `parameters` object.

For example:

```js
const DEFAULT_REMOTE_URL = "https://your-mendix-runtime.example.com";

export async function mountEmbeddedMendix(container) {
    const configuredRemoteUrl = window.__MENDIX_REMOTE_URL__ ?? DEFAULT_REMOTE_URL;
    const remoteUrl = configuredRemoteUrl.replace(/\/+$/, "");

    const embeddedModule = await import(`${remoteUrl}/dist/embedded-index.js`);

    return embeddedModule.render(container, {
        remoteUrl: `${remoteUrl}/`,
        minHeight: "620px",
        parameters: {
            customerId: "12345"
        }
    });
}
```

The parameter names in `parameters` must match the page parameters expected by the embedded home page.

## Creating a Host Container

Create an element in the host application where the Mendix app will be mounted.

For example:

```html
<div id="embedded-mendix-host"></div>
```

Or in a framework component:

```tsx
<div ref={hostRef} />
```

The host only needs to provide a real DOM node.

## Loading and Rendering the Embedded Client

The embedded bundle exposes a `render(...)` function. A minimal framework-agnostic integration looks like this:

```js
const DEFAULT_REMOTE_URL = "https://your-mendix-runtime.example.com";

export async function mountEmbeddedMendix(container) {
    const configuredRemoteUrl = window.__MENDIX_REMOTE_URL__ ?? DEFAULT_REMOTE_URL;
    const remoteUrl = configuredRemoteUrl.replace(/\/+$/, "");

    const embeddedModule = await import(`${remoteUrl}/dist/embedded-index.js`);

    return embeddedModule.render(container, {
        remoteUrl: `${remoteUrl}/`,
        minHeight: "620px",
        parameters: {
            customerId: "12345"
        }
    });
}
```

This code does the following:

* Resolves the runtime URL
* Loads the embedded bundle from the Mendix runtime
* Passes page parameters to the embedded home page
* Calls `render(...)` with the container element
* Returns the unmount function from the embedded client

{{% alert color="info" %}}
If your host application uses Vite, add the `/* @vite-ignore */` comment to the dynamic import so Vite does not try to resolve the runtime URL during the host build.
{{% /alert %}}

## Mounting and Cleaning Up

Call the embedding logic only after the container exists and store the returned unmount function.

Typical lifecycle hooks include the following:

* React: `useEffect(...)`
* Vue: `onMounted(...)` and `onBeforeUnmount(...)`
* Plain JavaScript: after locating the container with `getElementById(...)` and during your own teardown flow

For example:

```js
const container = document.getElementById("embedded-mendix-host");
const unmount = await mountEmbeddedMendix(container);

// Call this when the host view is removed.
unmount();
```

## Handling Errors

The most common integration failure is that the Mendix runtime is not serving `embedded-index.js`. Wrap the load and render steps in a `try/catch` block.

```js
const remoteUrl = "https://your-mendix-runtime.example.com";

try {
    const embeddedModule = await import(`${remoteUrl}/dist/embedded-index.js`);
    const unmount = await embeddedModule.render(container, {
        remoteUrl: `${remoteUrl}/`,
        minHeight: "620px",
        parameters: {
            customerId: "12345"
        }
    });
} catch (error) {
    console.error(error);
    showError(`Unable to load the embedded Mendix bundle from ${remoteUrl}/dist/embedded-index.js.`);
}
```

At minimum, the host should do the following:

* Log the technical error
* Show a visible error message in the host UI
* Keep the surrounding page intact

## Cross-Origin Requests

If the host app and the Mendix runtime use different origins, make sure the Mendix runtime accepts requests from the host origin. This is required because the host app loads the embedded bundle and subsequent client resources from the Mendix runtime. For more information, see [Configure CORS](/refguide/configure-cors/).

## Read More

* [Mendix Client](/refguide/mendix-client/)
* [Mendix React Client](/refguide/mendix-client/react/)
* [Navigation](/refguide/navigation/)
* [Setting Up Navigation](/refguide/setting-up-the-navigation-structure/)
* [Configure CORS](/refguide/configure-cors/)