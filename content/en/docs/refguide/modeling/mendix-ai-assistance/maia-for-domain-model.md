---
title: "Maia for Domain Model"
url: /refguide/maia-for-domain-model/
weight: 50
description: "Describes the features in Maia for Domain Model."
aliases:
    - /refguide/domain-model-generator/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

{{% alert color="info" %}}
To use Maia for Domain Model, an internet connection and signing in to Studio Pro are required.
{{% /alert %}}

Maia for Domain Model is an AI-powered tool that you can use for generating new [domain model](/refguide/domain-model/) as well as explaining and providing suggestions for existing domain models. It has some limitations. For more information, see the [Limitations](#limitation) section below.

## Using Maia for Domain Model

Maia for Domain Model is enabled by default.

You can find it in the the toolbar of a domain model:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/maia-for-domain-model/maia-for-domain-model.png" max-width=80% >}}

If you want to disable this feature, go to **Edit** > **Preferences** > the **New Features** tab > the **Maia** section.

After clicking **Maia for Domain Model**, a dedicated chat interface will appear at the right side of Studio Pro under the **Maia** tab:

{{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/maia-for-domain-model/chat-interface.png" max-width=42% >}}

Describe your application or its main goals in the chat. Maia will use this information to generate a relevant domain model for you. After a domain model is generated and if the dedicated chat is still active, you can continue asking Maia to add more entities and associations to the generated domain model. It also supports adding more attributes to an existing entity in the generated domain model.You can also ask Maia to explain or improve the generated domain model through further requests.

{{% alert color="info" %}}
Maia for Domain Model does not support deleting items in the domain model. You can always delete them manually. 
{{% /alert %}}

{{% alert color="info" %}}
In this dedicated chat, only requests related to domain model will be properly handled. If you have other questions, close this chat and go back to the general [Maia Chat](/refguide/maia-chat/) interface.
{{% /alert %}}
 
### Best Practices for Text Input

To achieve optimal results, provide context about your application by describing its main use cases, customer needs, or other relevant details. The more Maia knows about your application, the more tailored and accurate the generated domain model will be.

Below are some examples of prompts you can use as a starting point:

* The app will help users ...
* This module handles ...
* Customers need to be able to ...
* I want to send notifications when ...
* The app will analyze and report on ...
* This feature allows users to ...

Below are examples of prompts you can use to ask Maia for explanations or suggestions to improve a domain model:

* Can you explain the function and purpose of this domain model?
* I want to add an attribute `ATTRIBUTE_NAME` to the entity `ENTITY_NAME`.
* I want to make the domain model more efficient. Can you suggest optimizations?
* Could you identify any potential issues with this domain model and recommend solutions?

## Limitations {#limitation}

Maia for Domain Model has some limitations.

### Enumeration Generation

It can only generate an attribute of type Enumeration if there is an existing enumeration in the module to refer to.

### Generalization Not Supported

It cannot set [generalization](/refguide/generalization-and-association/) for an entity.

## Read More

* [Data in the Domain Model](/refguide/domain-model/)
* [Mendix AI Assistance (Maia)](/refguide/mendix-ai-assistance/)
* [Maia Chat](/refguide/maia-chat/)
