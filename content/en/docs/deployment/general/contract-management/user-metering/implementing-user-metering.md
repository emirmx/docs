---
title: "Implementing User Metering"
url: /developerportal/deploy/implementing-user-metering/
weight: 30
description: "This document describes how to implement user metering."
---

## Introduction

User metering provides complete visibility into the number and types of users accessing the application, ensuring compliance with the license agreement. You can see this data in the Usage Report of Control Center on the Mendix platform.
For accurate user metering, in-app user classification is a crucial first step. For more information, refer to [How User Metering Works](developerportal/deploy/user-metering/#how-user-metering-works). To do this, the logic in your app models needs to cater to the following aspects, if applicable:

* [Unique user identification](/developerportal/deploy/implementing-user-metering/#guidelines-for-unique-user-identification-deduplication)
* [User classification](/developerportal/deploy/implementing-user-metering/#user-classification)
* [User deactivation](/developerportal/deploy/implementing-user-metering/#deactivation-of-users)

## Guidelines for Unique User Identification (Deduplication)

Mendix allows you to build multi-app solutions and offers a multi-app user license option. For internal users, you may have a single-app user license or a multi-app user license. External user licenses cover both single-app and multi-app users; a multi-app external user is counted only once.  For more information, refer to [User types and definitions](/developerportal/deploy/licensing-apps-outside-mxcloud/#user-types-and-definitions).  For accurate user metering, it is a crucial step to identify the unique multi-app user by using the same user identifier in all apps for a user.

The Mendix metering mechanism requires your app logic to store an identifier for every user in the `UserCommons.namedUserIdentifier.value` (or `system.user.name` as a fallback). For relevant entities and their attributes, see the [Domain Model Entities](/developerportal/deploy/implementing-user-metering/#domain-model-entities) section below.

For metering of multi-app users, the same value must be stored across all apps. When different values persist, Mendix treats them as different users.

To ensure your multi-app users are counted correctly, you may want to consider the following guidelines:

### Choosing a Cross-App User Identifier:

User metering may not have been considered when your application was originally designed. As a result, different applications may store different identifier values for the same multi-app user in the `system.user.name`. Additionally, all apps in your portfolio may use different user-provisioning logic. Some apps use standard identity modules (Administration, SAML, OIDC SSO, SCIM, or LDAP) while others rely on proprietary modules or app logic to create users. This can lead to inconsistent identifiers.
Even when all apps use the same solution, for example, OIDC SSO, the technical identifier value for a multi-app user may still differ.

### Avoiding Pairwise Identifiers:

Even if all your apps are using the same user onboarding (provisioning) logic, this does not guarantee a user gets the same (technical) identifier value in all your applications. 
The OpenID Connect specifications incorporate the concept of [pairwise user identifiers](https://openid.net/specs/openid-connect-core-1_0.html#PairwiseAlg). The general idea of pairwise identifiers is to prevent user correlation across apps owned by different service providers.  A user gets a different value in each app when using pairwise user identifiers.

Mendix recommends storing the user’s email address in the `UserCommons.NamedUserIdentifier.value`; this ensures usage of a pairwise unique identifier in the `system.user.name` does not affect metering.

Within your application portfolio, the possibility to prevent cross-app user correlation is probably not needed; instead, you do want the Mendix metering system to correlate users and recognize multi-app users. Microsoft’s Entra ID uses pairwise identifiers in the OIDC `sub` claim. It is, however, possible to include the `oid` claim, which contains the same value for a given multi-app user across all applications; Entra ID’s user object ID. Mendix recommends storing the `oid` claim in the `system.user.name` if you are using the OIDC SSO module with Entra ID.

### Keeping the Existing Logic and Extending

If your apps are not consistently storing the same identifier for a multi-app user in `system.user.name`, you can start using a new user attribute, a new `UserCommons.namedUserIdentifier.value`.You can keep the existing application logic as it is, and in addition, store the selected user identifier value for a multi-app user in this new attribute.

### Paying Attention to Case Sensitivity

By design, the `system.user.name` is case sensitive and allows customers to store any user identifier without restrictions. If you choose to store an email address in these fields, or another identifier, that should be treated as case-insensitive. Mendix recommends downcasting such values to lowercase. The SAML, OIDC SSO, and SCIM modules already apply this for email claims or attributes received from your IdP.

For more information, refer to the [Versioning](/developerportal/deploy/implementing-user-metering/#versioning-information) section below.

## User Classification

For accurate user metering, `External` users must be correctly classified. If they are not, your company may exceed the licensed capacity for `Internal` users, and Mendix may require you to acquire additional internal user licenses. If you do not classify your `External` users, they will be treated as `Internal` users by default.

There are several approaches to classify users as `Internal` or `External`, ranging from configuration-only to custom development. These options are:

### IdP-Based User Classification

This method requires one of the following identity modules to be enabled: OIDC SSO, SCIM, or SAML. A key advantage of this approach is that it does not require any modifications to your existing app. Classification can begin immediately by setting a constant. However, because users are only classified when they log in, it may take some time before all end users are classified.

For more information, see the [IdP-Based User Classification](/developerportal/deploy/populate-user-type/#idp-based-user-classification) section of *Populate User Types*.

### Userrole-Based User Classification

The user-role-based user classification module classifies users by using the roles already defined in your app.  It can update all existing users in one run and works well if you already have separate roles for internal and external users. However, using this module requires upgrading your app to include the user classification module. For more information, refer to [User-Role-Based Classification](developerportal/deploy/populate-user-type/#user-role-based-user-classification).

### Custom User Classification

This method can be implemented either by using the [User Classification](/appstore/modules/user-classification/) module or by creating fully custom microflows. The main advantage of this approach is the flexibility it provides. You can apply any classification logic you choose while still relying on the user classification module as the base. However, this method requires developing custom logic and involves upgrading your app to a new version of your app to include the [user classification](https://marketplace.mendix.com/link/component/245015) module.

{{% alert color="info" %}}
In situations where there is ambiguity about the user classification, Mendix considers users as internal users. Ambiguity may happen in the following situations:

* A multi-app user is marked as `External` in one app and `Internal` in another app.
* When applying userrole-based classification, and a user has both an `Internal` and an `External` userrole.
{{% /alert %}}

Classification by the platform is not supported by using your email domains, nor any other logic. Your app logic is responsible for classification. For more information, refer to [Custom Classification](/developerportal/deploy/populate-user-type/#custom-classification).

## (De)Activation of Users

`system.user.active` attribute controls active or inactive users in your application. Users with an ‘active’ state can log in to your app and are counted for user metering purposes, while non-active users cannot login are not counted for user metering.

You can provision the user records (for example, created, updated, or deleted) using proprietary logic or via one of the following platforms supported modules: 

* SAML and OIDC SSO modules can create users with active state. By nature of the SSO protocols, these modules do not deactivate or delete any user records in your application.
* The SCIM module allows your app to be integrated with your IdP to create, update, or delete user records. Depending on the (configured) business logic in your IdP, users may be created, deactivated, or deleted

    * as a result of the Joiner/Mover/Leaver processes.
    * as a result of license optimization, users who have not logged in to a certain application for a certain period may be removed from the access group in the IdP, and therefore, the IdP may deactivate or remove users from your SCIM-enabled application.

* The LDAP module allows you to synchronize users and their status (active=false/true) from your on-premises Active Directory to your application. This is a similar module to SCIM but using a different protocol.

If you are considering deactivating or removing user records for optimization of user cost, you are advised to consider the following:  

* Users cannot log in when in the deactivated (active=false) state. The primary purpose of the active state is to control which users can log in. When SSO is used, the user may be successfully logged in at the IdP; however, the Mendix application fails to grant access if the active state is not updated.
* Actual logins, frequency of login, or `system.user.lastlogin` is also not relevant for metering. A user is counted as an active user if the state was active on any day of the calendar month.
* If you choose to remove user records, associated data may be deleted depending on your domain model.
* The SAML and OIDC SSO modules are capable of (re)creating any user that was deleted. This can be described as just-in-time user provisioning, also known as on-the-fly user onboarding or real-time user creation.

## Versioning Information

The improved user metering capabilities are introduced in Mendix 11. These capabilities in the platform apply to apps using any Mendix version. Mendix offers the following guidance on versioning.

| Classification Menthod | Mendix 9 LTS | Mendix 10 LTS, Mendix 11 |
| --- | --- | --- |
| IdP-based user classification | SAML 3.6.21 and above <br> OIDC 3.0.0 and above | SAML 4.0.0 and above <br> OIDC SSO v4.0.0 and above |
| Userrole-based or custom user classification | User Classification v1.0.1 and above | User Classification v2.0.0 and above |
| Multi-app user identification | `system.user.name` | `system.user.name` or `NamedUserIdentifier` in User Commons v2.2.0 and above |
| SCIM-based user deactivation | SCIM v3.0.0 and above | SCIM v4.0.0 and above |

If you have extended support on Mendix 8, contact your CSM for guidance on user metering if needed.

## Domain Model Entities

This section explains the entities and their attributes in your app domain model that are relevant for user metering.

The following are entities and their attributes:

1. `User`: Every Mendix app has a system module containing an entity `User`.

    * `system.user.name`: This field is used to identify users, unless a `UserCommons.namedUserIdentifier.value` is available for the same user.
    * `system.User.Active`: A Boolean attribute, `Active`, is used to select the active user account for user metering. A user record that has `Active=true` during the metering period is counted as an active user during the billing period and reconciliated against one of the allocated user licenses.

2. `UserReportInfo`: The system module also features the `UserReportInfo`.

    * `system.UserReportInfo.UserType`: `UserType` is used to classify end-users as `External` or `Internal` Users. Your application must set the attribute for all existing and new (external) end users. If it does not, Mendix will classify those users as `Internal`.

3. `namedUserIdentifier`(optional): Recent versions of the [UserCommons](https://marketplace.mendix.com/link/component/223053) module include a `namedUserIdentifier` entity.

    * `UserCommons.namedUserIdentifier.value`: User commons modules in your app model assign the values to the `namedUserIdentifier.value` attribute to identify a user instead of `system.user.name`. The `namedUserIdentifier` overrides `user.name`.

Note that one application may use `system.user.name` and not `userCommons.namedUserIdentifier.value`, or may exclude the UserCommons module, while another application may use `userCommons.namedUserIdentifier.value` instead. If the values in these different fields are the same for a multi-app user, the Mendix user metering mechanism correctly identifies the user as a single multi-app user (deduplication).

It is not possible to map multiple values of `system.user.name` to a single value of `UserCommons.namedUserIdentifier.value` within an app.
