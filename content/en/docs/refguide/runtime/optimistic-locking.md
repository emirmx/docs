---
title: "Optimistic Locking"
url: /refguide/optimistic-locking/
description: "Describes optimistic locking support."
---

## Introduction

{{% alert color="info" %}}
This feature is available from Mendix 11.5 and onwards.
{{% /alert %}}

Optimistic locking is a strategy used in concurrent systems to prevent **lost updates** when multiple users or processes try to modify the same piece of data at the same time. The **optimistic** part comes from the assumption that conflicts are rare. Instead of locking the data immediately, it allows multiple users to read and potentially modify the same data concurrently. It checks for conflicts only at the very last moment, when an update is attempted.

If a conflict is detected—meaning someone else has modified the data since you last read it—your update is rejected, and you typically have to re-read the data and try again.

## Project With Optimistic Locking Disabled

When two modifications are saved, they are applied in the order of processing. Only changed attributes are written to the database. This means that if two objects with disjoint sets of changed attributes are committed, the changes are not overwritten.

For example, if one user commits changes for `AttributeA` and `AttributeB` and another user commits changes for `AttributeB` and `AttributeC` for the same object, then both `AttributeA` and `AttributeC` are committed according to both users' changes. `AttributeB` is committed based on whichever change was committed last.

## Project With Optimistic Locking Enabled

The Mendix runtime implements optimistic locking by introducing a new attribute named `MxObjectVersion` with type `Long` to all entities tracking their version. The `MxObjectVersion` attribute is not write-protected. However, setting this value will **not** result in it being saved to the database. Its current value will be compared with the value for the same record in the database.

### How to Enable and Use Optimistic Locking

Optimistic locking can be enabled per Mendix application using the `Runtime` tab in the App settings dialog:

{{< figure src="/attachments/refguide/runtime/optimistic-locking/runtime-settings-dialog.png" >}}

After optimistic locking is enabled, whenever a commit is executed, the Mendix runtime will automatically ensure that the object that is committed was not already changed by another party after the committer retrieved the object.

### New Projects and Migration

In case an existing app already had the `MxObjectVersion` attribute, when optimistic locking is enabled a duplicate attribute will be reported in the Modeler. This must be fixed by renaming the existing attribute to another name. The system attribute cannot be renamed.

### Behavior

The following runtime actions are influenced by optimistic locking:

#### Create Object

Initialize the `MxObjectVersion` attribute to `0`.

#### Commit Object

When the `MxObjectVersion` attribute in the object being committed is different from the value in the database, or the object was deleted from the database, throw a `ConcurrentModificationRuntimeException`. Otherwise, proceed with the commit while incrementing `MxObjectVersion` by one.

#### Delete Object

When the `MxObjectVersion` attribute in the object being deleted is different from the value in the database, throw a `ConcurrentModificationRuntimeException`. Otherwise, proceed with the delete. If the object was already deleted, no error occurs.

## Performance Impact

Because of the version check performed during commit and delete, optimistic locking incurs some minor performance impact.

## Handling Optimistic Locking Errors in Microflows

When an optimistic locking error occurs, the runtime log contains an entry similar to the following:

```
    com.mendix.modules.microflowengine.MicroflowException: com.mendix.core.CoreRuntimeException: com.mendix.systemwideinterfaces.MendixRuntimeException: com.mendix.core.CoreException: com.mendix.core.CoreRuntimeException: com.mendix.systemwideinterfaces.MendixRuntimeException: com.mendix.systemwideinterfaces.connectionbus.data.ConcurrentModificationRuntimeException: Object of type 'MyFirstModule.MyEntity' with guid '3940649673949185' cannot be updated, as it is modified by someone else
    	at MyFirstModule.MyMicroflow (Change : 'Change 'MyEntity'')
```

The above error shows that there was a `ConcurrentModificationRuntimeException` during execution of the change action `Change 'MyEntity'` of the microflow `MyFirstModule.MyMicroflow`. The object had the id `3940649673949185` and was of type `MyFirstModule.MyEntity`.

Once a commit of an object causes an optimistic locking error, trying to commit the same object without reloading will always result in an optimistic locking error.

If the changes are still deemed valid, the action causing the optimistic locking error should be retried:

1. Change the error handling for that action to either `Custom with Rollback` or `Custom without Rollback`, and do the following steps in the error flow
2. If `$latestError/ErrorType` is `com.mendix.systemwideinterfaces.connectionbus.data.ConcurrentModificationRuntimeException`
3. Retrieve a fresh copy of the object from the database
4. Apply the original changes onto the retrieved copy
5. Invoke the operation again

An example can be seen below. The change action has error handling `Custom without Rollback`. If an optimistic locking error is detected, the object is reloaded from the database, changes are applied again and the change action is retried.

{{< figure src="/attachments/refguide/runtime/optimistic-locking/retry-example.png" >}}

Check the [documentation](https://docs.mendix.com/refguide/error-handling-in-microflows/) about error handling to find more information about differences between `Custom with Rollback` and `Custom without Rollback`.

## Handling Optimistic Locking Errors in Java Actions

In Java actions an optimistic locking error can be handled by similar steps described in [Handling Optimistic Locking Errors in Microflows]. However, the `com.mendix.systemwideinterfaces.connectionbus.data.ConcurrentModificationRuntimeException` exception will not be the top level exception thrown by the Mendix runtime. It will be wrapped in another exception, so the `cause` chain of caught exceptions should be inspected.

## Handling Optimistic Locking Errors in the Client

If the optimistic locking error is propagated to the client, then an error dialog is shown as follows:

{{< figure src="/attachments/refguide/runtime/optimistic-locking/optimistic-locking-error-dialog.png" >}}

To handle the optimistic locking error one of the following approaches can be used:

### Single `Save` Button

In this case the user can refresh the whole page, but this causes all their changes to be lost.

### `Save` Button With a Separate `Refresh` Button

A separate `Refresh` button that retrieves the latest object state from the database and applies the original changes to the retrieved object. After clicking the `Refresh` button, the user can inspect the latest state and can click the `Save` button again. This would be similar to a single retry from [Handling Optimistic Locking Errors in Microflows].

### Custom `Save` Button

A custom `Save` button that retries saving object using similar steps described in [Handling Optimistic Locking Errors in Microflows], to retry until successful.
