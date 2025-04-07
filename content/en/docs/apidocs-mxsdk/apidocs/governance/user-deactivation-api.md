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

## Examples

### Using the API to Retrieve User UUIDs

{{% alert color="info" %}}Only Mendix Admins from the company have the authority to retrieve user UUIDs.{{% /alert %}}

The following steps lead to retrieval of user's UUIDs of the email addresses provided in {emailAddresses}.

1. Set up your authentication PAT. You must be a Mendix Admin.
1. Create a request body containing the email addresses under `emailAddresses`. For example, to get user UUIDs of `jane.doe@domain.tld` and `john.doe@domain.tld`, provide a body like this:

    ```json
    {
      "emailAddresses":[
          { "emailAddress":"jane.doe@domain.tld" },
          { "emailAddress":"john.doe@domain.tld" }
      ]
    }
    ```

1. Call `GET /api/user-identifiers/v1/uuids` to get the UUIDs of the provided email addresses.

## API Reference

{{< swaggerui src="/openapi-spec/user-identifier-api.yaml"  >}}
