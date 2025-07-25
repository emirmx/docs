---
title: "Recommendations from Best Practice Recommender"
url: /refguide10/performance-best-practices/
description: "Describes Mendix best practices from Best Practice Recommender."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
#The anchors <mxp001-mxpnnn> below are all mapped, so they should not be removed or changed.
---

## Introduction

This document outlines all the best practices provided by [Best Practice Recommender](/refguide10/best-practice-recommender/) in Studio Pro.

## Performance Best Practices {#performance-best-practices}

This section outlines performance issues and Mendix best practices for optimizing an app performance. 

### Calculated Attributes Best Practices [MXP001][MXP002] {#mxp001}

<a id="mxp002"></a>When an object has calculated attributes, each time this object is changed or retrieved from the storage, its calculated attributes are computed by calling a microflow. If the logic behind calculated attributes retrieves other objects or executes Integration activities, it will result in an extra load (and delay) while the outcome of the logic is not used. Creating calculated attributes always affects performance, so you should evaluate whether it is necessary to use them. For more information on attributes, see [Attributes](/refguide10/attributes/).

In most cases, the logic behind a calculated attribute is always executed when the object is used. It is executed whenever there is no retrieval schema for a Retrieve activity (which is the case with data grids). The logic behind calculated attributes is executed in the following elements:

* Retrieve and change object activities in microflows
* In UI widgets (for example, data views, custom widgets)
* When an object is passed from the UI as a parameter to a microflow (for example, a button triggering a microflow).

There are two different performance issues with calculated attributes that you can easily fix:

