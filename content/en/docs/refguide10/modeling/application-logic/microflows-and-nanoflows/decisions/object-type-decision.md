---
title: "Object Type Decision"
url: /refguide10/object-type-decision/
weight: 2
aliases:
    - /refguide10/inheritance-split.html
    - /refguide10/inheritance-split
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

An object type decision is an element that makes a choice based on the type of an object of a generalized entity. The output of the object type decision are the specialized entities that inherit from the generalized entity. For more information on specialization and generalization, see [Entities](/refguide10/entities/).

If you want to use the specialized type in the rest of the microflow or nanoflow, you can use a [Cast object](/refguide10/cast-object/) activity.

## Properties

An example of object type decision properties is presented in the image below:

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/decisions/object-type-decision/object-type-decision-properties.png"   width="250"  class="no-border" >}}

The object type decision properties consists of the following sections:

* [Common](#common)
* [Input](#input)

### Common Section {#common} 

#### Caption

For more information, see the [Caption](/refguide10/microflow-element-common-properties/#caption) section in *Common Properties*.

### Input Section {#input}

#### Object

The input object contains an object of a generalized entity.

For example, you have an entity **Student** and an entity **Professor** which have an entity **Member** as their generalization. You want to open a different page for **Professor** than for any other **Member**. The selected **Member** object is available in the parameter **SelectedMember** and is used as input to the object type decision. Note that there is no outgoing flow for **Student**. If an outgoing flow is missing, the closest generalization that has an outgoing flow is searched. In this case, this generalization is **Member**. The outgoing flow with the caption **(empty)** is followed when **SelectedMember** does not contain an object.

{{% alert color="warning" %}}
In a nanoflow, only generalizations that are accessible by the user can be searched.
{{% /alert %}}

{{< figure src="/attachments/refguide10/modeling/application-logic/microflows-and-nanoflows/decisions/object-type-decision.png" class="no-border" >}}
