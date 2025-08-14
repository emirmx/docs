---
title: "Workflow Groups"
url: /refguide/workflow-groups/
weight: 40
---

## Introduction

A workflow group provides the means to group users for [user task targeting](/refguide/user-task#target-users).
The advantage of targeting users through groups is that it is a dynamic concept:
when users are added or removed from the group, the targeted users of a user task change accordingly.
This does not happen when targeting users directly.
For example, when a new user "John" is created and added to a group "Managers", he will instantly see all current tasks that are targeting this group.
Similarly, those tasks will disappear from his inbox when he is removed from the group.

## Workflow Group Properties

At design-time, each workflow group has the following properties:

- **Name**, the name of the group. The name can only contain letters, digits and underscores and cannot start with a digit.
- **Documentation**, a description of the group. The description can contain any free-form text.

### Workflow Group Entity

At run-time, a corresponding *WorkflowGroup* entity exists with the following attributes:

| Name | Type | Description |
| --- | --- | --- |
| *Name* | String(100) | The name of the workflow group. It must be unique. |
| *Description* | String | The description of the workflow group. |

In addition, it has the following associations:

| Name | Multiplicity | Description |
| --- | --- | --- |
| *WorkflowGroup_User* | * - * | The users that are part of the workflow group. |

## Configuration

To configure workflow groups, open **App** > **Settings** and select the **Workflows** tab:

{{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-groups-config.png" class="no-border" >}}

## Synchronization

Changes to workflow group properties at design-time will be automatically synchronized to the database at run-time, when the app is (re)deployed.
Exactly one *WorkflowGroup* object will exist for each defined workflow group,
where the *Name* attribute matches the **Name** property and the *Description* attribute matches the **Documentation** property.
When the name of an existing workflow group is changed, the existing object in the database is updated.

{{% alert color="warning" %}}
Workflow groups that are removed from the app will also be removed from the database upon start-up, including their associations to users.
The users themselves will not be removed.
{{% / alert %}}

{{% alert color="warning" %}}
The *Name* and *Description* attributes of a *WorkflowGroup* object should not be modified.
These changes would be overwritten in the next (re)deployment.
Renaming a group or changing its description should only be done from Studio Pro.
{{% / alert %}}

### Adding Users

Before workflow groups are useful, they must be populated with users.
This should be done by setting the *WorkflowGroup_User* association between the *WorkflowGroup* object and all *User* objects that should be in the workflow group.
The Workflow Commons modules provides default pages to do so.

## Using Groups in XPath

For each group that is defined an XPath token is defined as well, which you can use to select it.

For instance, to select the group called *Managers* you can use the following XPath constraint:

```[ id = '[%WorkflowGroup_Managers%]' ]```

Mendix does not recommend to use the following, because it will *not* be updated automatically when a group is renamed:

```[ Name = 'Managers' ]```
