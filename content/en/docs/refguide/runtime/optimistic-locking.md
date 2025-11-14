---
title: "Optimistic Locking"
url: /refguide/optimistic-locking/
description: "Describes optimistic locking support."
---

# What is Optimistic Locking

Optimistic locking is a strategy used in concurrent systems to prevent **lost updates** when multiple users or processes try to modify the same piece of data at the same time. The **optimistic** part comes from the assumption that conflicts are rare. Instead of locking the data immediately, it allows multiple users to read and potentially modify the same data concurrently. It only checks for conflicts at the very last moment, when an update is attempted.

If a conflict is detected, meaning someone else modified the data since you last read it, your update is rejected, and you typically have to re-read the data and try again.

This feature is available from Mendix 10.5.0 and onwards.

# Behavior before Mendix 10.5.0 or when Optimistic Locking is disabled

Before Mendix 10.5.0 the runtime did no locking. When two modifications are saved then they are applied in order of processing. This is still the behavior when optimistic locking is disabled.

# How to enable and use optimistic locking

Optimistic locking can be enabled per Mendix application using `Runtime` tab in the App settings dialog:

{{< figure src="/attachments/refguide/runtime/optimistic-locking/runtime-settings-dialog.png" >}}

After optimistic locking is enabled whenever a commit is executed, Mendix runtime will automatically ensure that the object that is committed was not already changed by another party after the committer read the object.

## New projects and migration

In case an existing app already had the `MxObjectVersion` attribute, when optimistic locking enabled a duplicate attribute will be reported in the Modeler. This must be fixed by renaming the existing attribute to another name. The system attribute cannot be renamed.

# Under the hood

Mendix runtime implements optimistic locking by introducing a new attribute named `MxObjectVersion` to all entities. When an object is committed, the `MxObjectVersion` of the committed object is compared to the existing data in the database. If the `MxObjectVersion` value does not match the one in the database, this means the object is modified by another party since it is last read. In this case runtime throws a `ConcurrentModificationRuntimeException`.

When Optimistic Locking is enabled, each entity gets an additional system attribute with the name `MxObjectVersion` of type `Long`. This field is automatically populated with the correct value. The default value is `1` and this value will be automatically increased with every commit of that entity instance.

Upon `update` and `delete`, the attribute value is from the object compared to the value available for this record in the database. If it is the same, then the update or delete will proceed. If it is different, which means the object is modified by another party since it is last read, a `ConcurrentModificationRuntimeException` is thrown, preventing the update or delete from proceeding. The `MxObjectVersion` attribute on the Mendix object is not write-protected. Setting this value however will **not** result in this value being saved into the database. Its current value will be used to compare it with the value for the same record in the database.

## Impact on insert

There is no impact on insert as this will just introduce the record in the database.

## Impact on update

When no concurrent modification occurs, update happens as before. However, when a concurrent modification is detected, the runtime will throw a `ConcurrentModificationRuntimeException`. This exception prevents the transaction from succeeding. If the entity instance was deleted before applying the update this would also result in `ConcurrentModificationRuntimeException`.

## Impact on delete

If the entity was updated before applying delete an optimistic lock error occurs. If the entity was deleted before applying delete no error occurs (as is the case without optimistic locking too). 

## Impact on performance

This attribute is only added to the entities that are not derived from other entities. This way all entities will have this attribute (the derived entities will derive it from the parent entity). This causes every entity to have maximum one extra attribute in queries and an extra check upon update. There can be some performance impact, although it is expected to be minor.

# Handling optimistic locking errors in Microflows

If the changes are still deemed valid, the action causing the optimistic locking error can be retried. To do that, error handling for that action should be changed to use either `Custom with Rollback` or `Custom without Rollback`. Then if `$latestError/ErrorType` is `com.mendix.systemwideinterfaces.connectionbus.data.ConcurrentModificationRuntimeException`
then this means an optimistic locking error is occured. To retry the operation a fresh copy of the object should be loaded from database, then existing changes should be applied onto that object and the operation should be invoked again. Once committing an object caused optimistic locking error, trying to commit the same object without reloading will always result in optimistic locking error.

An example can be seen below. Change/Commit action has error handling `Custom without Rollback`. If an optimistic locking error is detected, the object is reloaded from database, changes applied again and commit is retried.

{{< figure src="/attachments/refguide/runtime/optimistic-locking/retry-example.png" >}}

Check [documentation](https://docs.mendix.com/refguide/error-handling-in-microflows/) about error handling to get more info about differences between `Custom with Rollback` and `Custom without Rollback`.

# Handling optimistic locking errors in Java actions

In Java actions if an optimistic locking error is detected similar steps described in `Handling Handling optimistic locking errors in Microflows` can be used for retrying. The `com.mendix.systemwideinterfaces.connectionbus.data.ConcurrentModificationRuntimeException` exception will not be the top level exception thrown by Mendix runtime. It will be wrapped in another exception, so the `cause` chain of caught exception should be inspected.

# Handling optimistic locking errors in the client

If the optimistic locking error is propagated to the client, then an error dialog is shown as follows:

{{< figure src="/attachments/refguide/runtime/optimistic-locking/optimistic-locking-error-dialog.png" >}}

The runtime log file contains an entry similar to following:

```
    com.mendix.modules.microflowengine.MicroflowException: com.mendix.core.CoreRuntimeException: com.mendix.systemwideinterfaces.MendixRuntimeException: com.mendix.core.CoreException: com.mendix.core.CoreRuntimeException: com.mendix.systemwideinterfaces.MendixRuntimeException: com.mendix.systemwideinterfaces.connectionbus.data.ConcurrentModificationRuntimeException: Object of type 'MyFirstModule.MyEntity' with guid '3940649673949185' cannot be updated, as it is modified by someone else
    	at MyFirstModule.MyMicroflow (Change : 'Change 'MyEntity'')
```

Above error shows that there was a `ConcurrentModificationRuntimeException` during execution of the change action `Change 'MyEntity'` of the microflow `MyFirstModule.MyMicroflow. The object had the id ~3940649673949185` and was of type `MyFirstModule.MyEntity`.

The only option is refreshing the page which would cause losing all the changes.
