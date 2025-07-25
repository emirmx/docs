---
title: "Data Accessibility and Security"
url: /catalog/manage/security/
description: "Describes security aspects around registered assets and access."
aliases:
    - /data-hub/data-hub-catalog/security/
    - /data-hub/data-hub-catalog/manage-data-sources/security/
    - /catalog/security/
---

## Introduction

In the Catalog, the [Access Level](#access-level) indicates whether you can access a registered service.

Security for a Mendix app can be defined at the app-level, module-level, and entity-level. You can also specify further authentication methods to control access to the data associated with published datasets.

This security level determines which end-users of the apps will have access to the data represented by the exposed dataset. For further information, see the [Security](/refguide/published-odata-services/#security) section in *Published OData Services*.

Access to data is determined by the identification protocols of the organization and applied to all access to the data via Mendix apps. For an example of custom HTTP header validation, see the [custom HTTP header validation](/refguide/security-shared-datasets/#http-header-validation) section of *Security and Shared Datasets*.

## Access Level of Registered Services {#access-level}

Registered services have the following classifications that apply to the visibility and accessibility of the service in the Catalog:

* **Public** – the service is visible to all internal and external users of the Catalog
* **Internal** – the service is restricted to the members of the organization

The **Access Level** of the asset indicates the runtime security on the service and what users can see and use when consuming datasets in their app development.

The access level for a registered service is shown in the **Service Metadata** panel in the Catalog.

## Read More

See [Security and Shared Datasets](/refguide/security-shared-datasets/) for further information on security and authentication for OData services.
