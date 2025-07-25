---
title: "Data Storage"
url: /refguide10/data-storage/
description: "Introduces data storage, which is the data foundation of the Mendix Runtime."
---

## Introduction

Data storage is the data foundation of the Mendix Runtime. Data storage does the following:

* Connects to supported relational databases
* Stores and retrieves entities and associations from the domain model
* Translates XPath and OQL queries to SQL queries
* Handles security transparently and effectively

{{% alert color="warning" %}}
Each app has its own database which cannot be shared directly with other apps. If you want to share data with another app, you must publish an API using the [Catalog](/data-hub/share-data/) or the [REST and OData](/refguide10/integration/) capabilities of Mendix.

See [Databases and Apps](#databases), below, for an overview of this.
{{% /alert %}}

## Supported Databases

For apps deployed to Mendix Cloud, Mendix uses a PostgreSQL database for storing the data defined in the app domain model (or models).

If you are deploying to a different infrastructure, you will need to provide your own database.
See [System Requirements](/refguide10/system-requirements/#databases) for the list of supported databases.

## Databases and Apps{#databases}

Each app has its own database which is described in the domain models of the modules in the app.

This section explains why you cannot share a database between apps, and gives alternative methods to share data.

### Database Table Names

Your app refers to entities using their names and modules. However, the database table holding the data for each entity is linked to the entity in the domain model by a unique identifier. For example, an `Order` entity in your app may be given an ID `807106d1-c0a8-4ef5-a387-2073cdee6d55`. If you delete an entity and create a new entity with the same name, it will be give a different ID as the model will see it as a new entity.

The ID will remain attached to this entity within your app so that you can create multiple branches and deploy new versions of your app while using the same database. This allows you to change the entity in the domain model, renaming it for example, and the app will know that it is still the same entity.

However, if you export your module and import it to another app, the entities in the new app will be given new IDs. Since the ID is used to confirm the relationship between tables in the database and your domain model, you will get a completely different database table when you deploy your app, even though the domain model is the same. This also applies to restoring data to one app when it was backed up from a different app.

### Consequence of Sharing a Database

Imagine you have an order handling app, `Order Processing`, which uses an `MyModule.Order` entity with the ID `807106d1-c0a8-4ef5-a387-2073cdee6d55`.

You now create a second app, `Order Viewer`, which you want to use to view your orders. You export the domain model from `Order Processing` and import it into `Order Viewer`. However, the `MyModule.Order` entity in this new app will be given a different ID, for example `836a64f6-6e70-4f69-ba30-ed3876fd33b9`.

You now try to deploy `Order Viewer` to use the same database as `Order Processing`. Because, the `Order` entity has a different ID, the Mendix Runtime will see the two tables as being different. The existing table `mymodule$order` will be deleted and a new `mymodule$order` table will be created linked to the new ID.

### How to Share Data

If you want to share data between apps, you should set up a *microservices* architecture. In short, identify one app which you want to use to store the data. This app will now do all the creating, reading, updating, and deleting of the data. It will publish an API to allow other apps to access the data using, for example, [OData](/refguide10/published-odata-services/) or the [Catalog](/data-hub/share-data/). Other apps can then consume this API to use the data. This ensures that there is only one source for the data and that it is kept consistent.

An alternative is to copy the data to another app, for example using the [Database Replication](/appstore/modules/database-replication/) module. This however, will be a snapshot of your data at the time you replicate it and changes to the data made in the original app will not be reflected in your new app.

## Database Transactions and Locking

### Database Record Locks

Mendix does not use read locks on the database. Therefore, object reads can always be performed. A database write lock is put in place for the first object commit. If there are overlapping update transactions on the same object, the first update transaction puts a write lock on the database object. The next update transaction waits until the first one finishes before executing. However, if the first update process takes an inordinate amount of time, it may cause a timeout.

When users or microflows make changes to database records, all changes will execute in a transaction. Write locks are placed on individual records the moment they are committed. Until this point no locks will be placed on the data. For more information on the differences between transaction commits and object commits, see the [How Commits Work](/refguide10/committing-objects/#how-commits-work) section in *Commit Object(s)*.

When the record gets locked, as long as the transaction is executing, no other users or processes are able to change the data. The uncommitted information is not visible for others. The changed data becomes available for other users to read only after the transaction completes. While the transaction is running, other users are able to read and change the previously persisted version of the data. Any changes from other processes will be applied when the transaction completes and the initial write lock is lifted, overwriting the previous changes.

### Transaction Isolation

To ensure data validity and improve database performance in a multiuser environment, Mendix isolates concurrent database transactions. Isolation is done by using the `Read Committed` isolation level. This is the default isolation setting for PostgreSQL databases. Mendix relies on the implementation of `Read Committed` in the database. If you use a different database, the results of `Read Committed` might vary due to a different implementation of the isolation level.

For more information on how `Read Committed` works in PostgreSQL, see [PostgreSQL Read Committed documentation](https://www.postgresql.org/docs/12/transaction-iso.html#XACT-READ-COMMITTED).

## Foreign Key Constraints{#fkc}

A Foreign Key Constraint (FKC) is a database-level concept. An FKC enforces consistency of links between tables and makes sure that a reference to a table row exists only if a referred row exists as well.

### Foreign Key Constraints in Mendix

{{% alert color="info" %}}
Foreign Key Constraints are applied to new apps created in Mendix version 10.6.0 and above.
{{% /alert %}}

When it comes to the Mendix data model, having a FKC means that all associations and System members such as `owner` and `changedBy` are guaranteed to be consistent; if there is an association, then the associated object exists as well.

From the user perspective, that is already the case. In a Mendix app, is impossible to encounter a reference that does not refer to an object.

Nevertheless, because of the way Mendix uses association tables in the database to record associations from the Domain Model, there are certain scenarios that may lead to records in these association tables referring to non-existent records in entity tables, leaving the association tables having dangling references. Such association records are not accessible from the app itself, so such orphaned records do not affect app functionality in any way. But their existence in the database usually indicates that the app itself may contain an error that leads to inconsistent data.

The FKC feature is designed to safeguard against scenarios that would lead to dangling references. Using FKCs is also a good practice of database development.

With this feature, if the app is going to create a dangling reference, a runtime exception is thrown. This allows the developer to identify and investigate the root cause of the dangling reference. Without that feature, the erroneous scenario would go unnoticed.

### Adding Foreign Key Constraints to New Projects

Foreign Key Constraints are enabled for new projects in version 10.6.0 and above. This applies to:

* Projects created from scratch using a starter app
* Projects created using [Import app package](/refguide10/import-app-package-dialog/)

Apps created before 10.6 are not affected. This means that if your app is created in a version of Studio Pro below 10.6.0 and then upgraded to version 10.6.0 or above, FKCs do not get enabled for it.

When a new app is created from a Starter App or an app package, it may already contain a data snapshot. Before FKC is enabled during synchronization, any dangling references are deleted from the database. This cleanup is performed only once and is not repeated on consequent runs for the same database.

### Setting Foreign Key Constraints On and Off

In Mendix version 10.10.0 and above, it is possible to set Foreign Key Constraints for existing projects on or off regardless of which version the project was originally created in by toggling the option in the app's [runtime settings](/refguide10/app-settings/#database-fkc).

{{% alert color="info" %}}
Deploying an existing application after enabling or disabling FKC will cause a synchronization step to be executed.
{{% /alert %}}
