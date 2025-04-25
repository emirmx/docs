---
title: "Extensibility API for C# Developers"
linktitle: "C# Extensibility API"
url: /apidocs-mxsdk/apidocs/csharp-extensibility-api/
description: "The C# extensibility API allows your custom Studio Pro extensions developed in C# to interact with some internal services of Studio Pro."
weight: 10
no_list: false
description_list: true
aliases:
    - /apidocs-mxsdk/apidocs/extensibility-api/
---

{{% alert color="warning" %}}
This feature is in beta. For more information, see [Beta Releases](/releasenotes/beta-features/).
{{% /alert %}}

{{% alert color="info" %}}
For information on new releases of the Extensibility API, see [Extensibility: C# API Release Notes](/releasenotes/studio-pro/csharp-extensibility-api/).
{{% /alert %}}

## Introduction

Extensions can be written in C#, described here, or using a web API which is documented separately in [Extensibility API for Web Developers](/apidocs-mxsdk/apidocs/web-extensibility-api/).

If you need to add your own custom UI to Studio Pro, you can achieve this using web technology. Your web-based UI will be rendered in Studio Pro using a hosted web view, the API provides communication functionality between your web UI and the C# extension logic.

## Prerequisites

* You need at least a basic understanding of the Mendix platform.
* You need some understanding of the Mendix Model.
* You need to have some C# development experience. Extensions are developed using [C#](https://docs.microsoft.com/en-us/dotnet/), and compiled into a `.dll` assembly file.

## Getting Started

For detailed explanation on how to get started with extensions, check out [Get Started with the Extensibility API](/apidocs-mxsdk/apidocs/csharp-extensibility-api/get-started/).

You can also check out our examples and [API reference documentation](https://github.com/mendix/ExtensionAPI-Samples).

## How-tos

Here is a list of how-tos for you to begin with:

* [How to Create a Menu Extension Using C#](/apidocs-mxsdk/apidocs/csharp-extensibility-api/create-menu-extension/)
* [How to Create a Dockable Pane Extension Using C#](/apidocs-mxsdk/apidocs/csharp-extensibility-api/create-dockable-pane-extension/)
* [How to Create a Context Menu Using C#](/apidocs-mxsdk/apidocs/csharp-extensibility-api/create-context-menu/)
* [How to Create a Web View Hosted Inside a Modal Dialog Using C#](/apidocs-mxsdk/apidocs/csharp-extensibility-api/create-modal-web-view/)
* [How to Create Microflows for Calculations Using C#](/apidocs-mxsdk/apidocs/csharp-extensibility-api/create-microflows-for-calculations/)

## Advanced APIs

APIs for the Mendix platform's advanced users:

* [Use the Untyped Model Access API Using C#](/apidocs-mxsdk/apidocs/untyped-model-access-api/)

## Learn More

You can dive into the following topics in depth:

* [What are extension points](/apidocs-mxsdk/apidocs/csharp-extensibility-api/extension-points/)
* [What are the Extensibility API services](/apidocs-mxsdk/apidocs/csharp-extensibility-api/services/)
* [How to Interact with the Model API Using C#](/apidocs-mxsdk/apidocs/interact-with-model-api/)
* [How to host web content via a web view wrapper](/apidocs-mxsdk/apidocs/csharp-extensibility-api/web-views/)
* [How to Build a Todo Example Extension Using C#](/apidocs-mxsdk/apidocs/csharp-extensibility-api/build-todo-example-extension/)

## Documentation in This Category
