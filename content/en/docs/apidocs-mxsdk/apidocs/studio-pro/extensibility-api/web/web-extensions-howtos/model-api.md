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
* BuildingBlocks
* DomainModels
* Enumerations
* Pages
* Snippets

```ts
const { pages, domainModels } = studioPro.app.model;
```

## Reading the units info and loading units {#units-info-load}

An element is part of a Mendix model and all elements together form the logic of the model. Elements may contain other elements. An element always has a container element, which is its parent. The root of an element tree is always a unit.

Each component (e.g. `studioPro.app.model.pages` and `studioPro.app.model.domainModels`) exposes the units info of the units it is responsible for. The full unit content can be accessed only after loading the unit.

The unit info, described by the `UnitInfo` interface, contains the the following fields:
| Name | Description | Example value |
| --- | --- | --- |
| `$ID` | The unique id of the unit | `077d1338-a548-49a9-baee-c291e93d19af` |
| `$Type` | The type of the unit | `Pages$Page` |
| `moduleName` | (Optional) The name of the module containing the unit | `MyFirstModule` | 
| `name` | (Optional) The name of the unit | `ExamplePage` |

All the units managed by the DomainModels component can be retrieved by:

```ts
const unitsInfo: Primitives.UnitInfo[] = await domainModels.getUnitsInfo()
```

Units can be loaded by supplying a function to `component.loadAll(fn)` to execute for each unit. The function `fn` should return a truthy value to load the specified unit.

The followind snippet loads the DomainModel for the module named `MyFirstModule`:

{{% alert color="warning" %}}
Loading units is a resource intensive process. Only load the minimum number of units you need when you need them.
{{% /alert %}}

```ts
const [domainModel] = await domainModels.loadAll((info: Primitives.UnitInfo) => info.moduleName === 'MyFirstModule');
```
The followind snippet loads the Page named `Home_Web` inside the module named `MyFirstModule`:

```ts
const [page] = await pages.loadAll((info: Primitives.UnitInfo) => info.moduleName === 'MyFirstModule' && info.name === 'Home_Web')
```
## Reading the unit content {#read}

Elements contained inside units can be accessed using the `get<ElementName>` helper methods.

The following snippet will get the Entity named `MyEntity` from the previously loaded DomainModel unit:

```ts
const entity: DomainModels.Entity = domainModel.getEntity("MyEntity");
```

## Modifying the unit content {#modify}

The Mendix model can be modified by leveraging the `add<ElementName>` helper methods.

The following snippet will create a new Entity inside the previously loaded DomainModel unit:

{{% alert color="warning" %}}
Do not forget to invoke the `component.save(unit)` method after making changes to your unit. The method must be invoked for each modified unit, so changes on multiple units need to be saved separetely.
{{% /alert %}}

```ts
const newEntity: DomainModels.Entity = await domainModel.addEntity({ name: "NewEntity", attributes: [{ name: "MyAttribute", type: "AutoNumber" }]});

newEntity.documentation = "New documentation";

await domainModels.save(domainModel);
```

