---
title: "Version a REST Service"
url: /refguide10/version-rest-service/
weight: 10
description: "Describes the best practices for versioning REST services in Mendix."
aliases:
    - /howto10/integration/version-rest-service/
---

## Introduction

You can have different versions of the same REST service published at the same time.

This document teaches you how to do the following:

* Utilize best practices for when and how to use different versions of the same REST service published at the same time
* Deprecate old versions

## Numbering the Versions

Give each service a version number following the `MAJOR.MINOR.PATCH` format:

* A service must get a new `MAJOR` version when you make incompatible API changes (such as removing an operation).
* A service should get a new `MINOR` version when you add functionality that is backwards-compatible (such as adding an operation).
* A service may get a new `PATCH` version when you make a bug fix that is backwards-compatible.

To make versioning more explicit in your model, it is suggested to create a folder for each service named *ServiceName_Version*. Store all microflows, mappings, and message definitions that are used in the service in this folder.

After you have published a service and users are using it, it is not advisable to change it anymore. 

If you change a service in a way that requires the user to change, you would introduce a breaking change, which would cause the user to receive errors without doing anything different. 

Instead of changing a published service, you should duplicate the service and give it a new major version. If you want to change a microflow, a mapping, or a message definition, duplicate it as well, and change the duplicate.

Change the location of the new version to include the new version (for example, **rest/myservice/v1.1**). It is customary to omit the **.0** or **.0.0** in the URL.

The new version of the service reuses all microflows, mappings, and message definitions from the previous version that did not change in the new version.

## Examples

Below are examples of typical changes that occur and how to handle the versioning of your service in those cases.

### Fixing a Bug in a Microflow

#### Scenario

You have a REST service in Petstore version 1.0.0 in production. You find out you need a small change to the GetPet microflow. When the incoming `pet_id` is empty, it is giving an error that results in a **500 Internal Server Error**, but it should be **400 Bad Request**.

#### Solution

Since this is a non-breaking change, there are two solutions to this problem: create a separate patch version, or fix the bug in the current version.

To create a patch version, do the following:

1. Create a new folder called *PetStore_1_0_1*.
2. Duplicate the PetStore service, name it *PetStore_1_0_1*, and move it to the **PetStore_1_0_1** folder.
3. Duplicate the GetPet microflow, name it *GetPet_1_0_1*, and move it to the **PetStore_1_0_1** folder.
4. Update the **PetStore_1_0_1** service, making the GET operation refer to **GetPet_1_0_1**.
5. Change the **GetPet_1_0_1** microflow to fix the behavior.

### Adding an Operation to a Resource

#### Scenario

You have a REST service in Petstore version 1.0.0 in production. You want to add an operation to retrieve pets by status.

#### Solution

Since this change is backwards-compatible, there are two solutions to this problem: create a new minor version, or add the operation to the current version.

To create a new minor version, do the following:

1. Create a new folder called **PetStore_1_1_0**.
2. Duplicate the PetStore service. Call it **PetStore_1_1_0** and move it to the **PetStore_1_1_0** folder.
3. Add the **GetPetByStatus** operation to the **PetStore_1_1_0** service.

### Changing the Type of an Attribute

#### Scenario

You have a REST service in Petstore version 1.0.0 in production. The **GET /pet** operation returns a pet's year of birth of as an integer. You have a message definition called **Pet** that is based on the **Pet** entity. The **ExportPet** export mapping maps the entity to the message definition.

You want to change the year of birth to date of birth.

#### Solution

Note that this is a breaking change (because it is not backwards-compatible), so you must create a new version of your service.

1. Add a new attribute **DateOfBirth** to the **Pet** entity.
2. Create a new folder called *PetStore_2_0_0*.
3. Duplicate the new message definitions. Call it **PetStoreMessages_2_0_0** and move it to the **PetStore_2_0_0** folder. Remove any message definitions you do not want to change.
4. Create a new export mapping called *ExportPet_2_0_0* in the **PetStore_2_0_0** folder. Base it on the **Pet** entity, selecting the same attributes as before, but choosing **DateOfBirth** instead of **YearOfBirth**.
5. Duplicate the **PetStore** service. Call it *PetStore_2_0_0* and move it to the **PetStore_2_0_0** folder.
6. Update the **GET /pet** operation in the **PetStore_2_0_0** service, choosing the **ExportPet_2_0_0** export mapping.

## Deprecation

After you have created a new version of your service, mark the old version as deprecated.

Do this by adding **(deprecated)** to the service name. Write a description of why it was deprecated and what the new version number is in the **Public documentation** of the service. You should also mark all the operations that changed as deprecated so the user can see which operations have changed from one version to the next.

You should let users know that the this version is deprecated (for instance, by publishing release notes).

After a version has been deprecated for a sufficiently long time, you can remove it. 
