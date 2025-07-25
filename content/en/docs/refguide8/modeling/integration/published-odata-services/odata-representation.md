---
title: "OData Representation"
url: /refguide8/odata-representation/
---

This document describes how entities are represented in a published OData service.

## Attributes

| Mendix Data Type | Edm Type | Attribute Value | Atom XML Representation |
| --- | --- | --- | --- |
| Id ¹ ²| Edm.Int64 | 3940649673954387 | 3940649673954387 |
| Autonumber | Edm.Int64 | 1 | 1 |
| Binary (not supported) ³ |   |   |   |
| Boolean | Edm.Boolean | true | true |
| Date and time | Edm.DateTimeOffset | Fri, 19 Dec 2014 10:27:27 GMT | 2014-12-19T10:27:27.000Z |
| Enumeration | Edm.String | Color.Blue | Blue |
| Big decimal  | Edm.Decimal | 0.3333333333333333333333333333333333 | 0.3333333333333333333333333333333333 |
| Hashed string | Edm.String | HashPassword | HashPassword |
| Integer  | Edm.Int64 | 50 | 50 |
| Long ² | Edm.Int64 | 3940649673954387 | 3940649673954387 |
| String ⁴ | Edm.String | John | John |

<small>¹ In the service's metadata, IDs are marked with `isAttribute="false"` (using a Mendix-specific XML attribute in the `http://www.mendix.com/Protocols/MendixData` namespace) in order to indicate that these are not attributes that appear in the domain model. Note: the `isAttribute="false"` feature was introduced in Studio Pro [8.6.0](/releasenotes/studio-pro/8.6/#860).<br />
² When using Excel to import an OData source, long numbers may seem cut off. This is due to a restriction in the data type Microsoft uses. For more information, see [Last digits are changed to zeroes when you type long numbers in cells of Excel](https://support.microsoft.com/en-us/kb/269370).<br />
³ Even though the binary data type is not supported, the FileDocument and Image system entities are supported and represented as Base64-encoded strings with the `Edm.Binary` type.<br />
⁴ When the string attribute has a limited length, the `MaxLength` attribute is specified. Note: this feature was introduced in Studio Pro [8.16.0](/releasenotes/studio-pro/8.16/#8160).</small>

Additionally, the `updated` field for an entry in OData comes from the system changedDate attribute of an entity. If this attribute is not available (because it is not exposed, the user does not have access rights, or it is empty in database), the default date (1-1-1970) will be used.

## Associations {#associations}

In the settings of the OData service, you can choose how associations are represented. There are two options, which are described below.

### As a Link

When you choose to represent associations as links, each object contains a link for each of its associations. The associated object(s) can be retrieved via those links.

This means that you can only expose an association when the entity on the other side is a resource of this service as well. This also means that you cannot publish the same entity more than once in the same service (because in that case, it would not be clear where the link should point to).

Using this method, you can expose both sides of the association and you can expose many-to-many associations.

### As an Associated Object ID

When you choose to represent associations as an associated object ID, the ID of the associated object is represented as an `Edm.Int64` property. If the association refers to more than one object, you can not expose it from that side.
