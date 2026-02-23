---
title: "Implementing User Metering"
url: /developerportal/deploy/user-metering/
weight: 40
description: "This document describes how to implement user metering."
---

## Introduction

User metering provides complete visibility into the number and types of users accessing the application, ensuring compliance with the license agreement. You can see this data in the Usage Report of Control Center on the Mendix platform.
For accurate user metering, you need in-app user classification as a first step. For more information, refer to [How User Metering Works](developerportal/deploy/user-metering/#how-user-metering-works). To do this, the logic in your app models needs to cater to the following aspects, if applicable:

* Unique user identification
* User classification
* User deactivation

## Guidelines for Unique User Identification (Deduplication)

Mendix allows you to build multi-app solutions and offers a multi-app user license option. For internal users, you may have a single-app user license or a multi-app user license. External user licenses cover both single-app and multi-app users; a multi-app external user is counted only once.  For more information, refer to [User types and definitions](/developerportal/deploy/licensing-apps-outside-mxcloud/#user-types-and-definitions).  For accurate user metering, it is a crucial step to identify the unique multi-app user by using the same user identifier in all apps for a user.

The Mendix metering mechanism requires your app logic to store an identifier for every user in the `UserCommons.namedUserIdentifier.value` (or `system.user.name` as a fallback). For relevant entities and their attributes, see the [Domain Model Entities]() section below.

For metering of multi-app users, the same value must be stored across all apps. When different values have persisted, Mendix sees them as different users.

To ensure your multi-app users are counted correctly, you may want to consider the following guidelines:

### Choosing a cross-app user identifier

User metering may not have been considered when your application was originally designed. As a result, different applications may store different identifier values for the same multi-app user in the `system.user.name`. Additionally, all apps in your portfolio may use different user-provisioning logic. Some apps use standard identity modules (Administration, SAML, OIDC SSO, SCIM, or LDAP module) while others rely on proprietary modules or app logic to create users. This can lead to inconsistent identifiers.
Even when all apps use the same solution, for example, OIDC SSO, the technical identifier value for a multi-app user may still differ.

### Avoiding pairwise identifiers:

Even if all your apps are using the same user onboarding (provisioning) logic, this does not guarantee a user gets the same (technical) identifier value in all your applications. 
The OpenID Connect specifications incorporate the concept of [pairwise user identifiers](https://openid.net/specs/openid-connect-core-1_0.html#PairwiseAlg). The general idea of pairwise identifiers is to prevent user correlation across apps owned by different service providers.  A user gets a different value in each app when using pairwise user identifiers.

Mendix recommends storing the user’s email address in the `UserCommons.NamedUserIdentifier.value`; this ensures usage of a pairwise unique identifier in the `system.user.name` does not affect metering.

Within your application portfolio, the possibility to prevent cross-app user correlation is probably not needed; instead, you do want the Mendix metering system to correlate users and recognize multi-app users. Microsoft’s Entra ID uses pairwise identifiers in the OIDC `sub` claim. It is, however, possible to include the `oid` claim, which contains the same value for a given multi-app user across all applications; Entra ID’s `object-id`. Mendix recommends storing the `oid` claim in the `system.user.name` if you are using the OIDC SSO module with Entra ID.

### Keeping the Existing Logic and Extending

If your apps are not consistently storing the same identifier for a multi-app user in `system.user.name`, you can start using a new user attribute, new `UserCommons.namedUserIdentifier.value`.You can keep the existing application logic as it is, and in addition, store the selected user identifier value for a multi-app user in this new attribute.

### Paying Attention to Case Sensitivity

By design, the `system.user.name` is case sensitive and allows customers to store any user identifier without restrictions. If you choose to store an email address in these fields, or another identifier, that should be treated as case-insensitive. Mendix recommends downcasting such values to lowercase. The SAML, OIDC SSO, and SCIM modules already apply this for email claims/attributes received from your IdP.

For more information, refer to the [Versioning]() section below.

## User Classification

For accurate user metering, `External` users must be correctly classified. If they are not, your company may exceed the licensed capacity for `Internal` users, and Mendix may require you to acquire additional internal user licenses.
There are several approaches to classify users as `Internal` or `External`, ranging from configuration-only to custom development. These options are:

### IdP-Based User Classification

This method requires one of the following identity modules to be enabled: OIDC SSO, SCIM, or SAML. A key advantage of this approach is that it does not require any modifications to your existing app. Classification can begin immediately by setting a constant. However, because users are only classified when they log in, it may take some time before all end users are classified.

For more information, see the [IdP-Based User Classification](/developerportal/deploy/populate-user-type/#idp-based-user-classification) section of *Populate User Types*.

### Userrole-Based User Classification

The user-role-based user classification module classifies users by using the roles already defined in your app.  It can update all existing users in one run and works well if you already have separate roles for internal and external users. However, using this module requires upgrading your app to include the user classification module. For more information, refer to [User-Role-Based Classification](developerportal/deploy/populate-user-type/#user-role-based-user-classification).

### Custom User Classification

This method can be implemented either by using the [User Classification](/appstore/modules/user-classification/) module or by creating fully custom microflows. The main advantage of this approach is the flexibility it provides- you can apply any classification logic you choose while still relying on the user classification module as the base. However, this method requires developing custom logic and involves upgrading your app to a new version of your app to include the [user classification](https://marketplace.mendix.com/link/component/245015) module.

{{% alert color="info" %}}
In situations where there is ambiguity about the user classification, Mendix considers users as internal users. Ambiguity may happen in the following situations:

* A multi-app user is marked as external in one app and internal in another app.
* When applying userrole-based classification, and a user has both an Internal and an External userrole.
{{% /alert %}}

Classification by the platform is not supported by using your email domains, nor any other logic. Your app logic is responsible for classification. For more information, refer to [Custom Classification](/developerportal/deploy/populate-user-type/#custom-classification).

## (De)Activation of users