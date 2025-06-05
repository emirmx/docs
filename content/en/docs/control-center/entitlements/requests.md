---
title: "Requests"
url: /control-center/requests/
description: "Describes the Requests page in the Mendix Control Center."
weight: 30
no_list: false 
beta: true
---

## Introduction

The Technical Contact can request a plan change for an app environment. To do that, they need to click the **Change Plan** button on the environment in the **Apps** section of Mendix Portal. For more information, refer to [Changing Your Plan in Mendix Cloud](/developerportal/deploy/change-plan/).    

The **Requests** page displays all plan change requests, along with the actions you can take for each request. 

## Request Details

These are the details shown for each request:

* **Request Type** – The type of request being submitted.    
    The only available option is **Plan Upgrade**.
* **App Name** – The name of the app for which the request is submitted.
* **Environment** – The app environment for which the request is submitted.
* **Production** – This column displays a green checkmark if the environment for which the request is submitted is production.
* **Current Plan** – The plan that the environment is currently on.
* **Requested Plan** – The plan that the environment should be moved to.
* **Submitted On** – The date when the request was submitted.
* **Status** – The current status of the request, which can be one of the following:

    * **Pending Approval**
    * **Approved**
    * **Canceled**
    * **Rejected**
    * **Expired**
* **Action** – Click the **Details** button next to the request to access further details, such as the cost of the plan change and the reason for the request.

You can filter requests by status and type.

{{% alert color="info" %}} 
You can no longer purchase Legacy Cloud Resources Packs. You can now only purchase and provision Standard, Premium, and Premium Plus CRPs. Any legacy Cloud Resource Packs that you have already purchased will be converted into Mendix Cloud Tokens if they are deprovisioned. This will use the rate specified in the [Cloud Resource Packs](/control-center/cloud-tokens/#crps) section of *Cloud Tokens*, and the Mendix Cloud Tokens will be added to your Token pool.
{{% /alert %}}

## Approving a Request

Follow these steps to approve a request:

1. Click **Approve** in the request details window.
2. Click **Approve** again in the confirmation window that opens.

Once a request is approved, its status changes to **Approved**.

For the Technical Contact, the status changes to **Pending Schedule** on the [Request Overview tab](/developerportal/deploy/environments/#request-overview) of the **Environments** page. They then need to specify when the plan change should take effect. For more information, refer to the [Scheduling a Plan Change](/developerportal/deploy/change-plan/#scheduling-a-plan-change) section in *Changing Your Plan in Mendix Cloud*.

## Rejecting a Request

Follow these steps to reject a request:

1. Click **Reject** in the request details window.
2. Provide a reason for the rejection in the confirmation window that opens.
3. Click **Reject** again.

Once a request is rejected, its status changes to **Rejected**.    
The Technical Contact can see the same status on the **Request Overview** tab of the **Environments** page.
