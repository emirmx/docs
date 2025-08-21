---
title: "Extensibility: Web API Release Notes"
linktitle: "Extensibility: Web API"
url: /releasenotes/studio-pro/web-extensibility-api/
weight: 45
numberless_headings: true
---

These release notes cover changes to the [Extensibility API for Web Developers](/apidocs-mxsdk/apidocs/extensibility-api/).

## Version 11.2.0

* We introduced a new Command registration Api check out our guide at [Commands Api](/apidocs-mxsdk/apidocs/web-extensibility-api-11/command-api/)

* We included a new method for initializing the studio Pro api (this is a breaking change) Please check out the getting started guide for usage at [Getting started](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/)

## Version 11.1.0

* We added a new Editors api under `studioPro.ui.editors` which allows you to get the active document and open the default editor for a document.
You can check out our guide on the new api at [Editors Api](/apidocs-mxsdk/apidocs/web-extensibility-api-11/editor-api/).

## Version 11.0.0

* We introduced a new API for showing modal dialogs from web extensions. It is available under `studioPro.ui.dialogs` in the web extensibility API. For more details and practical examples, see [Open a Modal Dialog](/apidocs-mxsdk/apidocs/web-extensibility-api-11/dialog-api/).
* We introduced a new API for accessing user preferences from web extensions, which retrieves the user’s selected theme preference (light or dark) and language settings (for exampl, `en-US`). It is available under `studioPro.ui.preferences` in the web extensibility API. For more details and practical examples, see [Show User's Preferences](/apidocs-mxsdk/apidocs/web-extensibility-api-11/preference-api/).
* We introduced a new API for showing notification popups from web extensions. It is available under `studioPro.ui.notifications` in the web extensibility API. For more details and practical examples, see [Show a Pop-up Notification](/apidocs-mxsdk/apidocs/web-extensibility-api-11/notification-api/).

## Version 10.24.0

* No user facing changes. However, the extension package version must be the same as your Studio Pro version.

## Version 10.23.0

* No user facing changes. However, the extension package version must be the same as your Studio Pro version.

## Version 10.22.0

* No user facing changes. However, the extension package version must be the same as your Studio Pro version.

## Version 10.21.0

* The first [beta](/releasenotes/release-status/) release of the Web Extensibility API.