1. [When you use calculated attributes on a page](#calculated-attribute-on-page)
2. [When there are unused calculated attributes](#unused-calculated-attributes)

#### Avoid Using Calculated Attributes on a Page [MXP001] {#calculated-attribute-on-page}

Retrieve activities trigger the logic of calculated attributes, which can lead to database actions and microflow calls being executed (objects retrieving each other through calculated attributes).

If data containers (list view, data view, or data grid) on a page are using calculated attributes, this may affect the time to load and display the page. 

##### Steps to Fix

To fix the issue, do the following:

1. In the domain model, change the attribute to be stored instead of calculated.
2. Wherever the attribute is about to be committed to the database, calculate the value using the relevant microflow.

{{% alert color="info" %}}
You will also need to migrate any existing data, since when the attribute is changed to be stored, the database will only contain the default value for that data type.
{{% /alert %}}

#### Remove Unused Calculated Attributes [MXP002] {#unused-calculated-attributes}

As Retrieve activities trigger the logic of calculated attributes, it could lead to an execution chain of database actions and microflow calls (objects retrieving each other through calculated attributes).

If calculated attributes are not used, they can safely be removed to avoid redundant microflow calls.

##### Steps to Fix

To fix the issue, delete the unused calculated attribute.

### Add an Index to Attributes in Sort Order [MXP003] {#mxp003}

[Sort order](/refguide10/sort-order/) is used to sort items in data containers. Sort order can be used in three different types of data containers:

* Data grid
* Template grid
* Reference set selector

Each sort item in the sort order is sequentially utilized to order the data in the widget. Adding an [index](/refguide10/indexes/) on the attributes used in sort items can make the sorting process faster, subsequently improve the performance of the page. 

There can be four operations performed on an entity: create, update, delete, and select. Entities, for which the number of create, update, and delete operations is much greater than the number of select operations can be called *write-intensive* because most operations mutate data in a database rather than select from it.

Entities, for which the number of select operations is much greater than the number of create, update, and delete operations can be called *read-intensive* because most operations select data from the database. It is important to perform this optimization only on attributes belonging to entities which are predominantly *read-intensive*. 

As totally different best practices apply for read-intensive and write-intensive entities, it would be valuable to differentiate entities by the type of operations that are performed on the entities.

#### Steps to Fix

To fix the issue, add an index on attributes which are used as sort items in sort orders on pages.

### Avoid Committing Objects Inside a Loop with Create Object, Change Object, or Commit Activities [MXP004][MXP005] {#mxp004}

<a id="mxp005"></a>In a microflow, Mendix objects can be persisted to the database with three activities: the **Create object** activity, **Change object** activity, and **Commit** activity. For objects that are created or changed in a loop, it is not the best practice to commit them immediately in the loop, as this comes with an unnecessary performance overhead. Instead, it is recommended to perform a batch commit of several created/changed objects with the **Commit** activity outside of the loop to reduce database, application, and network overhead. For more information on **Create object**, **Change object**, and **Commit** activities, see [Create Object](/refguide10/create-object/), [Change Object](/refguide10/change-object/), and [Commit Object(s)](/refguide10/committing-objects/).

Committing lists of objects has the following benefits compared to individual commits:

* The prepared statement of creating or modifying records in the database is explicitly reused by the JDBC driver and has the following benefits:
    * The execution plan is cached
    * The driver cares for a minimum of network overhead
* For each database action that changes data the following actions are taken and added overhead:
    * A savepoint is created before the action and released afterwards
    * Auto-committed objects are retrieved from the database
    * Auto-committed objects are stored to the database (if relevant)

#### Steps to Fix for Create or Change Object Activities [MXP004]

To fix the issue for **Create** or **Change object** activities inside the loop, do the following:

1. Change the **Commit** option of a **Create**/**Change** object activity from *No* and make sure created/changed objects are available in a list.
2. Commit the list after the loop when the iteration has finished or when number of objects in the list reaches 1000 to avoid excessive memory usage.

#### Steps to Fix for the Commit Activity [MXP005]

To fix the issue for the **Commit** activity, commit the list after the loop when the iteration has finished or when number of objects in the list reaches 1000 to avoid excessive memory usage.

### Convert Eligible Microflows to Nanoflows [MXP006] {#mxp006}

Nanoflows are executed directly on an end-user's device or browser. This makes them ideal for offline usage. In contrast, microflows run in the Runtime server, thus involve usage of network traffic. Converting an eligible microflow to a nanoflow helps avoiding communication over networks and significantly boosts app performance. For more information on when to use on nanoflows and when to use them, see [Nanoflows](/refguide10/nanoflows/).

You can identify convertible microflows using the following criteria:

* Microflows that have one or more of the following categories:
    * Microflow has logic meant for offline applications.
    * Microflow has logic for online applications but does not involve any database related actions like a committing **Create object**, **Commit**, **Retrieve**, and **Rollback** activities. 
    * Microflow has at-most one database related action. (Not the best practice)
* Microflows that contain nanoflow-compatible activities. For information on activities supported by nanoflows, see [Activities](/refguide10/activities/). 
* Microflow expressions do not contain the following variables: `$latestSoapFault`, `$latestHttpResponse`, `$currentSession`, `$currentUser`, `$currentDeviceType`. These variables are not supported by nanoflows.
* As nanoflows are executed in the context of the current user, ensure that the microflow has only operations for which the current user is authorized. Otherwise the converted nanoflow will fail.

{{% alert color="info" %}}
Security: microflows execute on the server side which in general is simpler to fully secure. If security is a critical concern for your app, note this when deciding whether to convert a microflow to a nanoflow.
{{% /alert %}}

#### Steps to Fix

To fix the issue, do the following:

1. Create a new nanoflow by right-clicking the module and selecting **Add nanoflow**.
2. Replicate the same logic from the microflow. The new nanoflow must look almost identical to the microflow.
3. Check usages of the microflow by right-clicking the microflow and selecting **Find usages**. Replace all usages with the newly created nanoflow.
4. Delete the unused microflow. You can do this by selecting the microflow and pressing <kbd>Delete</kbd> or by right-clicking it and selecting **Delete**.

### Add Index to Attributes that Are Used in XPath Expressions [MXP007] {#mxp007}

[XPath expressions](/refguide10/xpath/) can take a long time to run depending on how many records the underlying entities contain. For read-intensive entities, it makes sense to add an index on the attributes used in XPath expressions. This can significantly boost performance of object retrieval from the database. XPath expressions can also be optimized by ordering them in such a way that the first class excludes as many items as possible. This can be achieved by using indexed attributes earlier in the expression. This will make the rest of the data set to join/filter as small as possible and reduce the load on the database.

Note that XPath expressions can be used in three different places:

* Access rules and entities
* Source/Filter for lists and grids on pages
* Retrieve actions in both microflows and Java actions

#### Steps to Fix

To fix the issue, do the following:

1. Check if the underlying entity contains a substantial amount of records before adding an index (at least 10000 records).
2. Add an index per each attribute used in the XPath expression only for scenarios where read-intensive operations are predominantly performed on the underlying entities.

{{% alert color="info" %}}
This optimization may not be very beneficial for data types like Boolean and enumerations due to a limited number of possible values of these types. It is not recommended to add indexes for such types.
{{% /alert %}}

### Avoid Caching Non-Persistable Entities [MXP008] {#mxp008}

A non-persistable object is an object that is considered temporary and only exists in the memory. It is an instance of a non-persistable entity. For more information on persistable and non-persistable entities, see [Persistablity](/refguide10/persistability/). As these objects exist only in memory, caching them is not useful. On the one hand, it is redundant to create associations of non-persistable entities with System.Session or System.User persistable entities. On the other hand, it is important to cache objects which do not change very often but are used frequently in logic. This will help avoid the overhead of database communication. Persistable entities can be connected to the System.Session of the user and be used as a cache of outcomes. For more information on objects and caching, see [Objects and Caching](/refguide10/objects-and-caching/).

You can use the following guidelines to decide whether caching is needed:

* Data does not change very often
* Data is read very often
* The volume of data is limited (usually less than 10 000 records)
* The impact of using stale data is accepted

#### Steps to Fix

To fix the issue, do the following:

1. For an entity that does not change very often, make it persistable if its objects are used frequently for your logic.
2. If the above condition is not met, remove the association of the non persisted entity with System.User or System.Session.

### Avoid Using Too Many Levels of Inheritance [MXP009] {#mxp009}

Using multiple levels of inheritance and too many specializations on entities may affect performance on large datasets, especially when you are using XPaths for [entity access rules](/refguide10/access-rules/). This generates complex queries, adds XPaths for every specialization access rule, and leads to slow queries. 

#### Steps to Fix

Do not use more than two levels of inheritance or overuse specializations on entities especially if you are using XPath for an entity access.  

Consider the following alternatives:

* Combine attributes in one entity and add an enumeration instead of setting the [generalization](/refguide10/generalization-and-association/)
* Create entities with a one-to-one association instead of setting the generalization
* Create a non-persistable entity that inherits from an outcome of your business logic

### Duplicated Access Rules [MXP010] {#mxp010}

Using duplicated access rules on entities can affect performance, especially when you are using XPaths for [entity access rules](/refguide10/access-rules/). This generates complex queries, adds XPaths for every specialization access rule, and leads to slow queries on a large dataset. 

#### Steps to Fix

To fix the issue, Mendix recommends revisiting your security rules and avoid letting your security model drive your process rules (the security engine is not optimized for process task assignment). You can do the following:

* Consolidate the variation you have in your rules, add additional checks in your microflows to validate state changes rather than having all variations in the access rules.
* Consider splitting your entity in multiple entities with one-to-one associations. On these individual entities you can simplify the access profiles and potentially limit access to the entire entity rather than dozens of individual fields.

### Avoid Deeply Nested Data/List Views [MXP011] {#mxp011}

This best practice applies when a data view or list view is used on a page that is nested to two or more levels, for example, a list view is in another list view and the second list view is in a data view.
When you use two or more levels of nesting, page performance may be affected due to the increased number of requests and transferred data volume.

#### Steps to Fix

To fix this issue, consider restructuring your current page and adding a new one. For example, you can add a pop-up page.

### Avoid Repeatedly Committing a Variable [MXP012] {#mxp012}

A microflow with a repeatedly committed variable may cause longer-running transactions, which may impact performance.

#### Steps to Fix

Refactor the microflow so that the variable is only committed once in the flow.

### Negating XPath Entity Access Rules [MXP013] {#mxp013}

An entity with a large amount of data and access rules that are negating one another may cause performance issues.

If two access rules differ only by negation, you can try to remove the negating rule (the rule that contains `not()` or `= false()`).

#### Steps to Fix

Try to avoid negating XPath rules. Otherwise your app may require database tuning if there is a large number of objects.

### Place Create/Update/Delete Activities Closer to the End Event of a Microflow [MXP014] {#mxp014}

A microflow with Create/Update/Delete activities that are placed too close to the start event may cause the database to acquire locks or resources earlier than necessary.
This may impact app performance.

#### Steps to Fix

Refactor the microflow so that the Create/Update/Delete activities are closer the end event of the microflow.

### Order XPath Parts Around AND: Cheapest and Most-Specific Parts First [MXP015] {#mxp015}

An XPath expression that has more expensive or less-specific parts to the left of `and`, or to the left of a sequence of predicate parts (like `[part 1][part 2]`), may impact app performance.

Depending on the data volume (100,000 items or more) and app usage, the functions `not()`, `contains(unlimited-string-attribute)` and `not(association)` are best placed to the right of the `and` keyword.

Additionally, for data volume of millions of items, placing more specific XPath steps first can mean less overall work for the database.
By "specific" we mean how many association steps are involved. For example, an XPath part that uses an association is more specific than an XPath part that directly accesses an attribute.

#### Steps to Fix

Rearrange the parts of the XPath, so that the cheapest and most-specific parts are to the left of each `and`. Similarly, if using separate predicates like `[part 1][part 2]`, you can rearrange the predicates to have optimal ordering.

### Entity Published in OData with Key That Uses Non-indexed Attribute [MXP016] {#mxp016}

Entities can be published as OData services. Depending on how many records the underlying entities contain, loading the data from the database can take time. For read-intensive entities that are published with a single key, it makes sense to add an index on the attribute used as a key. For larger volumes of data, this can significantly improve performance of object retrieval from the database.

#### Steps to Fix

To fix the issue, do the following:

1. Check if the underlying entity contains a substantial amount of records before adding an index (at least 10000 records).
2. Add an index on the attribute that is used as a key for the published entity.

{{% alert color="info" %}}
This optimization may not be very beneficial for data types like Boolean and enumerations due to a limited number of possible values of these types. It is not recommended to add indexes for such types.
{{% /alert %}}
