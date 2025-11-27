---
title: "GenAI Resources"
url: /control-center/genai-resources-self-service
description: "Describes how to provision and deprovision GenAI Resources using self-service."
weight: 20
---

## Introduction

The GenAI Resources section provides a detailed overview of all Mendix GenAI resources available within your company. This feature enables users to seamlessly provision and deprovision GenAI resources as needed. With this feature, company admins can efficiently manage all GenAI resources directly within the [Control Center](https://controlcenter.mendix.com/index.html) through a self-service capability, ensuring streamlined operations and improved governance. For more information, see [Accessing GenAI Resources](link).

## Prerequisites

Self-service provisioning of GenAI resources using Mendix Cloud tokens is available only to users who meet the following conditions:

1. Sufficient token entitlements: The user should have an adequate number of available Mendix Cloud token entitlements to allocate for GenAI resource provisioning.
2. Valid subscription plan: The user's Mendix subscription must be based on the FY21 or newer price list. Older subscription plans are not eligible for provisioning.
3. Single account ownership: Users should have a single account. Accounts with multiple accounts are also not supported for self-service GenAI provisioning.
4. Enterprise platform subscription: The user should have a single active enterprise platform subscription. If no active subscription is found, the system will display the warning message below:

{{< figure src="/attachments/control-center/genai-resources/warning-message.png" >}}

## Overview of Deployed Resources

The overview page provides a centralized view of all deployed GenAI resources, including text generation resources, embeddings generation resources, and knowledge base resources. From this page, you can easily review the status, basic information, and usage details of each deployed resource. The list below shows detailed information about your GenAI resource.

Status: shows the current status of the resource.
Name: indicates the name of the resource.
Model: indicates which model is used, for example, Anthropic Claude Sonnet 4.0.
Plan: indicates the subscription plan used for resources (for example, small, medium, or large).
Created for: for whom it is created.

{{< figure src="/attachments/control-center/genai-resources/overview-genai-resources.png" >}}

## Provisioning GenAI Resources

You can provision any GenAI resources directly within the Control Center using self-service capability. 
To do so, select the appropriate resource type and click **Provision Resource**. 

{{% alert color="info" %}}
Ensure that you are in the correct resource tab before provisioning. For example, to create a new text generation resource, first select the **Text Generation Resources** section.
{{% /alert %}}

When provisioning a new resource, enter the following information:

Display Name: indicates the name of the resource.
Environment: specifies the environment for which the resource is created, such as test, acceptance, or production.
Mendix Cloud Region: indicates the cloud region where the resource will be hosted.
Cross-region inference: specifies whether the selected model supports cross-region inference. For more information, see the [Settings](/appstore/modules/genai/mx-cloud-genai/Navigate-MxGenAI/#settings) section of *Navigate through the Mendix Cloud GenAI Portal*.
Available Text Generation Modes: lists the supported models you can choose from, for example, Anthropic Claude Sonnet V4.
Size: indicates the subscription plan with the tokens used for resources. 
User: the name of the user for whom the provisioning was initially created.
Email: the email address of the user.

After completing the required fields, you can review all entered details in the **Resource Specification**. To learn more, see [Mendix Cloud GenAI Resource Packs](/appstore/modules/genai/mx-cloud-genai/resource-packs/).

Click **Provision Resource** to finalize the process. You will return to the **GenAI Resources** page, where the newly created resource will appear in the list. Selecting the newly provisioned resource will open its details directly in the Mendix Cloud GenAI Portal in a new tab.

## Deprovisioning GenAI Resources

If you want to deprovision the resource, click the three dots ({{% icon name="three-dots-menu-horizontal" %}}) icon next to the selected resource and select **Deprovision Resource**. 
A confirmation pop-up will appear displaying a message and the details of the selected resource, as shown in the example below.

{{< figure src="/attachments/control-center/genai-resources/deprovisioning.png" >}}

Click **Deprovision** to proceed. After confirmation, the resource status will update on the **GenAI Resource** page to reflect that deprovisioning is scheduled. 

{{% alert color="info" %}}
Your subscription plan operates on a monthly bundle cycle. When you deprovision a resource, the actual deprovisioning will occur at the end of the current subscription month. Until that date, you can still use the resource, and the scheduled deprovisioning date will appear in the resource's **Status**. 
{{% /alert %}}
