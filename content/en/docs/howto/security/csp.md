---
title: "Content Security Policy"
url: /howto/security/csp/
weight: 80
description: By employing a content security policy (CSP) in your app, you can protect it from malicious content which might try to take advantage of the app's trusted web page context.
aliases:
    - /howto/security/using-mobile-capabilities/csp/
---

## Introduction

By employing a content security policy (CSP) in your app, you can protect it from malicious content which might try to take advantage of the app's trusted web page context. A rigorous CSP allows you to control which resources are loaded in the app.

A web app (including progressive web apps) can be made more strict and secure by setting its CSP to `default-src: self`. By doing so, only resources from the same domain can be loaded and no resources can be loaded inline (such as Base64 images or inline JavaScript).

For more background information on CSPs, see [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) from Mozilla.

{{% alert type="warning" %}}
Currently, some of Mendix's pluggable widgets are not fully compliant with CSP. If used with strict CSP, these widgets can result in CSP errors in the console or broken flows. Please refer to [widget's security documentation](/appstore/widgets/security/content-security-policy/) page for more details.
{{% /alert %}}

## Setup

In order to be able to use the strictest setting of a CSP (`default-src: self`) you must make some changes in your application. See the sections below for guidance.

### Updating the Theme Folder

To upgrade your theme directory to latest version, complete the following steps:

1. Rename your current theme directory. For example, you can use *theme_backup* as the new name.
1. Download the new theme files from this GitHub link: [theme.zip](https://github.com/mendix/atlas/releases/download/atlasui-theme-files-2024-01-25/atlasui-theme-files.zip). Extract the downloaded file into the root of your Mendix app folder. The folder structure should be similar to the previous folder, (meaning the Mendix app root, then the theme, and then the web and native folders).
1. After extracting the new theme files, restore your custom styling from the backup by copying over the new theme folder files. You will see the main changes enacted to make things compatible with strict CSP involve the `login.html` file and one JavaScript file for the toggled password.

### Changing the Theme

#### In React Client

Create a new file in your theme folder (**theme/web/appSetup.js**) with the following:

```js
if (!document.cookie || !document.cookie.match(/(^|;) *originURI=/gi)) {
    const url = new URL(window.location.href);
    const subPath = url.pathname.substring(0, url.pathname.lastIndexOf("/"));

    document.cookie = `originURI=${subPath}/login.html${window.location.protocol === "https:" ? ";SameSite=None;Secure" : ""}`;
}
```

Create a second file to contain the script for unsupported browsers (*theme/web/unsupported-browser.js*):

```js
// Redirect to unsupported browser page if opened from browser that doesn't support Symbols
if (typeof Symbol !== "function") {
    var homeUrl = window.location.origin + window.location.pathname;
    var appUrl = homeUrl.slice(0, homeUrl.lastIndexOf("/") + 1);
    window.location.replace(appUrl + "unsupported-browser.html");
}
```

Next, the *theme/web/index.html* file needs to be changed to use these files directly. If you lack this file, complete the [Customizing index.html (Web)](/howto/front-end/customize-styling-new/#custom-web) section of *Customize Styling*. Once you have the file, you can proceed.

In *theme/web/index.html* do the following:

1. Remove the line with the `{{unsupportedbrowsers}}` tag.
1. Remove the `<script>` which tells the client where to redirect to if a user is required to log in.
1. At the top of the `<head`> tag, add a reference to the `unsupported-browser.js` script:

    ```js
    <html>
        <head>
            <script src="unsupported-browser.js"></script>
            ...
        </head>
        ...
    </html>
    ```

1. In the `<body>` tag, add a reference to the `appSetup.js` script before `index.js` is loaded:

    ```js
    <html>
        <body>
            ...
            <div id="root"></div>
            <script src="appSetup.js"></script>
            <script src="dist/index.js?{{cachebust}}"></script>
        </body>
    </html>
    ```

Lastly, ensure you are not using any external fonts by checking your theme's styling to confirm all of the fonts are loaded locally.

#### In Dojo Client

{{% alert color="warning" %}}
In Mendix 11.0 and above, the Dojo Client is deprecated.
{{% /alert %}}

Create a new file to contain the Dojo configuration in your theme folder (*theme/web/appSetup.js*) with the following configuration:

```js
window.dojoConfig = {
    // Default Dojo config
	isDebug: false,
	useCustomLogger: true,
	async: true,
	baseUrl: "mxclientsystem/dojo/",
	cacheBust: "{{cachebust}}",
	rtlDirect: "index-rtl.html",

    // CSP Dojo config
	has: {
        "csp-restrictions": true
    },
	blankGif: "mxclientsystem/dojo/resources/blank.gif"
};

if (!document.cookie || !document.cookie.match(/(^|;) *originURI=/gi)) {
    const url = new URL(window.location.href);
    const subPath = url.pathname.substring(0, url.pathname.lastIndexOf("/"));
    document.cookie = `originURI=${subPath}/login.html${window.location.protocol === "https:" ? ";SameSite=None;Secure" : ""}`;
}
```

Create a second file to contain the script for unsupported browsers (*theme/web/unsupported-browser.js*):

```js
// Redirect to unsupported browser page if opened from browser that doesn't support Symbols
if (typeof Symbol !== "function") {
    var homeUrl = window.location.origin + window.location.pathname;
    var appUrl = homeUrl.slice(0, homeUrl.lastIndexOf("/") + 1);
    window.location.replace(appUrl + "unsupported-browser.html");
}
```

Finally, the *theme/web/index.html* file needs to be changed to use these files directly. If you lack this file, please follow the [Customizing index.html (Web)](/howto/front-end/customize-styling-new/#custom-web) section of *Customize Styling*.

In *theme/web/index.html* do the following:

1. Remove the line with the `{{unsupportedbrowsers}}` tag
1. Remove the `<script>` tag with the `dojoConfig` inside
1. At the top of the `<head`> tag, add a reference to the `unsupported-browser.js` script:

    ```js
    <html>
        <head>
            <script src="unsupported-browser.js"></script>
            ...
        </head>
        ...
    </html>
    ```

1. In the `<body>` tag, add a reference to the `appSetup.js` script before `mxui.js` is loaded:

    ```js
    <html>
        <body>
            ...
            <div id="content"></div>
            <script src="appSetup.js"></script>
            <script src="mxclientsystem/mxui/mxui.js?{{cachebust}}"></script>
        </body>
    </html>
    ```

Lastly, ensure you are not using any external fonts by checking your theme's styling to confirm all of the fonts are loaded locally.

#### Testing Your Changes Locally

To check that your changes are working locally, you can add a custom `Content-Security-Policy` header in your [configuration](/refguide/configuration/#headers).

After redeploying your app locally, it should function as normal. If your app does not load or if there are errors, check that you have completed all steps listed above.

After you finish testing locally, remember to remove the line of code in the `head` tag.

### Enabling the Header in the Cloud

To enable the header in the cloud, follow the instructions in the [HTTP Headers](/developerportal/deploy/environments-details/#http-headers) section of *Environment Details*.
