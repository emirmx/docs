---
title: "Company Settings"
url: /control-center/company-settings/
description: "Describes the Company Settings page in the Mendix Control Center."
weight: 20
no_list: list
---

## Introduction

The **Company Settings** page in Control Center allows you to change company details, company email domains, and security contact. Moreover, you can view and add Mendix Admins of your company.

## General Settings

On the **General Settings** tab of this page, you can manage your company details, company email domains, and security contact.

### Company Details

Under **Company Details**, you can edit your **Company Name** and **Description**.

### Company Email Domains {#company-email-domains}

{{% alert color="warning" %}}Once you add an email domain, it is permanently added to your company and cannot be removed anymore. This comes into effect as soon as a user creates a Mendix account with this email domain, whether the Mendix account is active or not.{{% /alert %}}

Under **Company Email Domains**, you can request new company email domains by clicking **Add Domain**, upon which a request is sent to [Mendix Support](/support/). Once a domain is added, every user who signs up to Mendix with that email domain will be assigned to your company, all existing users with this domain will be moved to your company, and their applications will also be moved to the new company.

If you have [configured single sign-on using BYOIDP](/control-center/security/set-up-sso-byoidp/), BYOIDP will automatically adopt it, without further actions from the Mendix Admins.

To export all the email domains, use the **Export to Excel** button.

{{< figure src="/attachments/control-center/company/company-settings/company-email-domains.png" width="650px" class="no-border" >}}

#### Merging Companies

In certain scenarios, you may want to ask Mendix to merge two companies into one on the Mendix Platform, such as when your company purchases another company which also uses the Mendix Platform. This can be done, but the action is irreversible. After the companies are merged, all users and their applications from both companies are consolidated under the same company.

Once user accounts are transferred to the new company, users retain access to private Marketplace content, as this content is also transferred. They can still download previously published private components after the transfer.    
However, content is removed from the original organization’s content groups. Users need to recreate these content groups within the new organization to manage the content as before.

### Security Contact {#security-contact}

Under **Security Contact**, you can provide a specific security contact who is informed if there are critical security issues with the Mendix Platform and platform-supported Marketplace components. Mendix strongly recommends applying a team email address or a functional mailbox instead of a personal individual email address.

After you click **Add security contact** and add the required **Name** and **Email** address, a confirmation email is sent to the proposed contact. The recipient has seven days to click the link in the email and validate the Security Contact request. After seven days, the pending request is automatically removed. If the recipient has not received the confirmation email but is still within the seven-day validation timeframe, you can click **Resend confirmation email**.

For an existing security contact, there are two menu options available:

{{< figure src="/attachments/control-center/company/company-settings/security-contact.png" class="no-border" >}}

Click **Edit** to edit the Security Contact's **Name**.

Click **Delete** to delete the existing Security Contact. This is the first step in changing a validated Security Contact. After you make the deletion, you can add the new Security Contact.

For more information on security issues, see [Security Advisories](/releasenotes/security-advisories/).

## Mendix Admins{#mendix-admins}

The **Mendix Admins** tab lists all the current Mendix Admins in your company. A Mendix Admin will normally be someone in the IT department of your company, and they will have full access to Control Center to perform all available tasks. 

To add a new admin, click **Add Mendix Admin**. To remove an admin, hover over their name in the list and click **Remove**.

{{< figure src="/attachments/control-center/company/company-settings/admin.jpg" class="no-border" >}}

{{% alert color="info" %}}
It is not possible to remove yourself as a Mendix Admin.
{{% /alert %}}

If the **Add Mendix Admin** button is not shown, your company is using a user group in your IdP to assign Mendix Admins. For more information, see [IdP-managed Mendix Admins](/control-center/security-settings/#idp-managed-mendix-admins).
