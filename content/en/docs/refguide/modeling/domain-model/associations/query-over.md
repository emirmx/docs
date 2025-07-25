---
title: "Querying Over Self-References"
url: /refguide/query-over/
weight: 20
---

## Introduction

Sometimes, you want to create a more generic domain model to allow more flexibility in the type and structure of your data. In this case, you often turn to using inheritance or self references to allow for simple yet efficiently designed models. This makes building your microflows and application logic much easier, but it can become challenging to query the correct objects: especially when you are using a self-reference.

## The Example

This example is for an implementation of folders on a computer, where one folder can contain several subfolders.

{{% alert color="info" %}}
This example describes an implementation using association tables. You can also specify direct associations. Although the technical implementation details are different, the XPath constraints in the use cases do not change.
{{% /alert %}}

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-example-structure.png" class="no-border" >}}

To implement this, a self-reference to **Folder** is used. The self-reference is an association called **SubFolder_Folder**. This allows you to build a folder structure with unlimited numbers and levels of folders.

{{% alert color="info" %}}
The association in this case is a one-to-many association, but the same techniques apply to many-to-many or one-to-one associations.
{{% /alert %}}

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/self-reference-domain-model.png" class="no-border" >}}

If we create our folder functionality in a module called **QueryOver**, the association **SubFolder_Folder** is described in two ways in the domain model:

| Name | Type | Owner | Parent | Child |
| --- | --- | --- | --- | --- |
| SubFolder_Folder | Reference | Default | QueryOver.Folder | QueryOver.Folder |

* Multiplicity: One 'Folder' object is associated with multiple 'Folder' objects

The **Child** is the **Owner** of the association - in other words, the association is always updated through the child.

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-association.png" class="no-border" >}}

There are six folders in the example above, and the database is structured and the attributes filled as shown below. In the **SubFolder_Folder** table, the **ChildFolderID** is shown on the left as it is the owner of the association.

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-example-database.png" class="no-border" >}}

For more information on how domain models are implemented in databases, see the [Implementation](/refguide/domain-model/#implementation) section of *Domain Model*.

### Retrieving the SubFolder (or SubFolders) (Children) from a Folder (Parent)

If you have the $ChosenFolder object available in your microflow you can easily retrieve the subfolder (or subfolders). Each association has a right side (parent in the association) and a left side (child or owner of the association).  The platform reads each association and determines whether the parent is equal to the $ChosenFolder.

This is implemented using the following XPath constraint: `[QueryOver.SubFolder_Folder=$ChosenFolder]`. The XPath constraint is read from right to left, with the resulting Folder (or Folders) being the result. This is key to how you should interpret which direction you are following the association.  

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-retrieve-normal.png"   width="400"  class="no-border" >}}

If the $ChosenFolder object has **Code** `202002141355334` and **Name** `SubFolder2` we have chosen the folder with **ID** `3`. The two folders in the left-hand table, highlighted in orange, will be returned. The platform applies the constraint by default on the right/parent side of the association and returns the relevant ChildFolder (or ChildFolders).

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-retrieve-normal-tables.png" class="no-border" >}}

### Retrieving the Parent Folder from a Folder

When you have the $ChosenFolder object available and you want to retrieve its ParentFolder (the folder next higher in the hierarchy, for example given **SubFolder2** you want to retrieve **MainFolder**) from the database, it becomes more complicated.

Use the expression `[reversed ()]` to instruct Mendix to read the constraint in the reverse direction to that which it would normally use.

{{% alert color="info" %}}
`[reversed()]` only applies to one association. If you have multiple associations they will continue to be interpreted the normal way. See [Creating More Complex Queries](#more-complex), below.

The `[reversed()]` expression can only be applied on self-references. When an association is between two different object types, the platform will be able to determine the direction of the join automatically.
{{% /alert %}}

In our example, we want to find the folder which is the parent of $ChosenFolder. Now, the query becomes `[QueryOver.SubFolder_Folder [reversed ()]=$ChosenFolder]`. Instead of reading the association from right to left (Parent to Child), the association is read from left to right.

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-retrieve-reversed.png"   width="400"  class="no-border" >}}

If the $ChosenFolder object has **Code** `202002141355334` and **Name** `SubFolder2` we have chosen the folder with **ID** `3`. The folder in the right-hand table, highlighted in orange, will be returned. The platform applies the constraint in reverse, on the left/child side of the association and returns the relevant ParentFolder.

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-retrieve-reversed-tables.png" class="no-border" >}}

Here is a video created by our community member [Mike Kumpf](https://developer.mendixcloud.com/link/profile/overview/1360) explaining the use of `reversed()` in an expression.

{{< youtube 5tznw5ZUQgk >}}

### Creating More Complex Queries {#more-complex}

The previous example was a simple one. However the `[reversed()]` expression can be used in more complicated queries.

Say, for example, that each folder can contain multiple files, associated with the folder over the association **File_Folder**.

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-extended-domain-model.png" class="no-border" >}}

You want to retrieve all the files in the parent folder of the folder object $ChosenFolder.

Use the constraint `[QueryOver.File_Folder/QueryOver.Folder/QueryOver.SubFolder_Folder [reversed ()]=$ChosenFolder]` to return all the **File** objects associated with the **Folder** which is associated (as parent) with the **Folder** which is the same as **$ChosenFolder**.

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/query-over-retrieve-complex.png" class="no-border" >}}

If the $ChosenFolder object is `SubFolder2`, you will retrieve all the **File** objects associated with `MainFolder` over the association **File_Folder**.

## Associations to Specializations

In the special case of self-reference when a one-to-many association is with a specialization of itself, you cannot retrieve [by association](/refguide/retrieve/#source).

Here is an example inheritance:

{{< figure src="/attachments/refguide/modeling/domain-model/associations/query-over/limitation.png" class="no-border" >}}

In this example, if a standard by-association retrieve in a microflow is used starting from a `Specialization` this will return the `Specialization` that the starting point `Specialization` points to and not the list of `Generalization` which are associated via the `Generalization_Specialization` association.

The list of `Generalization`'s that points to `Specialization` can be retrieved with a Java action using the [Runtime API](https://apidocs.rnd.mendix.com/11/runtime/com/mendix/core/Core.html#retrieveByPath(com.mendix.systemwideinterfaces.core.IContext,com.mendix.systemwideinterfaces.core.IMendixObject,java.lang.String,boolean)), as shown in this Java action which takes the `Specialization` object and a Boolean `Reverse` to indicate that the object instance is the child of the path of a self association:

```java
public class RetrieveAsAssociatedWithB extends CustomJavaAction<java.util.List<IMendixObject>>
{
	private IMendixObject __B;
	private main.proxies.Specialization B;
	private java.lang.Boolean Reverse;

	public RetrieveAsAssociatedWithB(IContext context, IMendixObject B, java.lang.Boolean Reverse)
	{
		super(context);
		this.__B = B;
		this.Reverse = Reverse;
	}

	@java.lang.Override
	public java.util.List<IMendixObject> executeAction() throws Exception
	{
		this.B = __B == null ? null : main.proxies.Specialization.initialize(getContext(), __B);
 
		// BEGIN USER CODE
		return Core.retrieveByPath(getContext(), __B, "Main.Generalization_Specialization", Reverse);
		// END USER CODE
	}
}
```

{{% alert color="info" %}}
Be sure to import `com.mendix.core.Core` so you are able to execute `Core.retrieveByPath(..)` in this code snippet.
{{% /alert %}}

When setting the `Reverse` Boolean to true and using the `Specialization` object as the input, the returned list will contain all the generalizations associated to the specialization.
