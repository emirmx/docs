---
title: "Commit Object(s)"
url: /refguide10/committing-objects/
weight: 30
---

{{% alert color="info" %}}
This activity can be used in both microflows and nanoflows.
{{% /alert %}}

## Introduction

The **Commit object(s)** activity works on one or more objects. For persistable entities, committing an object stores it in the database. Committing non-persistable entities stores the current attribute values and association values in memory. This allows a rollback to revert to those values. See also [Persistability](/refguide10/persistability/). External objects cannot be committed. To store changed values of external objects, use the [Send External Object](/refguide10/send-external-object/) activity.

{{% alert color="info" %}}
A Mendix commit does not always behave like a database commit. See [How Commits Work](#how-commits-work), below, for more information.
{{% /alert %}}

## Properties

An example of **Commit object(s)** properties is represented in the image below:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/activities/object-activities/committing-objects/commit-properties.png" alt="commit object(s) properties" width="650px" class="no-border" >}}

There are two sets of properties for this activity, those in the dialog box on the left, and those in the properties pane on the right.

The **Commit object(s)** properties pane consists of the following sections:

* [Action](#action)
* [Common](#common)

## Action Section{#action}

The **Action** section of the properties pane shows the action associated with this activity.

You can open a dialog box to configure this action by clicking the ellipsis (**…**) next to the action.

You can also open the dialog box by double-clicking the activity, or right-clicking the activity and selecting **Properties**.

### Object or List

The object or list of objects that you want to commit.

### With Events{#with-events}

{{% alert color="info" %}}
This property is for microflows only.
{{% /alert %}}

Indicates whether or not to execute the commit event handlers of the objects.

Default: **Yes**

{{% alert color="warning" %}}
Most validation rules are not triggered if you do a commit and **With events** is set to *No*. In most cases, validation rules will have been triggered when members (for example, an attribute) are changed, but Mendix advises that you set **With events** to *Yes* if you want to ensure that validations are always carried out. See [data validation](/refguide10/setting-up-data-validation/) for more information.
{{% /alert %}}

#### Events in Nanoflows

Nanoflows do not have this property.

If the **Commit object(s)** activity is used in an online app, it sends a commit request to the Mendix Runtime and always runs the events.

If the **Commit object(s)** activity is used in an offline app, the changes are committed to the offline database, and event handlers are run when the offline app synchronizes.

### Refresh in Client{#refresh-in-client}

This setting defines how changes are reflected in the pages presented to the end-user.

Default: **No**

{{% alert color="info" %}}
To make pages of a Mendix app efficient, many widgets display values from an attribute of an object which is cached on the page. Attributes in widgets which use cached data are always reflected in the client when they are updated or deleted irrespective of the value of **Refresh in client**.

If a widget is only updated when a [data source](/refguide10/data-sources/) is loaded, then changes are only seen when **Refresh in client** is set to **Yes**.

When testing your app, ensure that the desired data is being displayed by the widgets you choose.
{{% /alert %}}

{{% alert color="warning" %}}
When committing a large number of objects, Mendix recommends not enabling **Refresh in client**, because it can slow things down.
{{% /alert %}}

#### Microflow is Called from the Client in an Online App

If **Refresh in client** is set to **No**, the change is not reflected in the client.

If set to **Yes**, the object is refreshed across the client, which includes reloading the relevant [data sources](/refguide10/data-sources/).

#### Microflow is Called in an Offline or Native App

When inside a microflow that is called from an offline or native app, the **Refresh in client** option is ignored and functions as if it was set to **No**.

For more information, see the [Microflows](/refguide10/mobile/building-efficient-mobile-apps/offlinefirst-data/best-practices/#microflows) section of Offline-First Data.

#### Action is in a Nanoflow

When inside a [nanoflow](/refguide10/nanoflows/), the object is refreshed across the client as if **Refresh in client** was set to **Yes**.

## Common Section{#common}

{{% snippet file="/static/_includes/refguide10/microflow-common-section-link.md" %}}

## How Commits Work{#how-commits-work}

### Committing Objects

When you commit an object, the current value is saved. This means that you cannot roll back to the previous values of the object using the rollback object activity of a microflow.

However, a Mendix commit is not the same as a database commit. For an object of a persistable entity, the saved value is not committed to the database until the microflow and any microflows from which it is called, complete. This means that errors in a microflow can initiate a rollback. If a microflow activity errors and has **Error handling** set to **Rollback** or **Custom with rollback**, the value of the object is rolled back to the value it had at the start of the microflow. See [Error Handling in Microflows](/refguide10/error-handling-in-microflows/) for more information.

Mendix mimics this behavior for non-persistable entities. Committing a non-persistable entity means that you cannot use a rollback object activity to go back to the previous values, although the rollback error handling in a microflow rolls back to the original values.

### Autocommit and Associated Objects {#autocommit-and-associated-objects}

When an object is committed through a default **Save** button, a commit activity, or web services, it always triggers the commit events. The platform also evaluates all associated objects. To guarantee data consistency, the platform may also autocommit associated objects.

An autocommit is an automatic commit from the platform, which is done to keep the domain model in sync. If your application ends up having autocommitted objects, then you will have a modeling error. Since an association is also a member of an object, the association is stored in the database as well. This means that if you create an order line inside an order and the order line is the parent of the association, when you commit the order line, the order is autocommitted.

{{% alert color="warning" %}}
An autocommit is not the same as an explicit commit!

When objects are autocommitted, all changed member values, including associations, are saved in the database as well. These members are still marked as changed until these objects are explicitly committed. As a consequence, association rows for these objects are deleted and reinserted in the database when these objects are explicitly committed.

If a rollback is triggered for any reason (for example, if the user session is terminated by the user closing the browser), then autocommitted objects are deleted from the database. For more information about how Mendix handles persistable objects, see [Persistability](/refguide10/persistability/).
{{% /alert %}}

If you end up with autocommitted objects, it is always because of a modeling error. At some point in time, an association was set to a new object, the associated object was committed, and all of its associations were committed as well to keep all the data consistent.

During commits, the following occurs:

* Events:
    * For explicitly committed objects, all before and after events are executed, and if any before-rollback event returns false, an exception can be thrown
    * If an exception occurs during an event, all the applied changes are reverted with the default error handling behavior
    * Changes made prior to the commit are kept
        {{% alert color="warning" %}}Before and after events are not executed for autocommitted objects.{{% /alert %}}
* Database:
    * There is an insert or update query executed both for explicitly committed objects and autocommitted objects
    * Depending on the object state, the Mendix Runtime does an insert for objects with the state **Instantiated** and an update for all other states
* Result:
    * An object with the state **Instantiated** is inserted into the database, and an object with any other state is updated

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/activities/object-activities/committing-objects/during-commits.png" class="no-border" >}}
