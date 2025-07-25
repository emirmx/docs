---
title: "Attribute Type Migration"
url: /refguide10/attributes-type-migration/
weight: 10
---

## Introduction

Mendix allows you to change attribute and association types on existing domain models. This document explains the consequences of doing this.

## Data Type Changes on Existing Attributes

### Data Type Change Behavior

If the type of an existing attribute is changed in Mendix Studio Pro, the existing attribute will usually be deleted and a new attribute will be created. For some attribute type changes, Mendix tries to convert existing data in the database to the new type.

If data should NOT be converted to the new type, you must remove the attribute in Studio Pro and create a new attribute (with the same name). If you change the type and rename the attribute, Mendix remembers the old attribute name and will try to convert the attribute values if possible.

### Conversion Table

The table below shows, for each data type change, whether Mendix will convert the values.

Key | Means
--- | ---
**&#x2713;** | Conversion always possible.
**\*<sup><small>note</small></sup>** | Conversion is not always possible, or data will be changed during conversion. See related note for more information. If conversion is not possible, the behavior is the same as for "**X**", below.
**X** | Conversion not possible. The original database column will be removed and a new column will be created with default values for the existing rows.

{{< figure src="/attachments/refguide10/runtime/data-storage/attributes-type-migration/conversion-table.png" alt="Table of conversions - click to enlarge" class="no-border" >}}
(*Click the image to enlarge*)

### Manual Conversion

Even if Mendix cannot convert the values of a specific attribute to another type, you can still manage that manually. Change the name of the attribute, for example append the text 'Deleted' to its name. Create a new attribute with the same name and the new data type. Look up each occurrence of the old (renamed) attribute in the whole model and change this to the new attribute. Be sure that there is no microflow or page anymore which refers to the old attribute.

Create a microflow in which you retrieve all objects of the entity, loop through the objects and for each object, read the value of the old attribute, convert the value, store it in the new attribute and commit the object. Place a button on an administrator page which calls this microflow.

When you deploy, you have to run this microflow one time, after which you can remove both the microflow and the button pointing to it, and then you can also remove the old attribute.

## Association Type Changes on Existing Associations

When you change the type of an association from many-to-many to one-to-many or from one-to-many to one-to-one, be aware that duplicate associations are not automatically removed from the database. For example, a one-to-many association from entity A to entity B allows multiple references: a1 to b1, a1 to b2, etc. One-to-one associations only allow a single reference per object: a1 to b1. In this situation, the new version of your app will fail to start with an error message indicating a unique constraint violation. You will first have to clean up your data before you can deploy the new version with the changed association type.
