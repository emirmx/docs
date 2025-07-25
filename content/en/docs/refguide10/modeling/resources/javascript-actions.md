---
title: "JavaScript Actions"
url: /refguide10/javascript-actions/
weight: 20
description: "This reference guide details the ways JavaScript Actions can extend the functionality of your Mendix app."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

{{% alert color="warning" %}}
This activity can only be used in **Nanoflows**.
{{% /alert %}}

## Introduction

With JavaScript actions, you can extend your application's functionality in ways nanoflows alone cannot. To use a JavaScript action, call it from a nanoflow using the [JavaScript Action Call](/refguide10/javascript-action-call/).

{{% alert color="info" %}}

Each JavaScript action defined in Mendix Studio Pro corresponds to a file *{JavaScript action name}.js* in the subdirectory **javascriptsource{module name}/actions/** in your app directory.

The skeletons of these *.js* files are generated automatically when you save an action, and those JavaScript actions can immediately be edited in the embedded code editor.

{{% /alert %}}

To learn how to create, configure, and use a JavaScript action, see these [Build JavaScript Actions](/howto10/extensibility/build-javascript-actions/) how-tos.

## General Settings

After double-clicking a JavaScript action in your **App Explorer** you will see the JavaScript action's settings: 

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/javascript-action-settings-no-para.png" alt="javascript settings" width="600"  class="no-border" >}}

The settings for JavaScript actions and their implications are detailed below.

### Name

This setting handles a JavaScript action's name, which a nanoflow refers to when performing a call to it. This name is also the name of the generated *.js* file.

### Parameters

Parameters pass data to JavaScript actions. For example, if you had a JavaScript action which multiplied numbers, parameters would define the numbers to be multiplied. A JavaScript action can have zero or more parameters. Each parameter should have a unique name. You may add a parameter by clicking **Parameters** > **Add**, and then customize that parameter to pass data into a JavaScript action:

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/parameter-naming.png" alt="parameter" class="no-border" >}}

In a JavaScript action's **Code** tab, you can see its parameters' values and handle its implementation. Each parameter has **name** (1), **type** (2), **category**, **description** (3), **return type** (4), and **required** (5):

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/parameter-code.png" alt="parameter code" class="no-border" >}}

You will see a parameter's category (1), parameter name (2), and description (3) in the **Call JavaScript Action** dialog box after double-clicking its activity in your nanoflow:

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/call-js-action-dialog.png" alt="call javascript action dialog"   width="400"  class="no-border" >}}

The parameter types supported by JavaScript actions are described below.

#### Name

This setting handles the parameter's name. A name is required. Names must start with a letter and contain only letters. Spaces are not permitted in names.

#### Type

|   Name   |   Description   |
| ---- | ---- |
|  Object    |   The object parameter type allows you to pass a Mendix object to a JavaScript action. You must also select its entity type, which can be either a specific entity or a type parameter. In the generated JavaScript action template code, this type is represented as an MxObject. |
|   List   |   The list parameter type allows you to pass a list of Mendix objects to a JavaScript action. You must also select its entity type, which can be either a specific entity or a type parameter. In the generated JavaScript action template code, this type is represented as an array of MxObjects. |
|   Entity   |   The entity parameter type is a placeholder. It stands in for an entity that will be replaced with a new entity's name when it is called in a nanoflow. Additionally, the entity type can be used to fill in a type parameter. In the generated JavaScript action template code, this type is represented as a string.  |
|   Nanoflow   |   The nanoflow parameter type allows you to pass a nanoflow that you can call from your JavaScript action. The value of the parameter is an async function, where calling triggers the configured nanoflow. You can specify parameters as a JavaScript object, and capture the return value of the nanoflow once execution finishes. For example, you can call a nanoflow that has a string `Name` parameter and returns a `User` object with this given name: `const user = await nanoflowParameter({ Name: "John Doe" });`. |
|   Microflow (introduced in Studio Pro 10.21.0 )  |   The microflow parameter type allows you to pass a microflow that you can call from your JavaScript action. The value of the parameter is an async function, where calling triggers the configured microflow. You can specify parameters as a JavaScript object, and capture the return value of the microflow once execution finishes. For example, you can call a microflow that has a string `Name` parameter and returns a `User` object with this given name: `const user = await microflowParameter({ Name: "John Doe" });`. Calling a microflow from a JavaScript action with a microflow parameter uses a [runtime operation](/refguide10/communication-patterns/#RO) and is [strict mode](/refguide10/strict-mode/) compliant. |
|   Boolean   |   The Boolean parameter type allows you to pass a Boolean value to a JavaScript action.  |
|   Date and Time   |  The date and time parameter type allows you to pass a date and time value to a JavaScript action. In the generated JavaScript action code, this type will be represented as a JavaScript `Date`.  |
|   Decimal   |  The decimal parameter type allows you to pass a decimal value to a JavaScript action. In the generated JavaScript action code, this type will be represented as a [Big](https://www.npmjs.com/package/big-js) object.  |
|   Enumeration   |  The enumeration parameter type allows you to pass an enumeration value to a JavaScript action. In the generated JavaScript action code, this type will be represented as a string.  |
|   Integer/Long   |  The integer/long parameter type allows you to pass a decimal value to a JavaScript action. In the generated JavaScript action code, this type will be represented as a [Big](https://www.npmjs.com/package/big-js) object.  |
|   String   |  The string parameter type allows you to pass a string value to a JavaScript action. |

#### Category

Use categories to keep parameters apart in a [JavaScript Action Call](/refguide10/javascript-action-call/). Categories are useful for making logical groups of parameters when your app has several parameters. If you do not specify a category, the parameter will appear in the **Input** group.

#### Description

For apps with several parameters, descriptions serve as useful reminders of parameters' exact purposes. Descriptions also allow you to describe your parameters to app collaborators. Descriptions may contain both upper- and lower-case letters, numbers, and symbols.

#### Required

If a parameter is set to **Required**, a value must be selected for that parameter in every JavaScript action call. In order to make a parameter optional, set **Required** to **No**. This makes it possible to call the JavaScript action without any value set for the parameter. This can make JavaScript actions more flexible and backwards compatible.

If no argument is provided for an optional parameter, it defaults to `undefined` in the JavaScript action. You can handle an optional parameter within the JavaScript action by checking if it is `undefined`, allowing you to assign a default value or implement custom logic as needed. 

### Return

Your JavaScript action can return different data types to your app.

#### Return Type

The return parameter type determines the type of data a JavaScript action returns. Because many APIs are asynchronous, you can also return a `Promise` object which resolves to this type. The return value of the JavaScript action can be given a name and stored so it can be used in the nanoflow where it is called. For all types which you can use for parameters, you can also use a return type. In addition, you can use the return type 'Nothing' if no data should return from the action.

#### Variable Name

This setting allows you to give a name to the JavaScript action's return value if a return type is selected. This name is used when you drag the action into a nanoflow. The default value is set to **ReturnValueName**.

### Platform {#platform}

JavaScript actions can be for a specific platform, they have an optional platform property with the following values:

* All *(default)*
* Web – can be used in a browser or web app
* Native – can be used in a native mobile app

When using a JavaScript action for a specific platform in a nanoflow, it will restrict the platform for that nanoflow. For example, only native pages can be opened in a nanoflow that contains a JavaScript action where the platform is set to **Native**.

## Type Parameter

A type parameter is a placeholder for an entity type which will be filled with a specific entity when called in a nanoflow. Type parameters can be used when configuring the data type of a parameter, which allows users to pass an object or list of an arbitrary entity type. They can easily be added, edited, or deleted:

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/type-parameter.png" alt="type parameter" class="no-border" >}}

A JavaScript action can have zero or more type parameters. Each type parameter should have a unique name.

## Expose as Nanoflow Action

In the **Expose as nanoflow action** tab, it is possible to expose a JavaScript action as a nanoflow action. This example action has been given *Example Action* caption text, assigned *Workshop* as its category, and given no icon or image:

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/expose-jsaction.png" alt="expose action" class="no-border" >}}

When the **Expose as nanoflow action** option is selected, the JavaScript will appear in the **Toolbox** of a [nanoflow editor](/refguide10/nanoflows/) in the category of your choice.  When this action is used in a nanoflow, it will show the caption and icon you provided. The category and caption are apparent here, and the default icon and image are being displayed as no custom icon and image were assigned: 

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/workshop-exposed.png" alt="workshop exposed" class="no-border" >}}

### Caption

A caption is required when exposing a JavaScript action. This caption will accompany your JavaScript action inside the nanoflow **Toolbox** window and can give helpful reminder information about your JavaScript action there.

### Category

A category is required when exposing a JavaScript action. Use categories to organize JavaScript actions with similar purposes together in the nanoflow **Toolbox** window.

### Icon

The **Icon** property is optional when exposing a JavaScript action. The image in the **Icon** property is used for the list view of the **Toolbox**. For more information, see the [Toolbox](/refguide10/view-menu/#toolbox) section in the *View Menu*.

When no icon is selected, the default JavaScript action icon is used. The required icon size is 64x64 pixels; the required icon format is PNG. 

A separate icon can be provided for the [dark mode](/refguide10/preferences-dialog/#studio-pro-theme) of Studio Pro to fit its color scheme.

### Image

The **Image** property is optional when exposing a JavaScript action. The image in the **Image** property is used for the toolbox tile view. For more information, see the [Toolbox](/refguide10/view-menu/#toolbox) section in the *View Menu*.

When no image and no icon is selected, the default JavaScript action image is used. Otherwise the provided *icon* image is used. 

The required image size is 256x192 pixels; the required the image format is PNG. 

A separate image can be provided for the [dark mode](/refguide10/preferences-dialog/#studio-pro-theme) of Studio Pro to fit its color scheme.

## Documentation

In the **Documentation** tab, press **Edit** to document a JavaScript action: 

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/documentation-pro.png" alt="documentation"   width="450"  class="no-border" >}}

Documentation is visible in the **Code** tab. Your documentation also is copied into the JavaScript action as comment on the function in the corresponding *.js* file:

{{< figure src="/attachments/refguide10/modeling/resources/javascript-actions/documentation-js-file.png" alt="documentation js file"   width="450"  class="no-border" >}}

## Code

In the **Code** tab, you can edit the JavaScript action code without leaving Studio Pro. The editor is based on the [Monaco Editor](https://microsoft.github.io/monaco-editor/index.html). It offers features such as syntax highlighting and code completion. The code can be written in modern JavaScript (ES8 / ES2017) and can use functions like `async` with `await` and `Promise`.

The code has three sections: an import list, an extra code block, and a user code block. All code that is added should go in one of these blocks. Code outside the blocks will lost when re-generating the template code on deploy or update of the JavaScript action settings. 

Additional imports should start with `import` and be placed above `// BEGIN EXTRA CODE`. Extra code should be placed between `// BEGIN USER CODE` and `// END USER CODE`. User implementation code should be placed between `// BEGIN EXTRA CODE` and `// END EXTRA CODE`.

``` js
// This file was generated by Mendix Studio Pro.
//
// WARNING: Only the following code will be retained when actions are regenerated:
// - the import list
// - the code between BEGIN USER CODE and END USER CODE
// - the code between BEGIN EXTRA CODE and END EXTRA CODE
// Other code you write will be lost the next time you deploy the app.
import { Big } from "big.js";

// BEGIN EXTRA CODE
 function sayHello(message) {
     window.alert("Hello: " + message);
 }
// END EXTRA CODE

/**
 * Show an alert message to an user.
 * @param {string} message - Message shown to the user.
 * @returns {Promise.<void>}
 */
export async function Hello(message) {
	// BEGIN USER CODE
	sayHello(message);
	return Promise.resolve();
	// END USER CODE
}
```

## Read More

* [JavaScript Action Call](/refguide10/javascript-action-call/)
* [Nanoflows](/refguide10/nanoflows/)
* [Build JavaScript Actions](/howto10/extensibility/build-javascript-actions/)
* [Java Action Call](/refguide10/java-action-call/)
* [Microflow Call](/refguide10/microflow-call/)
