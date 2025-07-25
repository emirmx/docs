---
title: "OData Representation"
url: /refguide/odata-representation/
weight: 60
---

## Introduction

This document describes how entities are represented in a published OData service.

## Attributes {#attributes}

| Mendix Data Type | Edm Type | Attribute Value | Atom XML Representation |
| --- | --- | --- | --- |
| ID ¹| Edm.Int64 | 3940649673954387 | 3940649673954387 |
| AutoNumber | Edm.Int64 | 1 | 1 |
| Binary (not supported) ² |   |   |   |
| Boolean | Edm.Boolean | true | true |
| Date and time | Edm.DateTimeOffset | Fri, 19 Dec 2014 10:27:27 GMT | 2014-12-19T10:27:27.000Z |
| Enumeration | Enumeration⁴ (OData v4) or Edm.String (OData v3) | Color.Blue | Blue |
| Big decimal  | Edm.Decimal | 0.3333333333333333333333333333333333 | 0.3333333333333333333333333333333333 |
| Hashed string | Edm.String | HashPassword | HashPassword |
| Integer  | Edm.Int64 | 50 | 50 |
| Long ¹ | Edm.Int64 | 3940649673954387 | 3940649673954387 |
| String ³ | Edm.String | John | John |

<small>¹ When using Excel to import an OData source, long numbers may seem cut off. This is due to a restriction in the data type Microsoft uses. For more information, see [Last digits are changed to zeroes when you type long numbers in cells of Excel](https://support.microsoft.com/en-us/kb/269370).<br />
² Even though the binary data type is not supported, the FileDocument and Image system entities are supported and represented as Base64-encoded strings with the `Edm.Binary` type.<br />
³ When the string attribute has a limited length, the `MaxLength` attribute is specified. </small>
⁴ In Studio Pro 9.23 and below, all enumeration attributes were published as strings.

Additionally, the `updated` field for an entry in OData comes from the system changedDate attribute of an entity. If this attribute is not available (because it is not published, the user does not have access rights, or it is empty in database), the default date (1-1-1970) will be used.

### Representation of ID {#id-representation}

Each entity has an ID, which is not shown as an attribute in the domain model. This is indicated in the service's metadata.

In OData 4, IDs are annotated with vocabulary annotation `Com.Mendix.IsAttribute` with value `false`. The term for this vocabulary annotation is included in the metadata.

In OData 3, IDs are marked with `isAttribute="false"`, using a Mendix-specific XML attribute in the `http://www.mendix.com/Protocols/MendixData` namespace.

## Associations {#associations}

In the settings of the OData service, you can choose how associations are represented. There are two options, which are described below.

### As a Link

When you represent associations as links, the data of associated objects can be included in the response by using the `$expand` query parameter. For more information, see the [Retrieving Associated Objects](/refguide/supported-odata-operations/#retrieving-associated-objects) section of *Supported OData Operations*.

This means you can only publish an association when the entity on the other side is published in this service as well. This also means that you cannot publish the same entity more than once in the same service (because in that case, it would not be clear where the link should point to).

Using this method, you can publish both sides of the association and you can publish many-to-many associations.

### As an Associated Object ID

When you choose to represent associations as an associated object ID, the ID of the associated object is represented as an `Edm.Int64` property. If the association refers to more than one object, you cannot publish it from that side.
