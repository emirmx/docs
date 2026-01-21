---
title: "Preventing Duplicate Users When Enabling SCIM with the OIDC SSO Module"
url: /appstore/modules/scim/with-oidc
linktitle: "Using SCIM with OIDC"

description: "Describes use cases for preventing duplicate users when enabling SCIM with the OIDC SSO module."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Consider a scenario where the customer is already using OIDC or SAML for single sign-on (SSO) and plans to enable the SCIM module for automated user provisioning. In this scenario, existing users must be updated rather than duplicated when they are provisioned through SCIM. To ensure this behavior, the Identity Provider’s (IdP) unique identifier (claim) used by the OIDC or SAML module is mapped to a specific attribute in the Mendix user entity. When configuring the SCIM module, the same unique identifier must be mapped to the same Mendix entity attribute. This ensures that OIDC/SAML and SCIM all reference the same Mendix user record, allowing SCIM to update existing users instead of creating new ones.
The following decision flow diagram shows how to choose the correct identifier-mapping strategy when enabling SCIM for an existing OIDC-enabled Mendix application.

{{< figure src="/attachments/appstore/platform-supported-content/modules/scim/using-with-oidc/overview.png" >}}

### Scenario 1: Aligning OIDC and SCIM Identifiers to Update Existing Users

This scenario applies when users have already authenticated to the Mendix application using OIDC and the customer subsequently enables SCIM for user provisioning. When a user first logs in via OIDC, the Identity Provider (IdP) provides a unique identifier, such as the `sub` claim (for example, `00u12abcD3XYZpqRs5d6`).

The typical example of this scenario includes Okta as an IdP and using `sub` as a unique identifier. In the OIDC **Attribute Mapping** configuration, the unique IdP claim `sub` is stored in the `System.User.Name` attribute of the Mendix user entity, and the `Name` attribute is configured as the OIDC principal attribute. This establishes the `sub` value as the authoritative identifier for the user in Mendix.

To ensure that SCIM provisions and updates the same user rather than creating a duplicate record, the SCIM configuration must reference the same identifier. In IdPs such as Okta, the SCIM `externalId` attribute contains the same value as the OIDC `sub` claim.

In the **Attribute Mapping** of Entra Id portal, set the **Source attribute** of `externalID` to `objectId`.  In the case of Okta, `sub` is the default for `externalID`.

By mapping the SCIM `externalId` to the `System.User.Name` attribute and configuring `Name` as the SCIM principal attribute enable Mendix to correctly correlate SCIM provisioning requests with existing users. With this configuration in place, when users are provisioned or updated via SCIM, Mendix identifies the matching user record based on the shared identifier and updates the existing user instead of creating a new one.

| Protocol | Identifier | Value | Principal attribute |
| --- | --- | --- | --- |
| OIDC | sub | 00u12abcD3XYZpqRs5d6 | Name |
| SCIM | externalId | 00u12abcD3XYZpqRs5d6 | Name |

{{< figure src="/attachments/appstore/platform-supported-content/modules/scim/using-with-oidc/scim-principal-attribute.png" >}}

If you are using Entra ID as IdP, the unique IdP claim `oid` is stored in the `System.User.Name` attribute of the Mendix user entity, the `Name` attribute is configured as the OIDC principal attribute. By mapping the SCIM `externalId` to the `System.User.Name` attribute and configuring `Name` as the SCIM principal attribute enable Mendix to correctly correlate SCIM provisioning.
