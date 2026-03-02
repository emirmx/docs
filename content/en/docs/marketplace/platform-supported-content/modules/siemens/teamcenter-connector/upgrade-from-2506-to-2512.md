---
title: "Upgrade Teamcenter Connector 2506.0.0 to 2512.0.0"
url: /appstore/modules/siemens-plm/upgrade-teamcenter-connector-2506-to-2512/
weight: 4
description: "Describes the steps to upgrade Teamcenter Connector 2506.0.0 to 2512.0.0 and discusses how breaking changes can be resolved."
---

## Introduction
This guide will help updating the Teamcenter Connector 2506.0.0 to 2512.0.0.

In this release of the Teamcenter Connector the Single Sign-On (SSO) is implemented differently, which means that in 2512.0.0 the OIDC module is no longer in use.

To see all the changes in this release please view the release notes of the [Teamcenter Connector](https://marketplace.mendix.com/link/component/111627).

## Download Teamcenter Connector and Extension
* Download the latest 2512.x version of the [Teamcenter Connector](https://marketplace.mendix.com/link/component/111627).
* Download the latest 4.x version of the [Teamcenter Extension](https://marketplace.mendix.com/link/component/225544).

## Resolve Breaking changes
Fist of all, resolve the breaking changes mentioned in the release notes:
* Replace `UserLogin` with `NAV_UserLogin`.
* Replace `ShowLoginPage` with `NAV_UserLogin`.
* Replace `LoginToMultipleTeamcenters` with `NAV_UserLoginMultipleActive`.
* Replace `AdminHomePage` with `NAV_AdminHomePage`.
* Remove `SSO_RegisterRequestHandlers` after startup microflow.

The following fundamentals have been changed:
* The `Admin` user role used to have login options (using the `AdminLogin` microflow):
in 2512 we have removed login functionalities for the `Admin` user role. If the `Admin` role of your application requires the option to login to Teamcenter, provide it with the TcConnector `User` module role.
* Removed TeamcenterConfiguration event handlers:
if you have custom logic for `TeamcenterConfiguration` objects, confirm it has the correct validations.
* Multiple microflows and pages moved from `Public` to `Private` folder:
check these are no longer directly referenced.

## Update Entity access
To resolve the `Entity access is out of date` error: 
* Go to the domain model and click the **Update security** button on the top bar of the domain model.
* Once updated, there might be some entities that now give the following error: `Non-persistable entity (entityName) contains access rules restricting members to have no access rights, which is not allowed.`
This is happening because of introduction of new attribute names and relations. Open the entity in question and give all attributes with no rights the read-only access right.

## Remove marketplace modules
The following marketplace modules are no longer used. These can be removed if they are not used by any other logic in your app:
* OIDC SSO module
* UserCommons module
* MxModelReflection module

## Use React client (optional)
Teamcenter connector 2512 is compatible with React Client. To enable this go to `Settings > Runtime > Use React client > Yes`. [Read more about React Client here.](https://docs.mendix.com/refguide/mendix-client/react/)

## Setup Teamcenter Configuration
Because of the new way that SSO is setup, your Teamcenter configuration needs to be set again when deploying a new app with Teamcenter Connector 2512.0.0. Don’t worry, the configuration is much easier than it was before. For setting up the configuration please follow the guide on [Setting up Teamcenter Connector configuration in 2512](/appstore/modules/siemens-plm/configuring-connection-2512).