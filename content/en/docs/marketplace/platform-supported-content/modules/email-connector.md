---
title: "Email Connector"
url: /appstore/modules/email-connector/
description: "Describes the configuration and usage of the Email Connector module, which is available in the Mendix Marketplace."
aliases:
    - /appstore/connectors/email-connector/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
#ki: DHC-2535
---

## Introduction

The [Email Connector](https://marketplace.mendix.com/link/component/120739) allows you to send and receive emails using your own email server. It supports features like template-based emails, digital signatures, and encrypted email sending.

### Features

The Email Connector includes the following capabilities:

* Configuration of multiple email accounts
    * Support for Basic Authentication or OAuth 2.0 (Authorization Code Flow or Client Credentials Flow) for Microsoft Entra ID (formerly Azure Active Directory) accounts
    * Support for shared mailboxes with both Basic Authentication and OAuth 2.0
* Digital signatures and encryption
* Email templates
* Sending and receiving emails using OAuth 2.0 Authorization Code Grant or Client Credentials Flow

The Email Connector supports the following protocols:

* POP3 and POP3S
* IMAP and IMAPS
* SMTP

### Prerequisites {#prerequisites}

{{% alert color="warning" %}}
Follow these prerequisites carefully. Missing a step can lead to errors.
{{% /alert %}}

Before using the Email Connector in your app, make sure to complete the following:

* Download and configure the latest version of the [Mx Model Reflection](/appstore/modules/model-reflection/) module.
* Download and configure the latest version of the [Encryption](/appstore/modules/encryption/) module.
* Download and configure the latest version of the [Community Commons](/appstore/modules/community-commons-function-library/) module.
* Uninstall any previously installed email modules, such as [IMAP/POP3](https://marketplace.mendix.com/link/component/1042/) and [Email Module with Templates](https://marketplace.mendix.com/link/component/259/).
* Remove any unused JAR files from older email modules (for example, *javax.mail-1.6.2.jar*, *activation-1.1.jar*, and *commons-email.jar*) that may still be present in the userlib folder.
* [Clean the deployment directory](/refguide/app-menu/#clean-deployment-directory).

### Migrating from Another Module

When migrating to the Email Connector from a different email module, it is recommended to test your configuration in a separate app before applying it to your main project.

It is recommended to use the community-supported [Email Connector Migration Utility](https://marketplace.mendix.com/link/component/205008) module to migrate data from the [Email Module with Templates](https://marketplace.mendix.com/link/component/259/) service.

### Included Widgets {#included-widgets}

The module includes the following bundled widgets:

* [HTML Element](/appstore/widgets/htmlelement/)
* [Rich Text](/appstore/widgets/rich-text/)
* [Pop-Up Menu](/appstore/widgets/popup-menu/)

{{% alert color="info" %}}If you already have these widgets in your app and they are not up to date, you will get a "Some widgets can not be read" error.{{% /alert %}}

## Setup in Studio Pro {#setup}

To launch the user interface, add the **Email_Connector_Overview** page located in the **USE_ME > Pages** to your app navigation.

### Configuring Roles

The module includes a default **EmailConnectorAdmin** module role with pre-configured access rights for common use cases. Review and verify that the access rights align with your specific requirements and security policies before assigning this module role to user roles in [App Security](/refguide/app-security/).

## Send Email {#send-email}

After launching your Studio Pro application, you can begin setting up your **Send Email** accounts through the Email Connector user interface.

### Authentication Methods

Configuration supports two authentication methods: 

* **Basic Authentication** 
* **OAuth 2.0**

### Account Setup

#### Enable Microsoft Entra ID Authentication

* **Yes** – Enables OAuth 2.0 authentication through Microsoft Entra ID
* **No** (*default*) – Uses basic authentication with username and password 

#### Basic Authentication

##### Primary Account Details

* **Display Name** – The name shown to email recipients
* **Email or Username** – The email address or username for the sending account
* **Password** – The account password for basic authentication; the field is masked for security

##### Configure Shared Mailbox

This is an optional feature for setting up shared mailbox access. It requires using the primary account details for configuration. Enable this feature by clicking the checkbox **Shared mailbox**.

##### Email Protocol Settings

**Email Protocol** currently supports SMTP for outbound email transmission.

**Server Configuration** 

* **Server Host** – The SMTP server hostname or IP address 
* **Server Port** – The port number for SMTP communication

**Security Options** 

* **SSL** – Enable Secure Sockets Layer encryption 
* **TLS** – Enable Transport Layer Security encryption

#### OAuth Authentication

You can configure your account to authenticate with Microsoft Entra ID OAuth 2.0. Multiple OAuth 2.0 providers can be configured per app.

To manage configurations:

* Select the **Configure OAuth** tab to add, delete, and edit OAuth configurations
* If no email accounts are configured, you can create a new OAuth configuration

For detailed steps and implementation guidance, see the [OAuth Configurations](#oauth-config-details) section below.

### Sending Email

Use the **SUB_SendEmail microflow** for standardized, Mendix-compliant email delivery with proper error handling and configuration management.

When working with email templates, refer to the following sample microflows:

* **Sample_ACT_CreateEmailFromTemplateAndThenSend** – Demonstrates creating emails from templates with additional customization
* **Sample_ACT_SendEmailWithTemplate** – Shows direct email sending using predefined templates

{{% alert color="info" %}}
It is recommended to use **Sample_ACT_SendEmailWithTemplate** for most email template scenarios. It provides a streamlined implementation for sending templated emails with minimal configuration overhead.
{{% /alert %}}

### Additional Account Settings

#### Server Connection Settings

* **Use SSL check server identity** – Optional security feature to verify server identity during SSL connections and enhance connection security by validating server certificates
* **Connection Timeout** – Configure the maximum time to wait for server connections; default is 20000 milliseconds (20 seconds)

#### Send Email Configuration

* **Full Name** – The display name for outgoing emails; this name appears in the **From** field of sent messages
* **Max. send attempts** – Maximum number of retry attempts for failed email sends; default is 0 (no retry attempts)

#### Digital Signature Settings

* **Configure Digital Signature** – Enable to digitally sign outgoing email messages during send email actions; once configured,it provides message authentication and integrity verification
* **Certificate (PKCS#12)** – Upload a PKCS#12 certificate file for digital signing; this supports standard PKCS#12 format certificates
* **Passphrase** – Required to access the PKCS#12 certificate; this field is mandatory when using certificate-based digital signatures

#### Email Encryption Settings

* **Configure Email Encryption** – Enable to encrypt outgoing email messages during send email actions; it provides additional security for sensitive email communications
* **LDAP Configuration**  
    * **LDAP Host** – LDAP server hostname for certificate lookup
    * **LDAP Port** – LDAP server port (default: 389)
* **Authentication Method** – The system supports two authentication methods for LDAP access:
    * **No Authentication** – Connect to LDAP server without credentials
    * **Basic Authentication** – Use username and password for LDAP server access

## Receive Email {#receive-email}

After launching your application, you can set up your **Receive Email** accounts through the Email Connector user interface. 

### Authentication Methods 

Configuration supports two authentication methods:

* **Basic Authentication** 
* **OAuth 2.0**

### Account Setup

#### Enable Microsoft Entra ID Authentication

* **Yes** – Enables OAuth 2.0 authentication through Microsoft Entra ID
* **No** (*default*) – Uses basic authentication with username and password 

#### Basic Authentication

##### Primary Account Details

* **Display Name** – The name shown to email recipients
* **Email or Username** – The email address or username for the sending account
* **Password** – The account password for basic authentication; the field is masked for security

##### Configure Shared Mailbox

This is an optional feature for setting up shared mailbox access. It requires using the primary account details for configuration. Enable this feature by clicking the checkbox **Shared mailbox**.

##### Email Protocol Settings

**Email Protocol** – Choose from available email protocols for receiving emails:

* **IMAP** – Internet Message Access Protocol for email retrieval and management
* **IMAPS** –  IMAP over SSL/TLS for secure email access
* **POP3** – Post Office Protocol for email downloading
* **POP3S** – POP3 over SSL/TLS for secure email downloading

**Server Configuration** 

* **Server Host** – The hostname or IP address of the incoming mail server
* **Server Port** – The port number for the email protocol:
    * **IMAP** – Port 143 (non-encrypted) or Port 993 (SSL/TLS)
    * **IMAPS** – Port 993 (SSL/TLS encrypted)
    * **POP3** – Port 110 (non-encrypted) or Port 995 (SSL/TLS)
    * **POP3S** – Port 995 (SSL/TLS encrypted)

{{% alert color="info" %}} It is recommended to use encrypted ports (993 for IMAPS, 995 for POP3S) for enhanced security and data protection. {{% /alert %}}

#### OAuth Authentication

You can configure your account to authenticate with Microsoft Entra ID OAuth 2.0. Multiple OAuth 2.0 providers can be configured per app.

To manage configurations:

* Select the **Configure OAuth** tab to add, delete, and edit OAuth configurations
* If no email accounts are configured, you can create a new OAuth configuration

For detailed steps and implementation guidance, see the [OAuth Configurations](#oauth-config-details) section below.

### Additional Account Settings

#### Server Connection Settings

* **Use SSL check server identity** – Optional security feature to verify server identity during SSL connections and enhance connection security by validating server certificates
* **Connection Timeout** – Maximum time to wait for server connections; default is 20000 milliseconds (20 seconds)

#### Receive Email Configuration

* **Folder to replicate emails from** – Specify the email folder to monitor for incoming messages; default is "INBOX"
* **Subscribe to incoming emails** – Enable real-time monitoring for new email arrivals
* **Number of emails to retrieve from server** – Set the maximum number of emails to fetch in a single operation; default is 50
* **Fetch strategy** – Controls the order in which emails are retrieved from the server 
    * **Latest** – Retrieves most recent emails first 
    * **Oldest** – Retrieves oldest emails first

#### Server Email Management

##### Email handling on server after replication

This setting determines what happens to emails on the server after they have been replicated to your Mendix application.

* **None**
    * Leaves emails unchanged on the server after replication 
    * Suitable for read-only processing or multi-system access
* **Remove original emails from server** 
    * Permanently deletes emails after replication 
    * Helps manage server storage space 
  
{{% alert color="warning" %}} Use this setting with caution, as this action is irreversible. {{% /alert %}}

* **Move emails on server to another (existing folder)**
    * Transfers processed emails to a different folder 
    * Maintains email history while organizing processed messages 
    * Requires specifying an existing target folder

##### Email Security and Processing

* **Inline image rendering (HTML emails will have images visible in browser)** 
    * Controls how HTML email images are displayed 
    * When enabled, images embedded in HTML emails are rendered in the browser 
    * Enhances readability but may have security implications
* **Sanitize email to prevent XSS attacks** 
    * Enables security filtering to prevent cross-site scripting attacks 
    * Removes potentially malicious scripts and content from email messages 

## OAuth Configurations {#oauth-config-details}

Configure your email account to authenticate using Microsoft Entra ID OAuth 2.0. Multiple OAuth 2.0 providers can be configured within a single application.

The Email Connector supports both:

* OAuth 2.0 [Authorization Code Flow](#auth-code-flow)
* [Client Credentials Flow](#{#client-credentials-flow}) for Microsoft Entra ID (formerly Azure Active Directory) accounts

### OAuth Provider Configuration Details

To configure an OAuth provider for the Authentication Code Flow, provide the following details:

* **Client ID** – Application identifier obtained from [Microsoft Entra ID](https://portal.azure.com/) after app registration
* **Client Secret** – Authentication key generated during Microsoft Entra ID app registration
* **Callback Path** – Custom string used to autogenerate the callback URL
* **Callback URL** – **Redirect URI** where the OAuth provider returns after authorization

{{% alert color="info" %}} 
When deploying [on premises](/developerportal/deploy/on-premises-design/) on [Microsoft Windows](/developerportal/deploy/deploy-mendix-on-microsoft-windows/), add the following rule to the *web.config* file:

```
<rule name="mxecoh">
<match url="^(mxecoh/)(.*)" />
<action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
</rule>
```

For more information, see the [Reverse Proxy Inbound Rules](/developerportal/deploy/deploy-mendix-on-microsoft-windows/#reverse-proxy-rules) section of *Microsoft Windows*. 
{{% /alert %}}

To configure an OAuth provider for the **Client Credentials Flow**, provide the following details from Microsoft Entra ID after app registration:

* **Client ID** - Application identifier from your registered app
* **Client Secret** - Authentication key generated for your application
* **Tenant ID** - Directory identifier for your Microsoft Entra ID tenant

With the Email Connector version 5.2.0 and above, you can send emails using the Client Credentials Flow.

### Settings in the Microsoft Entra ID (Authentication Code Flow) {#auth-code-flow}

Follow Microsoft's tutorial [Register an app with Microsoft Entra ID](https://docs.microsoft.com/en-us/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory) to register your app on the Microsoft Entra ID. As mentioned above in [OAuth Provider Configuration Details](#oauth-config-details), make sure to set the **Redirect URI** as the **Callback URL**.

This connector contains functionality for sending and receiving emails, so during the OAuth process, the connector will ask for permissions for sending and receiving emails.

#### API Permissions

On the [Microsoft Entra ID](https://portal.azure.com/), ensure you have the following permissions enabled under the **API permissions** tab on the sidebar.

Depending on your use case, modify the **azure_defaultConfig** constant to specify the required OAuth scopes for your application.

##### Send Emails 

| Permission Name | Description |
|-----------------|-------------|
| SMTP.Send       | Send emails from mailboxes using SMTP AUTH. |
| User.Read       | Sign in and read user profile (required for authentication). |
| openid          | Sign users in (required for OAuth/OpenID Connect). |
| offline_access  | Maintain access to data you have given it access to (for refresh tokens). |
| profile         | View users' basic profile (often used during sign-in). |
| email           | View users' email address (optional but helpful). |

##### Receive Emails 

| Permission Name          | Description |
|--------------------------|-------------|
| IMAP.AccessAsUser.All    | Read and write access to mailboxes via IMAP. |
| POP.AccessAsUser.All     | Read and write access to mailboxes via POP. |
| User.Read                | Sign in and read user profile (required for authentication). |
| openid                   | Sign users in (required for OAuth/OpenID Connect). |
| offline_access           | Maintain access to data you have given it access to (for refresh tokens). |
| profile                  | View users' basic profile (often used during sign-in). |
| email                    | View users' email address (optional but helpful). |

### Settings in the Microsoft Entra ID (Client Credentials Flow) {#client-credentials-flow}

Follow Microsoft's [Register an app with Microsoft Entra ID](https://docs.microsoft.com/en-us/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory) to register your app in the Microsoft Entra ID portal.

This connector contains functionality for sending and receiving emails. APIs related to Office 365 Exchange Online need to be given permission along with admin consent.

#### API Permissions

On the [Microsoft Entra ID](https://portal.azure.com/), ensure you have the following permissions enabled under **API permissions** tab on the sidebar.

##### Send Emails (Application Permissions)

| Permission Name | Type        | Description                                     |
|-----------------|-------------|-------------------------------------------------|
| SMTP.SendAsApp  | Application | Sending email via SMTP AUTH.                    |

##### Receive Emails (Application Permissions)

| Permission Name    | Type        | Description |
|--------------------|-------------|-------------|
| IMAP.AccessAsApp   | Application | Read and write access to all mailboxes via IMAP. |
| POP.AccessAsApp    | Application | Read and write access to all mailboxes via POP. |

Admin status is given on the added API permissions. The tenant admin must register the Microsoft Entra ID application's service principal in Exchange via Exchange Online PowerShell, as described in [Register service principals in Exchange](https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth#register-service-principals-in-exchange).

## Email Templates

You can create and use templates with any email account.

### Creating an Email Template{#create-template}

Use the **SNIP_EmailTemplate_Overview** snippet located in **Email_Connector** > **Private** > **Snippets**. Add the snippet directly to a page and save it.

  {{% alert color="info" %}} The [Mx Model Reflection](/appstore/modules/model-reflection/) must be installed and properly configured in your app prior to creating placeholder tokens and before exporting/importing email templates containing placeholder tokens.{{% /alert %}}

### Creating an Email Message from a Template

Use the **CreateEmailFromTemplate** Java action to generate a draft email message that can be previewed and customized. Once finalized, send it using the **Send email** action. The input parameters are as follows:

* **DataObject** – Entity object from which placeholder tokens are extracted. To retrieve data from multiple objects, create a [non-persistable entity](/refguide/persistability/#non-persistable).
* **EmailTemplate** – Email template used to construct and send an **EmailMessage** object.
* **Queued** – When enabled (**true**), the email message is stored in the **EmailMessage** entity with a **Queued** status. This configuration enables later transmission via [scheduled events](/refguide/scheduled-events/). 

You can leverage the **SE_SendQueuedEmails** microflow to set up the necessary scheduled events. You can also create a [task queue](/refguide/task-queue/) and run the microflow in that task queue to minimize system resource usage. With a task queue, you can set the number of threads, node or cluster-wide scope, time intervals, and other parameters.

Review the demonstration microflow **Sample_ACT_CreateEmailFromTemplateAndThenSend** as a reference. This microflow demonstrates how to implement the **CreateEmailFromTemplate** Java action while managing attachments for the **EmailMessage**, incorporating both existing **EmailTemplate** attachments and supplementary files.

### Sending an Email with a Template

Use the **SendEmailWithTemplate** Java action to send an email from a template. The input parameters are as follows:

* **Data Object** – Entity object from which placeholder tokens will be extracted. To retrieve data from multiple objects, create a [non-persistable](/refguide/persistability/#non-persistable) entity.
* **EmailAccount** – Email account with the necessary send email configuration details.
* **EmailTemplate** – Email template used to construct and send an **EmailMessage** object.
* **Queued** – When enabled (**true**), the email message is stored in the **EmailMessage** entity with a **Queued** status. This configuration enables later transmission via [scheduled events](/refguide/scheduled-events/). 

You can leverage the **SE_SendQueuedEmails** microflow to set up the necessary scheduled events. You can also create a [task queue](/refguide/task-queue/) and run the microflow in that task queue to minimize system resource usage. With a task queue, you can set the number of threads, node or cluster-wide scope, time intervals, and other parameters.

Review the demonstration microflow **Sample_ACT_SendEmailWithTemplate** as a reference. For dynamic configuration of **To**, **CC**, or **BCC** fields, update the EmailTemplate object with the required attribute values and provide this modified EmailTemplate object as input to the Java action.

### Export Email Template

You can export an email template using the Email Connector to streamline the process of template recreation across various environments including development, acceptance, and production. Follow the steps below:

1. Choose the email template you want to export.
2. Select **Export** from the **Pop-up Menu**. 

An XML file will be generated with a filename that combines the template name and current datetime, and will be saved directly to your default downloads folder. The following image illustrates the downloaded XML file after completing the email template export.

### Import Email Template

An exported email template can be imported into the current deployment environment or a different one. To import, follow these steps:

1. Click **Import Template**.
2. Select the template file (*.xml*) you want to import.
3. Click **Import Template**.

## Troubleshooting

### Sending or Receiving Email

If you have issues sending or receiving emails:

1. Check the **Error logs** in **Account Settings**.
2. Review the logs in Studio Pro. 

If an email does not appear in your app and there is nothing in the logs, the problem is not with the connector.

### Gmail Accounts {#gmail-accounts}

Gmail no longer supports basic authentication (usernames and password). However, you may still be able to set up an account in the Email Connector by doing the following:

1. Read [Less secure apps & your Google Account](https://support.google.com/accounts/answer/6010255) and turn off the **Less secure app access** setting in your Google account.
2. Set up an app password to sign in to the Email Connector. For more information, see [Sign in with app passwords](https://support.google.com/accounts/answer/185833).

### Adding OAuth 2.0 Configuration to an App with Basic Authentication

If you already have an email account configured using basic authentication in your app, and you want to use OAuth 2.0 authentication without removing that email account, do the following:

1. On the overview page, click **OAuth Configurations** and add a new configuration. For more information, see [OAuth Provider Configuration Details](#oauth-config-details).
2. For the desired email account, set the **isOAuthUsed** attribute of the **EmailAccount** entity to **True**.
3. Associate the email account with your newly created OAuth provider.
4. Navigate to the overview page, click **Manage Accounts**, and select the account.
5. Go to the **Server Settings** tab in **Account Settings** and select **Re-authenticate Access**.

### Deprecation of Basic authentication in Microsoft Exchange Online

As of October 1, 2022, Microsoft has deprecated Basic Authentication, and it is no longer supported in Exchange Online, promoting Modern Authentication (OAuth 2.0) for enhanced security. From that date, Microsoft started disabling Basic Authentication for the following protocols:
 
* Outlook
* EWS
* RPS
* POP
* IMAP
* EAS

### Deploying to On-Premises Cloud Environments

When deploying [on premises](/developerportal/deploy/on-premises-design/) on [Microsoft Windows](/developerportal/deploy/deploy-mendix-on-microsoft-windows/), you need to add the following rule to the *web.config* file:

```
<rule name="mxecoh">
   <match url="^(mxecoh/)(.*)" />
   <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
</rule>
```

For more information, see the [Reverse Proxy Inbound Rules](/developerportal/deploy/deploy-mendix-on-microsoft-windows/#reverse-proxy-rules) section of *Microsoft Windows*.

### Configuring Local Email Clients

Configuring local clients, such as [Papercut](https://github.com/ChangemakerStudios/Papercut-SMTP), is supported. If you are using a tool like Papercut, do the following:

1. Follow the steps for [adding an email account](#adding-email-account).
2. Continue with manual configuration in the wizard (automatic configuration does not work for local clients).
3. Select the **Send emails** checkbox.
4. Select **SMTP** for the **Protocol**, and enter *localhost* for the **Server host**. Enter the **Server port** number (for example, *25*).
  
Both email and password are not required.

### Adding Attachments

#### Normal Attachment

To add attachments to the email message, do the following:

1. Create an **Attachment** entity. The **Attachment** entity extends the **FileDocument** entity by making it usable in all the places where the **FileDocument** entity is required.

   {{% alert color="info" %}}If you have a custom entity, extend it with the **Attachment** entity instead of **FileDocument**, or use the community commons **DuplicateFileDocument** function to create an **Attachment** from your custom entity.{{% /alert %}}

2. Set the **Attachment_EmailMessage** association.

#### Inline Attachment

To add inline attachments to an email message, use the Rich text editor to insert images directly into the email body. Alternatively, you can insert inline attachments using a microflow by following these steps:

1. Create an EmailMessage with the `Content` property set as seen below:

    ```
    'before inline image<br><img src="cid:mxcid:test.png" width="530" height="110"><br>after inline image'
    ```

2. Specify the image's tag source using the **cid:mxcid** prefix before the source file to have the image added as inline image.
3. Create the attachment with the Position attribute set to **ENUM_AttachmentPosition.Inline**.
4. Associate the attachment with EmailMessage. You can then send the email using the **SUB_SendEmail** microflow.

### Page Styling

If the **Email Connector** page styling is affected as you select and view email messages, it likely due to errors in the email message CSS. 

To resolve the errors, turn on **Sanitize email to prevent XSS attacks** in [Account Settings](#other-account-settings).

### Importing Emails with Long Attachment Names

{{% alert color="info" %}} This issue is resolved in version 5.3.0 of the Email Connector. {{% /alert %}}

Some email clients (for example, Gmail) break down the name of attached files if it is longer than a specific value, and add the file name in the **Content Type** for an attachment. This can cause an error while fetching or importing emails with long attachment names. For example: *"ERROR - Email_Connector: Attribute 'Email_Connector.Attachment.attachmentContentType' has a maximum length of 200, tried setting a value of length xxx."*. 

## Known Issues

### Widgets

If you already have the [included widgets](#included-widgets) in your app, and they are not up to date, you may get a "Some widgets cannot be read" error when trying to run locally.

### Consistency Error

You may get a consistency error when importing the Email Connector module in Mendix 10.1 or above that states *"No argument has been selected for parameter "Token" and no default is available"*. This can be resolved by double-clicking the error, which takes you to the snippet **SNIP_EmailTemplate_NewEdit**. Double-click the **Edit [default]** button, then in the **Events** field under **Page settings**, click **Edit**. Once the **Page Settings** dialog box opens, click **OK**, as shown in the image below. The error should resolve.

{{< figure src="/attachments/appstore/platform-supported-content/modules/email-connector/consistency-error-token.png" class="no-border" >}}

### Duplicate Mx Model Reflection Objects in Email Template

{{% alert color="info" %}} It is recommended to try this microflow in a non-production environment and check if is working as expected after the cleanup. After the successful validations, run it in the production environment.{{% /alert %}}

The Email Template export/import functionality introduced in EC v5.8.0 had a bug, which was subsequently fixed in v5.9.0. If you exported and imported Email templates using EC v5.8.0, you may see duplicate records in the placeholder entity drop-down, as shown below.

{{< figure src="/attachments/appstore/platform-supported-content/modules/email-connector/duplicate-mx-reflection-objs.png" class="no-border" width="700" >}}

To clean up these duplicate Mx Reflection records, a microflow is delivered as part of EC v5.9.2. As the cleanup microflow (**DEL_DuplicateMxReflectionObjects**) deletes database records from certain tables, it is strongly advised to complete a full DB backup before proceeding with the steps below. At a minimum, the following 3 tables must be fully backed up before starting.

1. MxObjectMember
2. MxObjectReference
3. MxObjectType

To perform the cleanup, follow these steps:

1.Call the **DEL_DuplicateMxReflectionObjects** microflow from a page using the **Call microflow** button. This should preferably be executed by an Admin user as a one-time activity.

This microflow identifies and removes duplicate records from the backend. Upon completion, a pop-up dialog confirms the process has finished. The Console log displays the number of records removed from each respective table.

   {{< figure src="/attachments/appstore/platform-supported-content/modules/email-connector/mx-reflection-objs-cleanup-logs.png" class="no-border" width="700" >}}

2. After completing the cleanup process, remove the **Call microflow** button from the page to prevent the microflow from being triggered again.
