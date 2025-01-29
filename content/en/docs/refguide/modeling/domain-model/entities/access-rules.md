---
title: "Access Rules"
url: /refguide/access-rules/
weight: 70
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

The access rules of an entity define what an end-user is allowed to do with objects of the entity. You can allow end-users with specific roles to do one or more of the following things:

* create objects
* delete objects
* view member values
* edit member values

A member is an attribute or an association of an entity.

You can also limit the set of objects available for viewing, editing, and removing using an [XPath constraint](/refguide/xpath-constraints/).

Every entity can have multiple access rules which are applicable to one or more [module roles](/refguide/module-security/#module-role). Each access rule grants certain access rights to those roles. Rules are additive, which means that if multiple access rules apply to the same module role, all access rights of those rules are combined for that module role.

{{% alert color="info" %}}
Access rules are not inherited from an entity's [generalization](/refguide/entities/#generalization), because the security for every entity is specified explicitly. So when adding an access rule to an entity, always make sure that all required XPath constraints are applied. If the entity has a generalization with access rules defining XPath constraints, these will not apply to its specializations and will therefore not limit its visibility.
{{% /alert %}}

{{% alert color="warning" %}}
The **System.User** entity has inbuilt access rules where access is given to its attributes if the end-user can manage the role of that User object. Specializations of **System.User** (such as **Administration.Account**) cannot change this access with their own access rules.
{{% /alert %}}

## Defining Access Rules

{{% alert color="info" %}}
In preparation for making the modernized access rule editor generally available, the features and interface were modified in Studio Pro version 10.17.0. The description below describes that version of the interface and does not fully describe the previous beta interface (Studio Pro versions 10.6.0 through 10.16.0).
{{% /alert %}}

{{% alert color="info" %}}
For Studio Pro versions 10.6.0 through 10.16.0, the modernized access rule editor worked with normalized access rules. A normalized access rule is an access rule that has exactly one module role attached to it. See [Access Rule Normalization](#normalization), below, for the implications when you switch to the new editor for those versions.
{{% /alert %}}

Access rules can be viewed via the **Access rules** tab of the entity properties dialog, which can be opened by double clicking on an entity in the domain model.

{{% alert color="info" %}}
The **Access rules** section is visible only if the [App Security](/refguide/app-security/) is set to **Production**.
{{% /alert %}}

An example of the access rules properties is represented in the image below:

{{< figure src="/attachments/refguide/modeling/domain-model/entities/access-rules/access-rules-properties.png" alt="Access Rules for Entities" width="700px" class="no-border" >}}

Access rules properties consist of the following sections:

* [Documentation](#documentation)
* [Module Roles](#module-roles)
* [Access rights](#access-rights)
* [XPath constraint](#xpath-constraint)

### Documentation Section {#documentation}

In **Documentation**, you can describe the intention of the access rule. This helps to keep access rules comprehensible, especially in the case of non-trivial XPath constraints.

### Rule Applies to the Following Module Roles Section {#module-roles}

{{% alert color="info" %}}
To apply an access rule to an entity, you need to have at least one of the following Access rights selected:

* Allow creating new objects
* Allow deleting existing objects
* An Entity Member (attribute or association) with `Read` or `Read, Write` rights
{{% /alert %}}

#### Roles

All module roles are listed, and those to which this access rule applies are checked. All end-users that have at least one of the checked module roles get the access rights that the rule defines.

#### Select / Deselect All

You can easily select, or deselect, all module roles using this checkbox.

### Access Rights {#access-rights}

The **Access rights** tab allows you to assign rights to end-users with the selected module roles.

#### Create and Delete Rights Section

##### Allow Creating New Objects

If **Allow creating new objects** is checked, end-users are allowed to create new objects of this entity. This is not restricted by any configured XPath constraints.

##### Allow Deleting Existing Objects

If **Allow deleting existing objects** is checked, end-users are allowed to delete existing objects of this entity.

The set of objects that can be deleted can be limited by using an [XPath constraint](#xpath-constraint).

#### Member Read and Write Rights Section {#member-access}

**Member read and write rights** define the access rights for every member ([attribute](/refguide/attributes/) or [association](/refguide/associations/)) of the entity. These access rights indicate whether end-users are allowed to view and/or edit the member's value. The set of objects to which these rights apply can be limited by using an [XPath constraint](#xpath-constraint).

| Value | Description |
| --- | --- |
| - | End-users are not allowed to view or edit the value of the member. |
| Read | End-users are allowed to view the value of this member, but cannot edit it. |
| Read, Write | End-users are allowed to view and edit the value of this member. |

{{% alert color="info" %}}
You cannot set *Write* access to attributes which are calculated. This includes both attributes where the attribute value is set to **Calculated** and attributes of type *Autonumber*.
{{% /alert %}}

**Default rights for new members** specifies the rights which are applied to new attributes or associations of this entity.

**Set all to** allows you to quickly set all the access rights for members to **None**, **Read**, or **Read, Write**.

For example, a customer is allowed to view the discount, but is not allowed to edit it. The access rights for the discount attribute are **Read**.

{{< figure src="/attachments/refguide/modeling/domain-model/entities/access-rules/access-rule-discount-read.png" class="no-border" >}}

See [Attribute Changes and Security Constraints](#attribute-changes), below, for important considerations about giving access to attributes.

#### XPath Constraint {#xpath-constraint}

An [XPath constraint](/refguide/xpath-constraints/) can be used to constrain the set of objects to which the access rule applies. If the constraint rule is true, the rule applies to that object. If the XPath constraint is empty, the rule applies to all objects of the entity.

Click **Edit…** to edit the XPath constraint.

{{< figure src="/attachments/refguide/modeling/domain-model/entities/access-rules/access-rule-xpath-tab.png" width="450px" class="no-border" >}}

{{% alert color="warning" %}}
XPath constraints can only be applied to persistable entities as they are applied by the database. Defining XPath constraints for non-persistable entities results in consistency errors.
{{% /alert %}}

There are two constraints that can be appended easily with a single button click. 

##### Owner

The **Owner** button adds an XPath constraint so the access rule is only applied if the object owner is the current end-user.

```java
[System.owner='[%CurrentUser%]']
```

This constraint is only valid when the [Store 'owner'](/refguide/entities/#store-owner) checkbox in the **System members** section of the entity properties is checked.

##### Path to User

The **Path to user...** button adds an XPath constraint so the access rule is only applied when a User object which is associated (directly or indirectly) with the current object is the current end-user. When you click **Path to user...**, you can select a path to an associated entity that is either a `System.User` or a specialization of `System.User`. This is then converted into an XPath constraint for the access rule.

For example:

1. Assume that the **Customer** entity is a specialization of the **User** entity. The **Order** entity is associated with the **Customer** entity via the **Order_Customer** association.
2. Assume that a logged-in customer is only allowed to view their orders, but is not allowed to view the orders of other customers.

The XPath constraint can be constructed easily using the **Path to user...** button by selecting the **Customer** entity in the **Order** entity access rule. The created rule will look like this:

```xpath
[Module.Order_Customer = '[%CurrentUser%]']
```

{{< figure src="/attachments/refguide/modeling/domain-model/entities/access-rules/access-rule-order-xpath.png" width="1000px" class="no-border" >}}

Because of this XPath constraint, access defined in the **Access rights** tab is only applied to orders for which the customer is the current end-user.