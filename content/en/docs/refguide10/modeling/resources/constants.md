---
title: "Constants"
url: /refguide10/constants/
weight: 60
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Constants are used to define configuration values. These can differ per environment.

When running the application on a licensed Mendix Cloud environment, SAP BTP, or Mendix on Kubernetes you can configure the constant values for each environment separately using the [Model Options](/developerportal/deploy/environments-details/#model-options) tab of the **Environment Details** page to set your constants.

For other cloud environments – for example, Siemens [Insights Hub](/partners/siemens/mindsphere/) – the constants can be accessed as **Environment Variables** in, for instance, Cloud Foundry. The constant is exposed with the name **module** + **.** + **constant** (for example, `mymodule.myconstant`).

When running the application locally or in a Free App environment, the values defined in Studio Pro are used.

{{% alert color="info" %}}
The value for a constant can also be overridden in a [configuration](/refguide10/configuration/). This allows you to run locally using different values for one or more constants, without having to change the default value for the constant every time.
{{% /alert %}}

Constants can be used in the following:

* [Expressions](/refguide10/expressions/) – by prefixing the full name of the constant with `@`
* [Consumed web services](/refguide10/consumed-web-services/) – in this case, the constant is a URL that specifies where the web service is located; this can vary based on the environment in which the application is running, so that you can, for example use different web services for development and production

## Common Properties

### Name

The name of the constant. This name is used to refer to it.

### Export Level 

{{% alert color="info" %}}

This property is only available for add-on and solution modules. For more information on types of modules, see the [Module Types](/refguide10/modules/#module-types) section in *Modules*. 

{{% /alert %}}

**Export level** allows you to define access level to this document on the consumer (customer) side when developing an add-on module or a solution. 

| Value              | Description                                             |
| ------------------ | ------------------------------------------------------- |
| Hidden *(default)* | The document/element content is hidden from a consumer. |
| Usable             | Consumers can see the constant and use it in their app. |

### Documentation

This field is for documentation purposes only: end-users will never see it, and it does not influence the behavior of your application

## Type Properties

### Type

The [data type](/refguide10/data-types/) of the constant. This determines what kind of values a constant can hold. Supported data types are string, Boolean, date and time, decimal, and integer/long.

## Value Properties

### Default Value {#default-value}

This property is the default value of the constant. This value is used when running locally or in a Free App environment. When running locally, the value can be overridden in the currently selected [configuration](/refguide10/configuration/).

### Exposed to Client

This property defines whether the constant is accessible from client-side expressions (expressions in [nanoflows](/refguide10/nanoflows/) and [pages](/refguide10/pages/)).

| Option | Description |
| --- | --- |
| Yes | The constant will be sent to the client and will be accessible from client-side expressions |
| No *(default)* | The constant will not be sent to the client and will be only accessible from [microflow](/refguide10/microflows/) expressions |

{{% alert color="warning" %}}
When a constant is exposed to the client, Mendix Runtime sends its value to the client so that in addition to microflow expressions, it will also be accessible from nanoflows and page expressions. This means that you should not use sensitive data or secrets such as passwords when a constant is exposed to the client.

For a web app, changes to a constant's values are reflected when the end-user refreshes the browser or restarts the app. For an offline-first PWA or native application, the app stores the constants' values for offline use. The app updates the constant's values in the following cases:

* When an end-user logs in or logs out in the app
* When you deploy a new version of the app that contains domain model changes used in the offline-first app
{{% /alert %}}
