---
title: "Anonymous Users"
url: /refguide10/anonymous-users/
weight: 40
---

## Introduction

You can use anonymous users to allow end-users access your application without having to sign in. You can restrict the data that anonymous users can view and access by assigning a specific user role to them. 

## Anonymous Users Properties {#properties}

Open **App Security** > the **Anonymous users** tab to access the properties:

{{< figure src="/attachments/refguide10/modeling/security/app-security/anonymous-users/anonymous-users-tab.png" class="no-border" >}}

The properties of anonymous users are described in the table below:

| Property              | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| Allow anonymous users | When **Yes** is selected, anonymous users are allowed. End-users do not have to sign in to access the application. <br />When **No** is selected, anonymous users are not allowed. End-users have to sign in to access the application. |
| Anonymous user role   | The user role that end-users of your application have when they are not signed in. This tells the application which role should be automatically applied to anonymous users who access the app. The **Allow anonymous users** property should be set to **Yes** to select an anonymous user role. |

## Read More

* [App Security](/refguide10/app-security/)
* [User Roles](/refguide10/user-roles/)
* [Administrator](/refguide10/administrator/)
* [Demo Users](/refguide10/demo-users/)
* [Password Policy](/refguide10/password-policy/)
