---
title: "SCIM"
url: /appstore/modules/scim/

description: "Describes the configuration and usage of the SCIM module, which is available in the Mendix Marketplace."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

The [SCIM](https://marketplace.mendix.com/link/component/229630) module facilitates integration with your IdP to create (pre-provision) selected users in your application as soon as they are created in the IdP. It deactivates them when they are removed or deactivated in the IdP.

SCIM is an abbreviation for System for Cross-domain Identity Management; a protocol supported by most major IdP technologies, such as Entra ID (formerly known as Azure AD), Okta, Auth0, etc. For more information, see the [SCIM protocol](https://scim.cloud/).

The SCIM module allows you to integrate your app with the Joiner, Mover, Leaver (JML) process in your organization. It enables the assignment of tasks to users before their first login to the app. Additionally, based on the information in your IdP (for example, user groups), the SCIM module automatically creates and deactivates users in your app. This functionality helps you to control Mendix user licensing costs.

{{% alert color="info" %}}
Before you include the SCIM module in your app, you need to check if your IdP supports SCIM protocol. If you want to integrate with an on-premises AD or similar, you may need to use the [LDAP module](/appstore/modules/ldap/) instead.
{{% /alert %}}

### Typical Use Cases

* **Seamless integration into the JML process:** Organizations need to handle employee access rights efficiently, when employees join, switch roles, or leave the organization. The Joiner, Mover, Leaver (JML) process is crucial for this, and the SCIM module helps your app integrate smoothly into these processes.

* **User onboarding before first login:** The SCIM module can be used when you want to create users in your Mendix app before their first login. For example, if your app has workflow logic, you can assign tasks to users even before their first login.

* **Use user groups to control access to your app:** Entra ID helps you organize various groups based on their jobs and roles. To manage app access based on these groups, you can use the SCIM module. If someone changes their job or role and is no longer part of a group that allows access to the app, the SCIM module automatically deactivates the user in the app. Even if the user still has an active Entra ID account, it becomes inactive in your app. This way, you can also save on licensing costs.

* **Update local user profile:** Changes in the personal details of your employees, such as email address or username modifications due to life events like marriage, are synchronized between the local user profile and Entra ID through the SCIM module.

* **B2E vs. B2C apps:** Many organizations use a common Identity Provider, such as Entra ID in Microsoft’s Office 365 suite, to centralize employee accounts. If you are building apps for employees (Business-to-Employee or B2E), connecting them with Entra ID is a common and easy choice. As an app developer, you can use OIDC SSO or SAML for Single Sign-On and the SCIM module for managing users. For apps designed for customers (Business-to-Customer or B2C), the use of the SCIM module may not be as obvious. If your organization uses a third-party solution, such as Entra ID to handle customer accounts, you can also use the SCIM module for that.

### Features {#features}

Your IdP can perform create, read, update, and delete (CRUD) operations on the users in your app when it includes the SCIM module. It has the following features:

* Create users: users who are in Entra ID and assigned to the Mendix SCIM application in Azure are automatically created in the Mendix application.

* Remove users: user deletion can be either a 'hard' delete, which removes user records from the app's database, or a 'soft' delete, which deactivates the Mendix user but keeps their records. If you set the flag **Default_DeleteUserPermanently** (in the **Acceptance Environment Details** of the Mendix application environment) to *True*, the user will be hard deleted. By default, the flag is set to *False*.

* The following user attributes are supported during the creation or updating of users: first name, last name, and email address.

* Update users: synchronizes changes in the user's profile in your IdP with your Mendix app, such as a change in the user’s information.

* Disable (Deactivate) / Enable (activate) users: deactivates or activates users in your Mendix app when you disable or enable them in the Entra ID.

The SCIM module also has the following features:

* It has been tested with Entra ID and Okta.
* It has been tested in combination with SAML and OIDC SSO modules using Entra ID and Okta as an IdP.

    {{% alert color="info" %}}
If you are using the SCIM module in combination with Entra ID and OIDC SSO, you need to choose the correct attribute mapping. For more information, see the [Guidance on User Identifier](/appstore/modules/oidc/#guidance-user-identifier) section of the *OIDC SSO* and [Attribute Mapping](#attribute-mapping) section below.
    {{% /alert %}}

* Your IdP allows the selection of users that sync with your Mendix app, meaning your IdP controls which users are created and active in your app.
* Each application within your organization using the SCIM module must undergo a separate configuration. This allows the selection of the right target group of users for each app.
* Synchronization of users from multiple SCIM clients is currently in beta.
* The module supports deployment-time configuration using constants, eliminating a need for runtime configuration by a local admin user.

### Limitations

The SCIM module has the following limitations:

* The SCIM module does not sync groups (or group memberships) to your app. This means you cannot use the SCIM module to assign user roles to your app’s users. Instead, you can assign user roles using the features offered by [SAML SSO](/appstore/modules/saml/) or [OIDC SSO](/appstore/modules/oidc/) modules.
* If you want to do **Provision on demand** from Entra ID to test the SCIM integration of your app you cannot trigger a partial sync based on a group. This will trigger Entra ID to invoke a `/groups` endpoint, which is not yet supported.
* The module does not support the development of a SCIM client application.
* By default, the SCIM module’s admin screen is displayed in English. Users cannot add additional languages to the module, as it is protected and does not support language modification. 
* Multiple clients can be configured only at runtime via the admin screen.

### SCIM Protocol Adherence

This section provides clarity on the extent to which the SCIM module supports the SCIM protocol. It is targeted at readers who are familiar with the SCIM protocol.

* The SCIM module is based on version 2.0 of the SCIM protocol, as specified in [RFC 7642](https://datatracker.ietf.org/doc/html/rfc7642), [7643](https://datatracker.ietf.org/doc/html/rfc7643), and [7644](https://datatracker.ietf.org/doc/html/rfc7644).

* The SCIM module is a server-side implementation of the SCIM protocol; external systems act as SCIM clients when interacting with your app.

* The supported features align with the requirements for integration with Entra ID.

* SCIM module supports users via the `/users` endpoint. Known resources can be retrieved using a GET.

* It is possible to have multiple SCIM clients for a single Mendix application. The tenancy model enables each client to create and access a private subset of users. For more information, see the [Multi-Tenancy](https://datatracker.ietf.org/doc/html/rfc7644#section-6) section.

Currently, the SCIM module does not support the following features of the SCIM schema:

* Sync password

* User attributes other than a basic user profile. Mendix currently supports a limited set of user attributes from the SCIM user schema, including UserName, Email, FamilyName, GivenName, and Active status

* Multi-valued attributes

* User schema extensions

* Multi-tenancy

The SCIM module does not support the following features of the SCIM protocol:

* `/Me` endpoint

* Query resources using GET

* Bulk operations `/bulk`

* `/group` endpoint

### Prerequisites

1. An IdP such as Microsoft Entra ID

2. A user account in the IdP with permissions to configure provisioning (for example, application administrator, cloud application administrator, application owner, or global administrator)

### Dependencies

It is necessary to include the following Marketplace modules in your app:

* [Encryption](https://marketplace.mendix.com/link/component/1011) module – this is required to encrypt the **API key** (Token)

* [Community Commons](https://marketplace.mendix.com/link/component/170) – see [Community Commons](/appstore/modules/community-commons-function-library/) documentation

* [User Commons](https://marketplace.mendix.com/link/component/223053) module - install and configure the User Commons module to support user provisioning and deactivation of user

* [Mx Model Reflection](https://marketplace.mendix.com/link/component/69) module - install and configure the [Mx Model Reflection](/appstore/modules/model-reflection/) module

## Upgrading the SCIM Module

This section provides an overview of updates for the SCIM and UserCommons modules across different versions. It includes several key updates to ensure a smooth transition while migrating to higher module versions.

{{% alert color="info" %}}
When upgrading to version 2.0.0 of the SCIM module, ensure you are also using version 2.0.0 of the UserCommons module.
{{% /alert %}}

| Mendix Version | SCIM Module Version | UserCommons Version | SCIM Information|
| --- | --- | --- | --- |
| 10.12.10 and above | 2.0.0 | 2.0.0 | SCIM module is ready for the React client. |
| 9.24.2 and above | 1.0.2 | 1.0.2 | – |

## Configuration

### Design-time / Development Time Configuration

Import the SCIM module from the marketplace and do the following configuration:

#### Configuring Roles

Configure the **Administrator** module role to create/delete and read/write the app configuration.

The following is a typical example of how you may want to include the SCIM module’s **Module Roles** to the **User roles** of your app.

| **User Role** | **SCIM Module Role** |
| --- | --- |
| Administrator | SCIM.Administrator |

{{< figure src="/attachments/appstore/platform-supported-content/modules/scim/user_roles.png" class="no-border" >}}

#### After Startup Microflow

In the **Runtime** tab, add the microflow **SCIM.ASU_StartUp** as the **After startup** microflow.

#### Navigation

The SCIM module has some configuration pages targeted at the users with local administrator app rights.
Add these pages to the **Navigation** tab and assign the **Administrator** user role.
Open the [Navigation](/refguide/navigation/) tab and do the following:

* Add the **Menu** item **IdPConfiguration** to the app **Navigation**. Link this item to the **Call_IdPConfOverview** nanoflow and assign it to the **Administrator** user role.
* Add the **Menu** item **MxObjects** to the app **Navigation**. Link this item to the **MxModelReflection.MxObjects_Overview** page and assign it to the **Administrator** user role.

#### Setting Encryption Key

Set up the required configuration of the [Encryption](https://marketplace.mendix.com/link/component/1011) module as described in the [Encryption](/appstore/modules/encryption/) document. The encryption module is used to encrypt the API key (token) that a SCIM client needs to interact with the SCIM endpoint in your app.

#### Default User Provisioning

When using SCIM, Mendix recommends disabling Just-In-Time (JIT) user provisioning in your SSO module. This ensures that SCIM remains in control of user provisioning and prevents the creation of duplicate users.

The SCIM module includes a default user provisioning microflow that maps user attributes from the SCIM payload to attributes in the common user objects within your Mendix app. For more details, see the [Attribute Mapping](#attribute-mapping) section below.

Ideally, both SCIM and SSO modules should utilize the same immutable user identifier for user identification.  This approach enables the SCIM module to modify any attribute except the principal attribute without creating a new user. However, in practice, this may require attention depending on your identity provider (IdP). When you are using Entra ID, OIDC SSO, and SCIM, (see the table mentioned in the [Features](#features) section above) the default user provisioning flows may not work for you. You may need to create custom user provisioning microflows for either module.

#### Attribute Mapping {#attribute-mapping}

This section provides reference information about the attribute mapping applied by the default user provisioning flow in the SCIM module. User-identifying attributes in SCIM are compared with those in SSO modules.

If you wish to deviate from this default attribute mapping, you can create your own mapping. To do this, select an **IdP Attribute** (claim) and specify the **Configured Entity Attribute**. Ensure that the same configured entity attribute should not be mapped with another IdP attribute.

For reference, the table below gives an overview of attribute mapping when using the defaults in both Entra ID and the default microflow in the SCIM module. This information may be helpful to see where a custom user provisioning flow differs from the defaults.

| **SCIM payload** (IdP Attribute) | **Configured Entity Attribute** | **Typical value with Okta** (IdP Attribute) | **Typical value with EntraID** (IdP Attribute) |
| --- | --- | --- | --- |
| externalID | Name | 00ctc4pufr85d7 | ObjectID (configured, non-default) For example, `550e8400-e29b-41d4-a716-446655440000` |
| familyName| lastname | Doe | Doe John |
| givenName| firstname | John |  |
| emails| Email | `john.doe@companyA.com` | `johndoe@companyA.com` |
| userName| FullName | `johndoe@companyA.com` | `johndoe@companyA.com` |
| active| Active | true | true |

{{% alert color="info" %}}
 In the SCIM protocol, you can configure the `object ID` to identify a user for Microsoft. It is used as the value for the `externalID` claim in SCIM payloads by default. That means, use the `oid` claim as a user identifier to introduce SCIM. For more information, see the [Guidance on User Identifier](/appstore/modules/oidc/#guidance-user-identifier) section of the *OIDC SSO*.
{{% /alert %}}

You can configure the `objectId` in SCIM using the steps below:

1. In the Azure portal, select the application which is integrated with the SCIM module.
2. Edit the attribute mapping.
3. For `externalId`, change the **Source attribute** to `objectId`.

The table below compares the primary user-identifying attribute used by SCIM (i.e., the External ID) with the identifying claims used by SSO modules.

| **IdP** | **SSO Module** | **Remark** |
| --- | --- | --- |
| Okta | OIDC SSO | SCIM.externalID and OIDC.sub contain the same value. |
| EntraID | OIDC SSO | SCIM.externalID and OIDC.oid contain the same value. |
| Okta | SAML | SCIM.externalID and SAML.Use Name ID contain the same value. <br> Note: Configure Application username to Custom with user.getInternalProperty("id"). |
| EntraID | SAML | SCIM.externalID and SAML.Use Name ID contain the same value. <br> Note: Map Unique User Identifier as user.objectid in SSO Configuration. |

### Runtime Configuration

#### API Security {#api_security}

Ensure that only legitimate SCIM clients can interact with your app via the SCIM module. You need to enable your SCIM client to authenticate itself with your app. The SCIM module currently supports using an **API Key** (token) for the authentication. You can generate an API Key to download or upload one into your SCIM client during [Deploy Time Configuration](#deploy-time). This can be done using two options below:

1. Using SCIM module to generate an API key

    To generate and download an API key, click **New** on the **IdPConfiguration** page and configure the fields below:

    * **Alias**: This is an alias name for your configuration. For Example, *Azure*.

    * **API Key**: Used as a token for header-based authentication. IdP will send this as the authorization header parameter in the request.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/scim/alias.png" class="no-border" >}}

2. Configuring API-Key via Pipeline

Another option is to generate an API key yourself and submit it to the SCIM module via a SCIM constant. To do this, set the constant `SCIM.Default_APIKey_Value` in the **Acceptance Environment Details** of the Mendix application environment. This approach enables you to manage API security without requiring a local administrator to log in to your application. It provides the flexibility to use the same API key for multiple applications using the SCIM module.

{{% alert color="info" %}}Starting from UserCommons version 2.0.0, If the IdP does not specify the time zone and language, these settings will be set according to the default **App Settings** of your app. If no default is available, they remain unset. Existing users retain their previously set values.{{% /alert %}}

#### User Provisioning

In the **Provisioning** tab of the SCIM server configuration, you need to configure the following fields:

* **Custom user Entity (extension of System.User)**: the entity in which you will store and look up the user account. If you are using the Administration module this would be `Administration.Account`.
* **The attribute where the user principal is stored** (primary attribute): unique identifier associated with an authenticated user.
* **Allow the module to create users**: this enables the module to create users based on user provisioning and attribute mapping configurations.
    * By default, the value is set to ***Yes***.
* **Default Userrole** – the role assigned to newly created users and remains unchanged even when the user's details are updated. You can select one default user role. To assign additional roles, use the Access Token Parsing Microflow. If the Access Token Processing Microflow is selected, OIDC verifies the updated default role configuration and applies any changes to the user's role. Note that, bulk updates for existing users are not automated when the default role configuration is changed.
* **User Type**: this allows you to configure end-users of your application as internal or external. It is created when the user is created and updated whenever user details, such as name, email, or active status, are changed. For more information, see [Populate User Types](/howto/monitoring-troubleshooting/populate-user-type/).
    * By default, the value is set to ***Internal***.
* **Attribute Mapping**: under **Attribute Mapping**, select an **IdP Attribute** (claim) for each piece of information you want to add to your custom user entity. Specify the **Configured Entity Attribute** where you want to store the information.

Note the following:

* You cannot use the IdP claim which is the primary attribute identifying the user and you cannot use the attribute you set in **The attribute where the user principal is stored**.
* You can map only one IdP claim to a **Custom user Entity** attribute.
* The **IdP Attribute** is one of the fixed claims supported by the SCIM module.
* **IdP attribute** (Claim) cannot be of type enum, autonumber, or an association.
* Use custom logic in the **User Provisioning** (Optional) – In **Custom UserProvisioning**, select a microflow you want to run for custom user provisioning.

The custom microflow name must begin with the string `UC_CustomProvisioning` and requires the following parameters:

1. **UserInfoParameter(UserCommons.UserInfoParam)**: A Mendix object containing user claims information through its associated objects. You can use this  parameter to retrieve user provisioning configuration information.
2. **User(System.User)**: A Mendix object representing the user to be provisioned. Ensure that the selected microflow matches this parameter signature.

 The microflow must return a **System.User** object to ensure proper user provisioning and updates. It will be executed after user creation or update of user. However, starting from version 2.0.0 of the UserCommons module, this is no longer mandatory. If you have added a new microflow, you need to refresh the module containing your microflow as described in the [Mx Model Reflection](/appstore/modules/model-reflection/).

{{< figure src="/attachments/appstore/platform-supported-content/modules/scim/user_commons.png" class="no-border" >}}

This selection can be blank if you do not want to add custom logic. Save this configuration. Double-click on the **Alias** name and you will be able to copy the generated **API Key**.

### Deploy-time Configuration {#deploy-time}

Starting from version 1.0.2, you can configure the SCIM module using app [constants](https://docs.mendix.com/refguide/constants/) instead of the app administration pages. As an app developer that uses SCIM, you can set default values. These values can be overridden at deploy-time using the app constants.

To enable app constants for configuring the SCIM module, set your app to run the after-startup microflow in the OIDC module (`SCIM.ASU_StartUp`).

Use the following security best practices when setting up your constants:

* Set the export level of these constants to **Hidden** to enhance security.
* Mask the `Default_APIKey_Value` to prevent its value from being visible in the Mendix Portal. For more details, see the [Constants](/developerportal/deploy/environments-details/#constants) section of the *Environment Details*.

#### Customizing Default Deploy-time Configuration

The table below lists all supported constants. Mandatory constants must be set at deploy-time, while optional constants can typically be set at design time but, if necessary, can be overridden at deploy-time.

| Constants | Description | Mandatory/Optional | Default Value |
| --- | --- | --- | --- |
| `Default_APIKey_Value` | **API Key** (token) for the authentication | Mandatory | No default value |
| `Default_IdPConfiguration_Name` | default IdP Configuration name. Since only one default deploy-time configuration constant is supported, it cannot be modified or deleted.| Mandatory | No default Value |
| `Default_AllowCreateUsers` | allows to create users in the application | Optional | `True` |
| `Default_CustomEntity_Name` | custom Entity name that can be specified for provisioning | Optional | `Administration.Account` |
| `Default_DeleteUserPermanently` | a flag to delete users permanently or not | Optional | `False` |
| `Default_FirstNameMapping` | sets the mapping entity attribute to the Identity provider attribute | Optional | `FullName` |
| `Default_LastNameMapping` | sets the mapping entity attribute to the Identity provider attribute | Optional | No default Value |
| `Default_PrincipalEntityAttribute` | the attribute holding the unique identifier of an authenticated user | Optional | `Name` |
| `Default_PrincipalIdPAttribute` | the IdP claim which is the unique identifier of an authenticated user | Optional | `ExternalId` |
| `Default_UserIdMapping` | sets the mapping entity attribute to the Identity provider attribute | Optional | No default Value |
| `Default_UserNameMapping` | sets the mapping entity attribute to the Identity provider attribute |  Optional | No default Value |
| `Default_UserRole` | sets the mapping entity attribute to the Identity provider attribute | Optional | `User` |
| `Default_UserType` | sets the mapping entity attribute to the Identity provider attribute | Optional | `Internal` |

#### Configuration with Entra ID

Setting up connectivity with an IdP varies depending on the vendor. The following subsection shows the configuration for the Microsoft Entra ID.

1. On the Microsoft Entra ID tenant, select **Enterprise Application** and create a SCIM client in it.
2. Change the **Provisioning Mode** to **Automatic**.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/scim/provisioning_revised.png" class="no-border" >}}

3. Configure the SCIM server (Mendix SCIM Application) with the following details:

    * **Tenant URL**: `https://{host name}/scim/v2`. Replace `{host name}` with your intranet domain. For example, `https://intranet.acme.com/scim/v2`.
    * **Secret Token**: copy the **API Key** (token) from the **IdPConfiguration** page of User Commons, as described in the [API Security](#api_security) section above.
    * Click **Test Connection**.

4. Configure **Mappings** and **Settings**.

    * **Mappings** allow you to define how data should be mapped between Entra ID and the Mendix application.
    * Click **Settings** and you can change the scope to **Sync only assigned user and groups**.
    * **Save** the configuration.

5. Click **Users and groups** to add and assign the required individual users or group (or groups) of users to the SCIM Client. The scheduled provisioning will sync these users.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/scim/users_and_groups.png" class="no-border" >}}

6. On the SCIM client app, click **Provisioning** to do the user provisioning.

## Testing and Troubleshooting

### Testing with Entra ID

Once you have your SCIM module configured, you can test it by creating, updating, and deleting the user.

The test case below is defined for the scope of **Sync only assigned user and groups** and validation of provisioning status in the SCIM server.

* Create a user with the default `userPrincipalName`. Optionally, you may choose to change the user Mappings in the Microsoft Entra ID. You can change the Entra ID attribute from `userPrincipalName` to another attribute. For example, `mailNickName`.
* Update a user in the Microsoft Entra ID. For Example, add/update work email, last name, etc.
* Delete a user in the Microsoft Entra ID.

To hard delete a user, you can set the flag **Default_DeleteUserPermanently** (in the **Acceptance Environment Details** of the Mendix application environment) to *True*. By default, this flag is set to *False*.

You may want to use **Provision on demand** while testing SCIM module integration for immediate provisioning. You can either select individual users or users in a group (or groups).
The below options provide you with the choice of user selection during on-demand provisioning.

* **View members only**: select users from the group
* **View all users**: select users from the Entra ID

{{% alert color="info" %}}
Your app may have multiple tenants in the Entra ID. Users created from any of the Entra ID tenants will have the same domain in their `user_principle_name`. This results in an active user in your app which you have not assigned in your application in Entra ID.
{{% /alert %}}

### Troubleshooting

Attempting to edit or delete the default deploy-time configuration at runtime will result in errors. These errors indicate that the SCIM configuration was created at runtime with possible deploy-time overrides, as explained in the [Deploy-time configuration](#deploy-time) section above. You cannot modify such configurations at runtime using the admin screens.
