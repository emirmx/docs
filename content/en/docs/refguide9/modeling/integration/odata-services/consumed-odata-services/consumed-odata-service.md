---
title: "Consumed OData Service"
url: /refguide9/consumed-odata-service/
weight: 10
aliases:
    - /refguide9/consumed-odata-service-properties
---

## Introduction

When an external entity is used in an app module through the [Data Hub pane](/refguide9/data-hub-pane/), a consumed OData service document is added specifying the details of the consumed service. This is the API to the publishing app and the data associated with the entity.

## Consumed OData Service screen

The **Consumed OData Service** document contains the following information:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/consumed-odata-doc-connection-tab.png" alt="Connection Tab" class="no-border" >}}

* Service name and the icon for the source application of the originating app
* Version number of the consumed service
* **View in Catalog** link to the **Service Details** where you can see the full service details that are registered
* **Update/Switch** – you can update the consumed service contract to another version that has been detected in the [Catalog](/catalog/) for the same app and service; the button will show the following, depending on what has been returned for the consumed contract in the Catalog:
    * **Update** – this button is displayed so that you can **Update** the contract that is currently consumed (and shown in the **Consumed OData Service** document). You will be presented with the contract that is currently at the service endpoint. It is good practice that only minor, non-breaking changes are deployed to the same endpoint.
    * **Switch** – this button is shown if other registered instances of the same service (with the same name, from the same app) are available in the Catalog and are deployed to different endpoints (for example, to another environment or because of changes that would break existing apps consuming the previous version)

    {{% alert color="info" %}} Studio Pro will always show the **Update** option for the **Consumed OData Service** where you can check if an update is available. In the Data Hub search and **App**  pane, when a different contract is detected at the service endpoint, this will be indicated with an update arrow for the service. For further information on updating and switching services see the [Updating or Switching a Consumed OData service](#updating) section of this document. {{% /alert %}}

    {{% alert color="info" %}}In the **Data Hub** pane, consumed services have an **Update** icon (a blue arrow) if they have an update available.{{% /alert %}}

### Service URL {#service-url}

The **Service URL** displays the URL of the service endpoint:

* Click **Select** to select another [constant](/refguide9/constants/) for the service
* Click **Show** to display the **Constant** dialog box displaying the service URL or endpoint:

    {{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/constant.png" class="no-border" >}}

### Timeout

**Timeout** is the response time for fetching the data from the service endpoint. If the endpoint has not responded after the number of seconds in **Timeout (s)**, an exception will occur. If this happens during a microflow activity, the microflow will roll back or go into your custom [error handling](/refguide9/error-handling-in-microflows/).

Default value: *300 seconds*

### Proxy Configuration

**Proxy configuration** allows you to configure whether to use a proxy for the request:

* **Use app settings** – use settings which are defined at the app level (default)
* **Override** – override the app-level settings for this action by specifying the host, port, user name, and password settings for the proxy
* **No proxy** – do not use a proxy for this service, even if there is a proxy configured at the app level

{{% alert color="info" %}}
In most cases, this setting can be ignored, and the default **Use app settings** can be used.
{{% /alert %}}

### Authentication {#authentication}

The **Use HTTP authentication** checkbox specifies if basic authentication should be used. If checked, you have to specify the following details:

* **User name** – defines the user name that will be used for authentication
* **Password** – defines the password that will be used for authentication

Input these as a string with single quotes.

Besides basic authentication, you can use custom authentication. For more information, see the [HTTP Headers](#http-headers) section below.

### HTTP Headers {#http-headers}

You can specify additional HTTP request headers to be passed to the endpoint in this list by clicking **Add**, **Edit**, or **Delete** for custom HTTP headers for authentication. Each custom header is a pair with a key and a value.

Using **Headers from a Microflow**, you can specify a microflow for creating a key and value pair(s) for dynamic values. The microflow must return a list of **System.HttpHeader** objects.

{{% alert color="info" %}}
For more flexible HTTP request headers, you can select a microflow that returns a list of **System.HttpHeader**. This microflow may take a parameter of type **System.HttpResponse**. The microflow is called every time a request is made. Initially, the HTTP response parameter will be empty. If the response is **401 Unauthorized**, the microflow is called with that HTTP response and another call is made with the new HTTP headers.
{{% /alert %}}

{{% alert color="info" %}}
Custom authentication can be done with the microflow where the authentication value is retrieved (such as SSO). For further information on access and authentication, see [Using Custom HTTP Header Validation for Published Entities](/refguide10/security-shared-datasets/#http-header-validation) in the *Security and Shared Datasets* document in the Studio Pro 10 guide.
{{% /alert %}}

#### Authenticating with Mendix SSO {#authenticate-mendix-sso}

Publishers can set up [Custom](/refguide9/published-odata-services/#authentication-microflow) authentication using [Mendix SSO](/appstore/modules/mendix-sso/) module. (This is explained in more detail in the [Mendix SSO](/refguide9/published-odata-services/#authentication-mendix-sso) section of *Published OData Services*). 

Consumers of an OData service that is set up with [Mendix SSO](/appstore/modules/mendix-sso/) authentication can do the following:

* Use the **CreateAccessTokenAuthorizationHeaderList** from the module as the [headers from a microflow](#http-headers) (see above).

To learn more about how to publish an OData service with authentication (Mendix SSO, or other methods), see the [Authentication Methods](/refguide9/published-odata-services/#authentication-methods) section of *Published OData Services*. 

To learn more about using external entities with security enabled (in production environments), see the [Authentication](/refguide9/external-entities/#authentication) section of *External Entities*.

### Error Handling Microflow

When a call to the OData service fails, users will see a generic error message. Create an error handling microflow to change this message.

When the service responds with an unsuccessful status code (not in the 2XX range), or does not return a response at all, then this microflow decides which message to show to the user.

The microflow should have an argument of type `System.HttpResponse`. If the OData service returns a response, the argument has a value, otherwise it is `empty`.

The microflow must return a `String` containing the error message. If it returns `empty`, the original generic message is used.

Note for developers of Java actions: the message returned by the error handling microflow can be caught as a [UserException](https://apidocs.rnd.mendix.com/9/runtime/com/mendix/systemwideinterfaces/core/UserException.html).

{{% alert color="info" %}}
The *error handling microflow* feature was introduced in Studio Pro 9.6.0.
{{% /alert %}}

### Metadata

When you create a consumed OData service, the metadata editor allows you to open an OData contract from a file or URL. When you have already consumed a contract, you can click the **Update** button to update the existing contract with a new version from file or URL.

To open the **Metadata Editor**, click **Update**. In the editor, you can specify a URL or file for the metadata:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/metadata-editor.jpg" alt="Metadata Editor" class="no-border" >}}

The following settings are available:

* **Import from** – select **URL** or **File** for the location of the metadata:
    * **URL** – specify the URL for the metadata
    * **File** – click **Browse** to select an XML metadata file

When downloading the metadata from a URL, the server may request a username and password (basic authentication). In that case, a dialog box will prompt you to enter your username and password. If the metadata file refers to other metadata files on the same server within the same realm, the username and password are re-used.

{{% alert color="info" %}}
This information is not stored, so if you download the metadata from the same server again, you will have to enter your username and password again.
{{% /alert %}}

When you import the metadata, you can add external entities from the consumed OData service in the [Data Hub Pane](/refguide9/data-hub-pane/).

### Properties

Click the **Properties** tab for the consumed OData service which displays the properties that were defined for the OData service document and the following additional properties:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/consumed-odata-service-doc-properties.png"   width="300"  class="no-border" >}}

* **Entities** – The URL of the metadata defining the entities and associated datasets.
* **Documentation** – An additional description about this service for the current app.
* **Service name** – The name of the published OData service that is consumed.
* **Service version** – The version of the service that is consumed.
* **Service ID** – The unique identifier of the service in the Catalog.
* **Application ID** – The unique identifier of the application that the service was published from in the Catalog.
* **Metadata** – The contents of the metadata file defining the service.
* **OData version** – The OData version (can be v3 or v4).
* **Use QuerySegment** – When set to `No`, the application retrieves data using a `GET HTTP` method and places data query arguments in the URL's query string. When set to `Yes`, then a `POST HTTP` method is used, `/$query` is appended to the resource path of the URL, and the query string is provided as the request body. This enables limiting the length of the URL and avoiding potential problems with the systems involved. This feature is not available for OData v3 or if the consumed service explicitly indicates that it is not supported. For details, see [Passing Query Options in the Request Body](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html#sec_PassingQueryOptionsintheRequestBody) in the OData specification. This property was introduced in Studio Pro 9.6.0.

## Updating or Switching a Consumed OData Service {#updating}

### Consuming from Service Endpoints{#consume-service-endpoints}

When you add an external entity to your app, you are consuming the entity from a specific version of a service (the *service endpoint*), deployed to a given environment. The metadata file or contract for the service is located at this endpoint.

The same service, deployed to a different environment will be to a different service endpoint and this will be registered as a different asset in the Catalog. In the following example, there are three endpoints for the **Sales 1.0.0** which is deployed to the production environment and the **Acceptance** and **Test** environments:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/same-service-different-endpoints.png" alt="2 endpoints"   width="250"  class="no-border" >}}

When you drag the **Customer** entity from **CustomerApi version 1.0.0** deployed to the **Acceptance** environment into your app, Studio Pro will retrieve the information it requires from the contract that is at that endpoint.

### Semantic Numbering for Service Versions {#semantic}

It is important that the publishers of the services adopt a strict revision process for any changes they make to their published OData services that are consumed by other users.

Mendix recommends using a strict versioning system (for example, semantic numbering) when issuing updates to services. The service version should clearly indicate the level and severity of the changes that have been made when a service is updated and deployed according to the following guidelines.

#### Minor Service Updates

*Minor* service updates are, for example, additional fields added to the service or new operations included which would not break any apps that consume the previous versions.

If semantic numbering is used then a minor/non-breaking change to a service can be indicated by an increase in the decimal part of the version number. For example, 1.0.11, 1.0.12, 1.1, 1.2.

Minor service updates can be deployed to the same service endpoints, thereby ensuring that all consuming apps consume the latest version of the service.

#### Major Service Updates

*Major* service updates are for example, when entities or attributes are removed, or input parameters are required, which would be incompatible for the consuming apps and result in the consuming app "breaking".

When a major change has been made to a published service, Mendix recommends that the service is deployed to a different endpoint with the new service version number clearly indicating that there has been a major change—with semantic numbering this would be an incremental increase of a whole number.

In this case the new service should be registered in the Catalog as a different service, and show up in the catalog as a separate asset. In the following example, there are 4 registered occurrences of the **OrderManagementService**:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/consume-major-service-update-version.png" alt="4 endpoints"   width="250"  class="no-border" >}}

There is a major service update indicated by the change in the version number from **1.0.0** to **2.0.0**. Further, both major versions have also been deployed to **Acceptance** which also results in separately registered assets in the Catalog at different endpoints.

{{% alert color="info" %}}
Entities of non-Mendix OData services are identified with a key of one or more fields. If the key fields are changed in an update of the service, this will also be seen as a breaking change.
{{% /alert %}}

### Update or Switch {#update-switch}

When a change in the contract at a consumed endpoint is detected (possibly due to a minor change), or if the same named service is deployed with a major version number but to a different endpoint, the following options are available in the **Consumed OData Service** screen.

#### Update

The **Update** option is available when Studio Pro detects that the contract at the Catalog endpoint is different to the one currently consumed in the app. If the **Update** option is selected, the new contract will be loaded in the App.

See the [Limitations](/refguide9/consumed-odata-services/#consumed-odata-service-limitations) section of *Consumed OData Services* to read about known update limitations.

##### Data Hub Pane

In the **Data Hub** pane, in search results and in the **Used in your App** section an update arrow indicate if there is a different contract at the Catalog endpoint:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/update-available.png" alt="update service app-pane" class="no-border" >}}

* The service version that is *currently consumed* is shown (in this example **1.0.0**)
* Blue **Update** - click to open the **Update Service** box and update the contract to the new one. Studio Pro will retrieve the new contract at the Catalog endpoint and this will be loaded in the app.
* The list of entities in this new version in the Catalog are shown, including the locally consumed entities which are marked with a green check-mark. These entities are, however, greyed out to indicate that they cannot be dragged into the domain model as the contract for the previous version is currently being consumed. The only option is to click **Update** to retrieve the updated OData Service.

##### Update Service Dialog Box

When you click **Update** on the **Consumed OData Service** document or the update icon in the **Data Hub** and **App** sections, the **Update** dialog box is displayed.

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/update-service-dialog-box.png" alt="update service dhpane" class="no-border" >}}

The consumed OData service that is currently consumed in the app (**1.0.0**) is shown on the left, and you can click **Update** to retrieve the new contract from the Catalog (**2.0.0**).

#### Switch

When an OData service is published to a different endpoint or to a different environment this will mean that it will be registered as a different asset in the Catalog.

In the example given in the [Consuming from Service Endpoints](#consume-service-endpoints) section above, if you are consuming the service from the **Acceptance** environment, the Consumed OData service screen will display the **Switch** button to enable you to consume the same service from the **Production**.

#### Switching Consumed Services

A published OData services that is deployed to multiple environments or is published with major service updates (and therefore deployed to a different endpoint) will be shown as separate items in the search results of the **Data Hub** pane.

In the following example, the consumed **Orders** version **1.0.0** deployed to **Test** environment is consumed in the app. However, the same service is deployed to the **Acceptance** environment:

{{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/consume-major-service-update.png" alt="major change environment" class="no-border" >}}

To consume the service deployed to the **Acceptance environment**, follow these steps:

1. Click  **Update** > **Switch** on the **Consumed OData Service** document:

    {{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/update-switch.png" alt="major change environment" class="no-border" >}}

2. On the **Switch** dialog box, from the drop-down list, select the service that you want to consume from (note that an endpoint is also detected that is deployed to **Production**) and click **Switch**:

    {{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/switch-environment.png" alt="major change environment" class="no-border" >}}

3. The consumed service is be consumed from the new selected environment. The information on the **Consumed OData Service** document will display the changed service details and the **Data Hub** pane now displays that you are consuming from the selected environment:

    {{< figure src="/attachments/refguide9/modeling/integration/consumed-odata-services/consumed-odata-service/switch-new-environment.png" alt="major change environment dh pane"   width="300"  class="no-border" >}}

## Read More

* [Data Hub Pane](/refguide9/data-hub-pane/)
* [Consumed OData Services](/refguide9/consumed-odata-services/)
