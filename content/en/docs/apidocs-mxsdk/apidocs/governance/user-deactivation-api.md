---
title: "User Deactivation API"
linktitle: "User Deactivation API"
url: /apidocs-mxsdk/apidocs/user-deactivation-api/
type: swagger
description: "The User Deactivation API allows Mendix Admins to deactivate users within their company on the Mendix Platform."
restapi: true
weight: 112
---

{{% alert color="warning" %}}
The User Deactivation API is available for Mendix Admins.
{{% /alert %}}

## Introduction

The User Deactivation API allows Mendix admins to deactivate users within their company on the Mendix Platform. You can use this API to automate the Joiner, Mover, and Leaver (JML) processes. It can help manage access by revoking it for certain 'movers' and 'leavers' from the Mendix platform. Using this API may also help your company to contribute to compliance with its policies.

Note that this API only manages access to Mendix as a development platform. If you need to implement JML processes for end-users in your Mendix apps, it is recommended to add the [SCIM](/appstore/modules/scim/) module to your applications.

Once you have deactivated users, they will no longer be able to log in to the Mendix platform or use the Mendix platform API with a Personal Access Token (PAT).

As an alternative, you can use this API to deactivate platform users, instead of the deprecated User Management API.

## Authentication

Authentication for the User Deactivation API uses a personal access token (PAT).

### Generating a PAT

To generate a PAT, see the [Personal Access Tokens](/community-tools/mendix-profile/user-settings/#pat) section of *User Settings*.

Select the following as **User Deactivation API** scopes:

* `mx:user-deactivation:write` – to deactivate users

Store the generated value `{GENERATED_PAT}` somewhere safe so you can use it to authorize your User Deactivation API.

### Using the PAT

Each request must contain an `Authorization` header with the value `MxToken {GENERATED_PAT}`. Here is an example:

```http
PATCH /v1/platform-users/user-status/{uuid} HTTP/1.1
Authorization: MxToken EKNJ…vk
```

To authenticate calls when using the Open API specification below, click **Authorize** and use the value `MxToken {GENERATED_PAT}`.

## Prerequisites

You must have the UserID of the user you want to deactivate. Follow the steps below to retrieve the UUID of the user:

1. The Mendix Administrator creates a Personal Access Token (PAT) via the Developer Portal, with the following scope:
`mx:mxid3:user-identifiers:uuid:read`
2. Invoke the User Identifier API to fetch the UUID based on the user's email address, using the PAT generated in the above step.

## Examples

### Using the API to Deactivate User

{{% alert color="info" %}}Only Mendix Admins from the company have the authority to deactivate user.{{% /alert %}}

The following steps lead to deactivate the user based on UUID provided as in {UUID}:

1. Set up your authentication PAT. You must be a Mendix Admin.
1. Create a request body containing the active status, provide a body like this:

    ```json
    {
     "active" : false
    }
    ```

1. Call `GET /v1/platform-users/user-status/{UUID}` to deactivate the User with the provided {UUID}.

## API Reference

