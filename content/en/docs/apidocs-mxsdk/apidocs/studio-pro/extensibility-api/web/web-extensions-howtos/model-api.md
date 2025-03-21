---
title: "Model Api"
url: /apidocs-mxsdk/apidocs/extensibility-api/web/model-api/
weight: 6
---

## Introduction 

This how-to provides guidance on using the Model Access API:

* [Using the Model Access API](#using-api)
* [Reading the units info and loading units](#units-info-load)
* [Reading the unit content](#read)
* [Modifying the unit content](#modify)

## Using the Model Access API {#using-api}

The Model Access API allow access to the Mendix model.

The model is split in several components exposed via `studioPro.app.model` object. Currently supported components are:
* BuildinBlocks
* DomainModels
* Enumerations
* Pages
* Snippets

```ts
const { pages, domainModels } = studioPro.app.model;
```

## Reading the units info and loading units {#units-info-load}

An element is part of a Mendix model and all elements together form the logic of the model. Elements may contain other elements. An element always has a container element, which is its parent. The root of an element tree is always a unit.

Each component exposes the units info of the units it is responsible for. The full unit content can be accessed only after loading the unit.

The unit info contains the the following fields:
| Name | Description | 
| --- | --- |
| `$ID` | The unique id of the unit |
| `$Type` | The type of the unit |
| `moduleName` | (Optional) The name of the module containing the unit |
| `name` | (Optional) The name of the unit |

All the units managed by the DomainModels component can be retrieved by:

```ts
const unitsInfo = await domainModels.getUnitsInfo()
```

Units can be loaded by suppling a function to `loadAll` to execute for each unit. It should return a truthy value to load the unit.

The followind snippet loads the DomainModel for the module named `MyFirstModule`:

{{% alert color="warning" %}}
Loading units is an expensive process. Only load the minimun number of units you need when you need them.
{{% /alert %}}

```ts
const [domainModel] = await domainModels.loadAll((info) => info.moduleName === 'MyFirstModule');
```

## Reading the unit content {#read}

Elements contained inside units can be accessed using the `get<ElementName>` helper methods:

The following snippet will get the Entity named `MyEntity` from the previously loaded DomainModel unit:

```ts
const entity = domainModel.getEntity("MyEntity");
```

## Modifying the unit content {#modify}

The Mendix model can be modified by leveraing the `add<ElementName>` helper methods:

The following snippet will create a new Entity inside the previously loaded DomainModel unit:

```ts
const newEntity = await domainModel.addEntity({ name: "NewEntity", attributes: [{ name: "MyAttribute", type: "AutoNumber" }]})

await domainModels.save(domainModel);
```

