---
title: "User Task"
url: /refguide/user-task/
weight: 70
---

## Introduction

User task allows you to assign a workflow task to a certain user or a group of users.

For example, you can add filters and assign a task to users with the Manager role:

{{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/user-task/user-task.jpg" alt="User Task Example" width="200" class="no-border" >}}

When the workflow reaches the user task activity, the task is created and will be displayed in the Task inbox if you are using the **Workflow Commons** module:

{{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/user-task/task-inbox.jpg" alt="Task Inbox" class="no-border" >}}

## Properties

User task properties consist of the following sections:

* [General](#general)
* [Due date](#due-date)
* [Targeted users](#users)
* [Outcomes](#outcomes)
* [Task page](#task-page)
* [Display information](#display-info)
* [Events](#events)
* [Boundary events](#boundary-events)
* [Common](#common)

### General Section {#general}

**Caption** defines a title of the user task.

### Due Date Section {#due-date}

**Due date** is stored in the System module on the **UserTask** entity as an attribute and its data can be dynamically displayed in the running app. For example, you can use it to set a deadline for the user task and display it in your app. However, this is not an automatic reminder but rather a deadline you reference when keeping track of the user task. If you are using the **Workflow Commons** module, **Due date** is used in page templates and preconfigured dashboards.

The **Due date** section properties are described in the table below:

| Property | Description |
| --- | --- |
| None | No due date is set. |
| Duration | You can set the deadline for the user task with the **Due in** option, which indicates the number of hours, days, or weeks the task is due in. Possible values of the property are the following ones:<br /><ul><li>Hours</li><li>Days</li><li>Weeks</li> </ul> |
| Expression | You can set a due date for the user task writing an expression. For example, to set a due date to tomorrow, you can use `addDays([%CurrentDateTime%], 1)`. |

### Targeted Users Section {#users}

#### Target Users {#target-users}

**Target users** allows you to manage what users will see a user task in their inbox.

There are three different ways to do this:

- Target **User(s)** directly. You can use either an XPath or a Microflow, that result in a list of one or more **User Entity** objects.
  These objects will be stored in the *WorkflowUserTask_TargetUsers* association, after which the user task will show up in the task inbox of those users.
- Target users indirectly through **Workflow group(s)**. You can use either an XPath or Microflow, that result in a list of one or more *WorklowGroup* objects.
  These objects will be stored in the *WorkflowUserTask_TargetGroups* association, after which the user task will show up in the task inbox of any user that is associated to those workflow groups.
- Perform **No targeting** when the user task is created. Instead, you must set the targets manually by settings either *WorkflowUserTask_TargetUsers* or *WorkflowUserTask_TargetGroups* (or both) yourself.
  Setting these associations can be done from the **On created** event handler or at some other time after the user task is created, for instance by an administrator.

{{% alert color="info" %}}
Group targeting exists as of Mendix 11.2.0 as an experimental feature.
As such you must enable it in the [New Features Tab](/refguide/preferences-dialog#new-features) of the [Preferences Dialog](/refguide/preferences-dialog).
See also [Workflow Groups](/refguide/workflow-groups).
{{% /alert }}

It is important to note that there is a fundamental difference between targeting **User(s)** and **Workflow group(s)**:

- Targeting **User(s)** is static and associates only those users that match the targeting criteria at the time the user task is created.
  After that, the targeted users are *not* updated when new users are created (or existing users are modified), even if these would match the criteria.
- Targeting **Workflow group(s)** is not static and will dynamically change when users are added or removed to the targeted groups.
  Note that the targeted groups themselves are still static and will not be changed when new groups are added.

Possible options are described in the table below:

| Target | Source | Description |
| --- | --- | --- |
| User(s) | XPath | Allows you to filter which users should see the user task in their inbox. For example, you can target a certain task only to users in a specific department (assuming such information is avalable in your domain model). You can use any attributes of the **User Entity** set in [App Settings](/refguide/app-settings/#workflows).|
| User(s) | Microflow | Allows you to target the user task to certain users. Using a microflow you can check, for example, which users have the right to approve user tasks and are currently not on vacation and assign the task only to users who passed the check. <br/> The return type of the microflow should be a list of the **User Entity** set in [App Settings](/refguide/app-settings/#workflows). |
| Workflow group(s) | XPath | Allows you to filter groups of users who should see the user task in their inbox. For example, you can target a specific *Managers* group, that associates only users with a management role. Use the following XPath: `[ id = '[%WorkflowGroup_Managers%]' ]`. |
| Workflow group(s) | Microflow | Allows you to target the user tasks to specific groups. Using a microflow you can check, for example, which groups are associated to a specific department (assuming such information is available in your domain model). <br/> The return type of the microflow should be a list of *WorkflowGroup*.  |
| No targeting | - | Allows you to not immediately target the user task. This can be useful when you, for example, want the user task to be created but have an administrator assign it to certain users and/or groups later. It should also be used if you want to have mixed targeting, containing both specific users and groups of users. |

See the [Task Inbox](/refguide/workflow-engine/#task-inbox) section for more details on how access rules are used to include tasks in the current user's inbox.

If an XPath or microflow results in an empty list (0 objects), the workflow fails. For more information on how to handle this kind of issues, see the [Operation](/refguide/change-workflow-state/#operation) section in *Change Workflow State*.

#### XPath Constraint {#xpath-constraint}

Specifies the expression used to target the user task. This option is displayed only when the **Source** is set to **Database**. Click **Edit** to edit the [XPath constraint](/refguide/xpath-constraints/).

Use workflow group tokens instead of the *Name* attribute. For instance, use `[ id = '[%WorkflowGroup_Managers%]' ]` instead of `[ Name = 'Managers' ]`.
The latter would *not* be updated automatically if a group is renamed.

#### Microflow

Specifies the microflow used to target the user task. This option is displayed only when the **Source** is set to **Microflow**.

#### Auto-Assign When Targeting Results In One User {#auto-assign}

Enables automatically assigning a user task when a single user is targeted. This option is displayed only when the [Target users](#target-users) is set to **User(s)**.

#### FAQ

| Question | Answer |
| --- | --- |
| When should I use user targeting and when group targeting? | In general we advise to always use group targeting as this provides the required flexibility most organizations need. Use user targeting when you want to select individual users that should be target for a user task. |
| Can I set/change the group targeting at runtime? | Yes, similarly as with user targeting, you can change the groups targeting at runtime (e.g. by using the Workflow Commons module). |
| How do I manage the users in a group? | The Workflow Commons module has a Groups function where you can managet group membership. You can build your own logic to add members to certain groups based on directory information. |

### Outcomes Section {#outcomes}

The outcomes property allows you to create new outcomes for the user task. Outcomes are translated into different outgoing paths of the user task and can be referred to by other elements, such as a button. For example, you have a process when you need to approve or reject a request. One button on a [task page](#task-page) can refer to the **Approve** outcome of the user task, while another one can use the **Reject** outcome.

### Task Page Section {#task-page}

Task page is the page that an assigned user will use to inspect their task and complete it. You can also allow users to add comments or attachments on this page.

If you generate the page using the templates in the **Workflows Commons** module, these templates contain necessary data containers and associated context entity.

### Display Information Section {#display-info}

#### Task Name

**Task name** is stored in the System module on the **UserTask** entity as an attribute and its data can be dynamically displayed in the running app. If you are using the **Workflow Commons** module, the **Task name** is used in page templates and on preconfigured pages to identify the task.

For more information on using parameters, see the [Parameters](#parameters) section below.

#### Task Description

**Task Description** is stored in the System module on the **UserTask** entity as an attribute and its data can be dynamically displayed in the running app. If you are using the **Workflow Commons** module, the **Task description** is used in page templates.

The **Task description** can contain parameters that are written between braces, for example, {1}.

#### Parameters {#parameters}

Parameters are attributes the value of which will be displayed. For example, you can display when the task is due using the **DueDate** parameter.

To view **Parameters**, click the ellipsis icon next to the **Task name** or **Task description** in properties depending on where you would like to display these parameters.

Parameters have the following settings:

* **Index** – an identification number of a parameter
* **Expression** – an attribute from the context that will be displayed

##### Adding New Parameters

To add a parameter to the **Task name** or the **Task description**, do the following:

1. Click the ellipsis icon next to the **Task name** or the **Task description**.

2. In the **Edit task name/description** dialog box > **Parameters** section, click the **New** button.

3. In the **Template Parameter (String)** dialog box, specify the expression, and confirm your choice:

   {{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/user-task/parameter-example.jpg" alt="Parameter Example" class="no-border" >}}

   {{% alert color="info" %}}Make sure that the attribute you use in the expression is of the string type.{{% /alert %}}

4. In the **Template** setting, write the text you would like to display and type **Index** of the parameter you would like to include. For example, you can add a template for the **Task description** specifying the name of the workflow and what the workflow due date is :

   {{< figure src="/attachments/refguide/modeling/application-logic/workflows/workflow-elements/user-task/task-description-example.jpg" alt="Task Description Example" class="no-border" >}}

##### Performing Other Actions on Parameters

In addition to adding new parameters, you can perform the following actions on parameters:

* **Delete** – to delete a parameter click **Delete** or press <kbd>Delete</kbd> on your keyboard
* **Edit** – double-click a parameter to edit it or click **Edit**
* **Move up** – to move a parameter up in the list of parameters and also to change its index, click **Move up**
* **Move down** – to move a parameter down in the list of parameters and also to change its index, click **Move down**

### Events Section {#events}

**On Created** event allows you to select a microflow that is executed immediately after users have been determined for a newly created task instance. You can use this setting for a microflow that will send an email notification about the user task to the assigned users.

### Boundary Events Section {#boundary-events}

For more information, see [Boundary Events](/refguide/workflow-boundary-events/).

### Common Section {#common}

**Name** is the internal name of the user task. When referring to the user task in the app you will use this name. It must be unique within the workflow, but you can have two user tasks with the same name in different workflows.

## Read More

* [Workflows](/refguide/workflows/)
* [Multi-User Task](/refguide/multi-user-task/)
