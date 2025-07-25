---
title: "List Values"
url: /apidocs-mxsdk/apidocs/pluggable-widgets-client-apis-list-values/
description: A guide to understanding the list of objects for the datasource property in Mx10.
---

## Introduction

`ListValue` is used to represent a list of objects for the [datasource](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#datasource) property. Corresponding list item values represent properties of different types linked to a [datasource](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#datasource) property.

## ListValue {#listvalue}

When a [`datasource`](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#datasource) property with `isList="true"` is configured for a widget, the client component gets a list of objects represented as a `ListValue`. This type allows detailed access and control over the data source.

```ts
export interface ObjectItem {
    id: GUID;
}

export interface ListValue {
    status: ValueStatus;

    offset: number;
    limit: number;
    setOffset(offset: number): void;
    setLimit(limit: Option<number>): void;
    requestTotalCount(needTotalCount: boolean): void;
    hasMoreItems?: boolean;
    totalCount?: number;
    items?: ObjectItem[];

    sortOrder: SortInstruction[];
    filter: Option<FilterCondition>;
    setSortOrder(sortOrder: Option<SortInstruction[]>): void;
    setFilter(filter: Option<FilterCondition>): void;
}
```

### Pagination {#listvalue-pagination}

The `offset` and `limit` properties specify the range of objects retrieved from the datasource. The `offset` is the starting index and the `limit` is the number of requested items. By default, the `offset` is *0* and the `limit` is `undefined` which means all the datasource's items are requested. You can control these properties with the `setOffset` and `setLimit` methods. This allows a widget to not show all data at once. Instead it can show only a single page when you set the proper offset and limit, or the widget will load additional data whenever it is needed if you increase the limit.

The following code sample sets the offset and limit to load datasource items for a specific range:

```ts
this.props.myDataSource.setOffset(20);
this.props.myDataSource.setLimit(10);
```

You can use the `setOffset` and `setLimit` methods to create a widget with pagination support (assuming widget properties are configured as follows):

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    pageSize: number;
}
```

To set the number of items requested by the datasource you can use the `setLimit` in the constructor of the widget:

```ts
export default class PagedWidget extends Component<PagedWidgetProps> {
    constructor(props: PagedWidgetProps) {
        super(props);

        props.myDataSource.setLimit(props.pageSize);
    }
}
```

To switch to a different page you can change the offset with the `setOffset` method:

```tsx
const ds = this.props.myDataSource;
const current = this.props.myDataSource.offset;
<button onClick={() => ds.setOffset(current - this.props.pageSize)}>
    Previous
</button>
<button onClick={() => ds.setOffset(current + this.props.pageSize)}>
    Next
</button>
```

The `hasMoreItems` property indicates whether there are more objects beyond the limit of the most recent list. When a widget does not show all the records immediately by setting a limit with `setLimit` and allows the user to load additional data, you can use this property to make it clear in the user interface that the user has reached the end of the list.

The following code sample shows a 'load more' button only when there is more data available, and loads additional data when the user clicks the button:

```tsx
const currentLimit = this.props.myDataSource.limit;
this.props.myDataSource.hasMoreItems &&
<button 
    onClick={() => this.props.myDataSource.setLimit(currentLimit + 10)}
>
    Load more
</button>
```

When a `limit` is set to *0*, that case is handled in a special way. In this case `ListValue` will avoid sending a request to the server to retrieve data and will immediately return an empty result. This property can be used to build widgets that load their data "lazily": only when and if a specific condition is met.

The following code sample loads the data only if a button is pressed:

```tsx
export default const LazyWidget = (props: LazyWidgetProps) => {
    useMemo(() => props.myDataSource.setLimit(0), []);
    return props.myDataSource.items?.length ? (
        props.myDataSource.items.map((i) => <div key={i.id}>Item</div>)
    ) : (
        <button onClick={() => props.myDataSource.setLimit(undefined)}>Load data</button>
    );
}
```

The `totalCount` property is the total number of objects the datasource can return. Calculating a total count might consume significant resources and is only returned when the widget indicated needs a total count by calling the `requestTotalCount(true)` method. When possible, please use the `hasMoreItems` instead of the `totalCount` property.

The following code sample shows how to request the total count to be returned:

```ts
export default class PagedWidget extends Component<PagedWidgetProps> {
    constructor(props: PagedWidgetProps) {
        super(props);
    
        props.myDataSource.requestTotalCount(true);
    }
}
```

The `setOffset` and `setLimit` are supported on all [data sources](/refguide/data-sources/#list-widgets). For the `XPath` and `Database` data sources, only the requested page is returned to the client. For other data sources the full set is returned to the client, but the widget will only receive the requested page in the `items` property.

### Sorting {#listvalue-sorting}

It is possible to set a specific sort order for items in the list using `setSortOrder` method and get the current sort order via `sortOrder` field. When a new sort order is set, widget will receive new results on next re-render.

`setSortOrder` method accepts one argument which should be an array of `SortInstruction`s. Also `undefined` could be passed to `setSortOrder` to restore default sort order. 

`SortInstruction` is defined as an array of two elements:

```ts
type SortInstruction = [id: ListAttributeId, dir: SortDirection];
```

The first element of `SortInstruction` type is `id` of an attribute property linked to the datasource. This lets the widget specify which attribute should be used for sorting. Though not every attribute could be used for sorting, for some attributes sorting may be disallowed. To determine if an attribute could be used for sorting `sortable` flag of its attribute property has to be checked. This flag specifies if it is possible to use particular attribute for sorting. See [Attribute ID, Sortable and Filterable Flags](#listattributevalue-id-sortable-filterable) section for more information about attribute `id` and `sortable` flag.

The second attribute of `SortInstruction` type is a literal string representing the sort direction, either `"asc"` or `"desc"`.

The following code examples show how to apply sorting to the property `myDataSource` based on linked attributes `attributeAge` and `attributeName`:

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    attributeAge: ListAttributeValue<BigJS>; // Integer
    attributeName: ListAttributeValue<BigJS>; // String
}
```

Set ascending sort order based on attribute represented by `attributeAge` property with the following code:

```ts
if (this.props.attributeAge.sortable) {
    // sort by age ascending
    const sortInstrs = [
        [this.props.attributeAge.id, "asc"]
    ]; 
    this.props.myDataSource.setSortOrder(sortInstrs);
} else {
    console.warn("Can't apply sorting. Attribute Age is not sortable");
}
```

The following code sample shows how to sort on multiple attributes at the same time:

```ts
if (this.props.attributeAge.sortable && this.props.attributeName.sortable) {
    // sort by age descending, and then by name ascending (within age groups)
    const sortInstrs = [
        [this.props.attributeAge.id, "desc"],
        [this.props.attributeName.id, "asc"],
    ]; 
    this.props.myDataSource.setSortOrder(sortInstrs);
} else {
    console.warn("Can't apply sorting. Attribute Age and/or Name is not sortable");
}
```

Reset to default sort order by passing `undefined` as the following code shows:

```ts
this.props.myDataSource.setSortOrder(undefined);
```

The `setSort` method is supported for all [data sources](/refguide/data-sources/#list-widgets). For `Database` and `XPath` data sources the sorting is done by the back end. For all the other data sources the sorting is done by the client.

### Filtering {#listvalue-filtering}

It is possible to set filtering conditions for items of a datasource. `setFilter()` method accepts filter conditions and applies filtering. `filter` field represents the current filter condition.

`setFilter` accepts only a specially created object of type `FilterCondition` that describes desired filtering behavior. It is possible to create a filter conditions object using functions provided in `mendix` module under `mendix/filters/builders` path. Those functions we call filter builders. Also `undefined` could be passed to `setFilter` to clear filtering conditions.

Some examples of builder functions are `equals`, `greaterThan`, `lessThanOrEqual` for filtering on `DateTime` or `Decimal` attributes. Functions like `startsWith`, `contains` are useful for filtering on `String` attributes. Filtering based on associations is also possible. For example, you can use `equals` with references and `contains` with reference sets.

The following code samples show how to use filter builders and apply filtering to a data source property with three linked attributes and two linked associations:

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    mySelectableObjects: ListValue;
    myAttributeString: ListAttributeValue<string>;
    myAttributeBoolean: ListAttributeValue<boolean>;
    myAttributeNumber: ListAttributeValue<BigJS>;
    myAssociationReference: ListReferenceValue;
    myAssociationReferenceSet: ListReferenceSetValue;
}
```

The `setFilter` method is supported for all [data sources](/refguide/data-sources/#list-widgets). For `Database` and `XPath` data sources the filtering is done by the back end. For all the other data sources the filtering is done by the client. In both cases the widget will receive the filtered items in the `items` property.

#### Simple Filtering {#simple-filtering}

To apply a simple filter based on the value of an attribute represented by `myAttributeString` property the following code may be used:

```ts
import { attribute, literal, startsWith } from "mendix/filters/builders";

// in the widget code
if (this.props.myAttributeString.filterable) {
    const filterCond = startsWith(attribute(this.props.myAttributeString.id), literal("B"));
    this.props.myDataSource.setFilter(filterCond);
} else {
    console.log("Attribute is not filterable");
}
```

The first step the code takes is checking for the possibility to use filtering on `myAttributeString` property by checking the `filterable` flag. Then the `filterCond` filter condition is constructed, which specifies that attribute represented by `myAttributeString` should start with character `"B"`. The `setFilter` call applies the filter, and on the next re-render the component gets only items where the value of an attribute represented by property `myAttributeString` is starting with `"B"`.

Similarly, code like this can apply a condition where the value on an attribute represented by `myAttributeBoolean` property is set to true:

```ts
import { attribute, literal, equals } from "mendix/filters/builders";

// in the widget code
if (this.props.myAttributeBoolean.filterable) {
    const filterCond = equals(attribute(this.props.myAttributeBoolean.id), literal(true));
    this.props.myDataSource.setFilter(filterCond);
} else {
    console.log("Attribute is not filterable");
}
```

Code like this can apply a condition to match only objects that are associated with another object:

```ts
import { association, literal, notEquals, empty } from "mendix/filters/builders";

// in the widget code
if (this.props.myAssociationReference.filterable) {
    const filterCond = notEquals(association(this.props.myAssociationReference.id), empty());
    this.props.myDataSource.setFilter(filterCond);
} else {
    console.log("Association is not filterable");
}
```

Similarly, code like this can apply a condition to match only the objects that are associated with at least the first two objects from the selectable objects data source:

```ts
import { association, literal, notEquals, contains } from "mendix/filters/builders";

// in the widget code
if (this.props.myAssociationReferenceSet.filterable) {
    // assuming those two objects are available
    const objectItem1 = this.props.mySelectableObjects.items[0];
    const objectItem2 = this.props.mySelectableObjects.items[1];
    
    const filterCond = contains(association(this.props.myAssociationReferenceSet.id), literal([objectItem1, objectItem2]));
    this.props.myDataSource.setFilter(filterCond);
} else {
    console.log("Association is not filterable");
}
```

The following code sample shows how to remove the current filtering condition:

```ts
this.props.myDataSource.setFilter(undefined);
```

#### Advanced Filtering {#advanced-filtering}

In some use cases it is necessary to apply more complex filtering conditions. For example if a use case requires fetching only items where `myAttributeString` starts with `"B"` and `myAttributeBoolean` is set to `true`, or items where `myAttributeNumber` is greater than `10` and `myAssociationReference` is associated with another object while `myAssociationReferenceSet` is not associated with any other objects. In order to construct such a condition, special filter builders `and` and `or` have to be used. The following code sample shows how to use them. Note that checks for the `filterable` flag have been omitted for simplicity. Real widgets should always take the `filterable` flag into account.

```ts
import { attribute, association, literal, startsWith, equals, notEquals, greaterThan, and, or } from "mendix/filters/builders";

// in the widget code
if (/* check that all properties are filterable */) {
    const filterCond = or(
        and(
            startsWith(attribute(this.props.myAttributeString.id), literal("B")),
            equals(attribute(this.props.myAttributeBoolean.id), literal(true))
        ),
        and(
            greaterThan(attribute(this.props.myAttributeNumber.id), literal(10)),
            notEquals(association(this.props.myAssociationReference.id), empty()),
            equals(association(this.props.myAssociationReferenceSet.id), empty())
        )
    );
    this.props.myDataSource.setFilter(filterCond);
} else {
    console.log("Some attribute is not filterable");
}
```

### Reloading {#listvalue-reload}

It is possible to reload items of a datasource. The `reload()` method triggers a new fetch from the underlying data source, preserving existing `filter`, `offset`, `limit`, `requestTotalCount`, and `sortOrder` properties. The `reload()` method accepts no arguments.

### Working With Actual Data

The `items` property contains all the requested data items of the datasource. However, it is not possible to access domain data directly from `ListValue`, as every object is represented only by GUID in the `items` array. Instead, a list of items may be used in combination with other properties, for example with a property of type [`attribute`](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#attribute), [`action`](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#action), or [`widgets`](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#widgets). See the next section for detailed information about working with different property types in combination with `ListValue`.

### View State {#view-state}

View state is a mechanism of storing the current state of a page when user navigates away from the page and restoring that state when user navigates back to the page. For example user has some sorting order applied in a DataGrid widget on an overview page and navigates away to a detail page. When user gets back to the overview page, the DataGrid widget will be initialized with previously used sorting order.

View state works transparently for a widget, no additional steps needed from the widget in order to benefit from view state mechanism. 

The following information of a `ListView` is getting automatically stored and restored:

* Pagination state (`limit` and `offset` fields)
* Sorting state (`sortOrder` field)
* Filtering state (`filter` field)

### Status of the List Value Items {#status-of-the-list-value-items}

The `status` property provides the component with additional information about the state of the items and how the component should handle them:

```tsx
export const enum ValueStatus {
    Loading = "loading",
    Unavailable = "unavailable",
    Available = "available"
}

if (this.props.listValue.status === ValueStatus.Available) {
    return (
        <div>
            ...
        </div>
    );
} else if (this.props.listValue.status === ValueStatus.Loading) {
    return <p>Loading... Please, wait...</p>;
} else if (this.props.listValue.status === ValueStatus.Unavailable) {
    return <p>There are no available items to show.</p>;
}
```

More specifically, the `status` property functions as follows:

* When `status` is `ValueStatus.Available`, then the list value items are accessible, and the result is exposed in the `items` array.
* When `status` is `ValueStatus.Unavailable`, then the list does not have any available data and the `items` array is `undefined`. This can be the case if the data source depends on a surrounding data view which has no data.
* When `status` is `ValueStatus.Loading`, then the list is waiting for new data to arrive. This can be triggered by a change in data that the data source depends on (such as a parent data view) or by an entity update, which occurs if an object of that type is committed or deleted. If this is done from a microflow, a [refresh in client](/refguide/change-object/#refresh-in-client) is also required.
    * If the list value was previously in a `ValueStatus.Available` state, then the previous `items` array is still returned. This allows a component to keep showing the previous items if it does not need to handle the `Loading` state explicitly, which prevents flickering.
    * In other cases, the `items` is `undefined`. This happens if a page is still being loaded or if the previous state was `ValueStatus.Unavailable`.

## Linked Property Values {#linked-values}

### ListActionValue {#listactionvalue}

`ListActionValue` represents action that may be applied to items from `ListValue`. The `ListActionValue` is an object and its definition is as follows:

```ts
export interface ListActionValue {
    get: (item: ObjectItem) => ActionValue;
}
```

In order to call an action on a particular item of a `ListValue` first an instance of `ActionValue` should be obtained by calling `ListActionValue.get` with the item (assuming widget properties are configured as follows):

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    myListAction: ListActionValue;
}
```

The following code sample shows how to call `myListAction` on the first element from the `myDataSource`.

```ts
const actionOnFirstItem = this.props.myListAction.get(this.props.myDataSource.item[0]);

actionOnFirstItem.execute();
```

In this code sample, checks of status `myDataSource` and availability of items are omitted for simplicity. See the [ActionValue section](/apidocs-mxsdk/apidocs/pluggable-widgets-client-apis/#actionvalue) for more information about the usage of `ActionValue`.

### ListAttributeValue {#listattributevalue}

`ListAttributeValue` represents an [attribute property](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#attribute) that is linked to a data source.
This allows the client component to access attribute values on individual items from a `ListValue`. `ListAttributeValue` is an object and its definition is as follows:

```ts
export interface ListAttributeValue<T extends AttributeValue> {
    get: (item: ObjectItem) => EditableValue<T>; // NOTE: EditableValue obtained from ListAttributeValue always readonly

    id: ListAttributeId;
    sortable: boolean;
    filterable: boolean;

    type: AttributeType;

    formatter: ValueFormatter<T>;
    universe: Option<T[]>; // only for attributes of type Enumeration
}
```

#### Obtaining Attribute Value {#obtaining-attribute-value}

{{% alert color="warning" %}}
Due to a technical limitation it is not yet possible to edit attributes obtained via `ListAttributeValue`. `EditableValue`s returned by `ListAttributeValue` are always **readonly**.
{{% /alert %}}

In order to work with the attribute value of a particular item of a `ListValue` first an instance of `EditableValue<T>` should be obtained by calling `ListAttributeValue.get` with the item. The type `<T>` depends on the allowed value types as configured for the attribute property. 

Let's take a look at some example. Assuming widget properties are configured as follows with `myAttributeOnDatasource` property allowing attribute of type `string`:

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    myAttributeOnDatasource: ListAttributeValue<string>;
}
```

The following code sample shows how to get an `EditableValue<string>` that represents a read-only value of an attribute of the first element from the `myDataSource`.

```ts
const attributeValue = this.props.myAttributeOnDatasource.get(this.props.myDataSource.items[0]);
```

Note: in this code sample checks of status of `myDataSource` and availability of items are omitted for simplicity. See [EditableValue section](/apidocs-mxsdk/apidocs/pluggable-widgets-client-apis/#editable-value) for more information about usage of `EditableValue`.

#### Attribute ID, Sortable and Filterable Flags {#listattributevalue-id-sortable-filterable}

`id` field of type `ListAttributeId` represents the unique randomly generated string identifier of an attribute. That identifier could be used when applying sorting and filtering on a linked data source property to identify which attribute should be used for sorting and/or filtering. For more information, see the [Sorting](#listvalue-sorting) and [Filtering](#listvalue-filtering) sections.

Fields `sortable` and `filterable` specify if the attribute could be used for sorting and/or filtering. Those flags have to be checked before a widget applies filtering or sorting on a data source property. Any attempt to filter on a non-filterable attribute or sort on a non-sortable attribute leads to an error during the execution time.

#### Attribute Type

The [attribute](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#attribute) property defines which attribute types can be configured for that property. For example, an attribute property may be configured to allow attributes of type `String` and `Integer` in order to present progress. While this is convenient for users, it may require additional work for developers by processing different data types.

It is possible to determine the type of attribute by checking the `type` field on an attribute property. The following code sample shows how to check the attribute type on the property named `myAttributeOnDatasource`:

```ts
if (this.props.myAttributeOnDatasource.type === "String") {
    console.log("String attribute");
} else if (this.props.myAttributeOnDatasource.type === "Integer") {
    console.log("Integer attribute");
} else {
    console.log("Not a String/Integer attribute");
}
```

#### Formatter and Universe

The `formatter` field represents the default formatter used on values obtained by the `get` function.

The optional `universe` field represents an array of possible values for an attribute. For more information, see the `universe` field of [EditableValue](/apidocs-mxsdk/apidocs/pluggable-widgets-client-apis/#editable-value).

### ListReferenceValue and ListReferenceSetValue {#listassociationvalue}

`ListReferenceValue` and `ListReferenceSetValue` are both used to represent an [association property](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#association) that is linked to a data source. This allows the client component to access associated values of individual items from a `ListValue`. `ListReferenceValue` and `ListReferenceSetValue` are both objects and their definitions are as follows:

```ts
export type ListReferenceValue = ListAssociationValue<ObjectItem> & { type: "Reference" };

export type ListReferenceSetValue = ListAssociationValue<ObjectItem[]> & { type: "ReferenceSet" };

export interface ListAssociationValue<T extends ObjectItem | ObjectItem[]> {
  get: (item: ObjectItem) => DynamicValue<T>;

  id: ListAssociationId;
  filterable: boolean;
}
```

#### Obtaining Association Values

In order to work with an object or objects that are associated with a particular item returned by `ListValue`, first an instance of `DynamicValue<ObjectItem>` (for `ListReferenceValue`) or `DynamicValue<ObjectItem[]>` (for `ListReferenceSetValue`) should be obtained by calling `get` with the item. 

If the association property has been configured to allow both types of associations, the type of the property is defined as `ListReferenceValue | ListReferenceSetValue` and a check on its `type` should be done to narrow down the type. For more information, see the [Association Type](#association-type) section.

Consult the following example code, which assumes widget properties are configured with the `myAssociationOnDatasource` property allowing association of type `Reference`:

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    mySelectableObjects: ListValue;
    myAssociationOnDatasource: ListReferenceValue;
    myAttributeOnSelectableObjects: ListAttributeValue;
}
```

The following code example shows how to get a `DynamicValue<ObjectItem>` that represents a read-only value of an associated object of the first element from the `myDataSource`:

```ts
const associationValue = this.props.myAssociationOnDatasource.get(this.props.myDataSource.items[0]);
```

This will return an `ObjectItem` representing the associated object, because in this example the widget is configured to allow only singular associations. If you want to access the individual attribute values of this associated object, you may use an attribute property linked to the selectable objects data source and pass the associated object to it. For more information, see [Obtaining Attribute Value section](/apidocs-mxsdk/apidocs/pluggable-widgets-client-apis-list-values/#obtaining-attribute-value).

Please note these code samples omit checks of `myDataSource` status and availability of items for simplicity. See [DynamicValue section](/apidocs-mxsdk/apidocs/pluggable-widgets-client-apis/#dynamic-value) for more information on the usages of `DynamicValue`.

#### Association ID and Filterable Flags {#listassociationvalue-id-filterable}

The `id` field of type `ListAssociationId` represents the unique randomly-generated string identifier of an association. That identifier can be used when applying filtering on a linked data source property to identify which association should be used for filtering. For more information, see the [Filtering](#listvalue-filtering) section.

The `filterable` field specifies if the association can be used for filtering. This flag has to be checked before a widget applies filtering on a data source property. An attempt to filter on a non-filterable association leads to an error during the execution time.

#### Association Type {#association-type}

The [association](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#association) property determines which association types could be configured for that property. For example, an association property may be configured to allow associations of type `Reference` and not `ReferenceSet`.

It is possible to determine the type of association by checking the `type` field on an association property. This is useful if the property has been configured to allow both references and reference sets. The following code sample shows how to check the association type on the property named `myAssociationOnDatasource`:

```ts
if (this.props.myAssociationOnDatasource.type === "Reference") {
  console.log("Reference association");
} else {
  // TypeScript will narrow it down to "ReferenceSet" when the type is not equal to "Reference"
  console.log("ReferenceSet association");
}
```

### ListWidgetValue {#listwidgetvalue}

`ListWidgetValue` represents a [widget property](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#widgets) that is linked to a data source. This allows the client component to render child widgets with items from a `ListValue`.
`ListWidgetValue` is an object and its definition is as follows:

```ts
export interface ListWidgetValue {
    get: (item: ObjectItem) => ReactNode;
}
```

For clarity, consider the following example using `ListValue` together with the `widgets` property type. When the `widgets` property named `myWidgets` is configured to be tied to a `datasource` named `myDataSource`, the client component props appear as follows:

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    myWidgets: (i: ObjectItem) => ReactNode;
}
```

Because of the above configurations, the client component may render every instance of widgets with a specific item from the list like this:

```ts
this.props.myDataSource.items.map(i => this.props.myWidgets.get(i));
```

When the `widgets` property is not required, there may not be any child widgets configured. In that case the value of the widgets property will be `undefined` (as in the example above `myWidgets`).

### ListExpressionValue {#listexpressionvalue}

`ListExpressionValue` represents an [expression property](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#expression) or [text template property](/apidocs-mxsdk/apidocs/pluggable-widgets-property-types/#texttemplate) that is linked to a data source. This allows the client component to access expression or text template values for individual items from a `ListValue`. `ListExpressionValue` is an object and its definition is as follows:

```ts
export interface ListExpressionValue<T extends AttributeValue> {
    get: (item: ObjectItem) => DynamicValue<T>;
};
```

The type `<T>` depends on the return type as configured for the expression property. For a text template property, this type is always `string`.

In order to work with the expression or text template value of a particular item of a `ListValue`, first an instance of `DynamicValue` should be obtained by calling `ListExpressionValue.get` with the item (assuming widget properties are configured as follows with an expression of type `boolean`):

```ts
interface MyListWidgetsProps {
    myDataSource: ListValue;
    myExpressionOnDatasource: ListExpressionValue<boolean>;
    myTextTemplateOnDatasource: ListExpressionValue<string>;
}
```

The following code sample shows how to get a `DynamicValue` that represents the value of an expression for the first element from the `myDataSource`.

```ts
const expressionValue = this.props.myDataSource.myExpressionOnDatasource.get(this.props.myDataSource.item[0]);
```

## Filter Helpers{#filter-helpers}

### Value Helpers {#filter-value-helpers}

Two basic helpers that allow to represent attributes and literal values in filter conditions are `attribute` and `literal` helpers. When creating a filter condition, every attribute or literal value has to be wrapped with a corresponding helper.

#### Attribute

The `attribute` helper takes one argument of type `ListAttributeId`. See [ListAttributeValue](#listattributevalue).

The following code sample shows how to apply `attribute` helper and use its result in constructing a filter condition:

```ts
const attrA = attribute(this.props.myAttributeA.id);
const filterCondition = equals(attrA, literal("Bob"));
```

Attribute types available for filtering:

* `Boolean`
* `DateTime`
* `AutoNumber`
* `Integer`
* `Long`
* `Decimal`
* `Enum`
* `String`
* `HashString`

Attribute types **not** available for filtering:

* `Binary`
* `EnumSet`
* `ObjectReference`
* `ObjectReferenceSet`

#### Literal

The `literal` helper takes one argument. Accepted argument types are:

* Boolean values for `Boolean` attribute types
* String literals for `String`, `HashString` and `Enumeration` attribute types 
* `BigJS` numbers for `AutoNumber`, `Integer`, `Long` and `Decimal` attribute types
* `Date` objects for `DateTime` attribute type
* `undefined` for any attribute type

The following code sample shows how to use `literal` helper:

```ts
const falsy = literal(false); // for Boolean
const bob = literal("Bob"); // for String, HashString, Enumeration
const meaningOfLife = literal(new BigJS(42)); // for AutoNumber, Integer, Long, Decimal
const now = literal(new Date()); // for DateTime
const undef = literal(undefined);
```

### Basic Helpers

#### Equals

The `equals` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Accepts attributes and literals of any type.

The following code sample shows how to use `equals` helper:

```ts
const attrA = attribute(this.props.myAttributeA.id);
const name = literal("Bob");

// filter keeps items where value equals "Bob"
const filterCondition = equals(attrA, name);
```

#### NotEqual

The `notEqual` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Accepts attributes and literals of any type.

The following code sample shows how to use `notEqual` helper:

```ts
const attrA = attribute(this.props.myAttributeA.id);
const name = literal("Bob");

// filter keeps items where value not equal to "Bob"
const filterCondition = notEqual(attrA, name);
```

#### GreaterThan

The `greaterThan` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `HashString`, `Enumeration`, `AutoNumber`, `Integer`, `Long` `Decimal`, `DateTime` attributes and their corresponding literals.

The following code sample shows how to use `greaterThan` helper:

```ts
const attr = attribute(this.props.myAttributeA.id);
const meaningOfLife = literal(new BigJS(42));

// filter keeps items where value is greater than 42
const filterCondition = greaterThan(attr, meaningOfLife);
```

#### LessThan

The `lessThan` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `HashString`, `Enumeration`, `AutoNumber`, `Integer`, `Long` `Decimal`, `DateTime` attributes and their corresponding literals.

The following code sample shows how to use `lessThan` helper:

```ts
const attr = attribute(this.props.myAttributeA.id);
const meaningOfLife = literal(new BigJS(42));

// filter keeps items where value is less than 42
const filterCondition = lessThan(attr, meaningOfLife); 
```

#### GreaterThanOrEqual

The `greaterThanOrEqual` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `HashString`, `Enumeration`, `AutoNumber`, `Integer`, `Long` `Decimal`, `DateTime` attributes and their corresponding literals.

The following code sample shows how to use `greaterThanOrEqual` helper:

```ts
const attr = attribute(this.props.myAttributeA.id);
const meaningOfLife = literal(new BigJS(42));

// filter keeps items where value is greater than or equals 42
const filterCondition = greaterThanOrEqual(attr, meaningOfLife); 
```

#### LessThanOrEqual

The `lessThanOrEqual` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `HashString`, `Enumeration`, `AutoNumber`, `Integer`, `Long` `Decimal`, `DateTime` attributes and their corresponding literals.

The following code sample shows how to use `lessThanOrEqual` helper:

```ts
const attr = attribute(this.props.myAttributeA.id);
const meaningOfLife = literal(new BigJS(42));

// filter keeps items where value is less than or equals 42
const filterCondition = lessThanOrEqual(attr, meaningOfLife); 
```

### String Conditions

#### Contains

The `contains` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `Integer`, `Long`, `Decimal` attributes and `String` literals.

The following code sample shows how to use `contains` helper:

```ts
const attrStr = attribute(this.props.myAttributeA.id); // string attribute
const subStr = literal("secret");

// filter keeps items where value has a substring "secret"
// like "my secret password", "secret file", "top secret"
const filterCondition1 = contains(attrStr, subStr);

// also works with numeric attributes
const attrNum = attribute(this.props.myAttributeB.id); // integer attribute
const subNum = literal("1337");

// filter keeps items where value has sequence of numbers "1337"
// like "133700", "1231337", "913379"
const filterCondition2 = contains(attrNum, substrNum);
```

#### StartsWith

The `startsWith` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `Integer`, `Long`, `Decimal` attributes and `String` literals.

The following code sample shows how to use `startsWith` helper:

```ts
const attrStr = attribute(this.props.myAttributeA.id); // string attribute
const subStr = literal("secret");

// filter keeps items where value starts with substring "secret"
// like "secret file", but not "my secret password" or "top secret"
const filterCondition1 = startsWith(attrStr, subStr);

// also works with numeric attributes
const attrNum = attribute(this.props.myAttributeB.id); // integer attribute
const subNum = literal("1337");

// filter keeps items where value stats with sequence of numbers "1337"
// like "133700", but not "1231337" or "913379"
const filterCondition2 = startsWith(attrNum, substrNum);
```

#### EndsWith

The `endsWith` helper takes two arguments produced by [Value helpers](#filter-value-helpers).
Allows only `String`, `Integer`, `Long`, `Decimal` attributes and `String` literals.

The following code sample shows how to use `endsWith` helper:

```ts
const attrStr = attribute(this.props.myAttributeA.id); // string attribute
const subStr = literal("secret");

// filter keeps items where value ends with substring "secret"
// like "top secret", but not "my secret password" or "secret file"
const filterCondition1 = startsWith(attrStr, subStr);

// also works with numeric attributes
const attrNum = attribute(this.props.myAttributeB.id); // integer attribute
const subNum = literal("1337");

// filter keeps items where value ends with sequence of numbers "1337"
// like "1231337", but not "133700" or "913379"
const filterCondition2 = startsWith(attrNum, substrNum);
```

### Logic Conditions

#### And

The `and` helper is used to combine other conditions in *logical and* operation. Takes 2 or more arguments.

The following usage example specifies that *all conditions have to be true* for an object in order to appear in the resulting filtered set:

```ts
const filterCondition = and(
    startsWith(attribute(this.props.myAttributeA.id), literal("Hi")), // myAttributeA starts with string "Hi"
    equals(attribute(this.props.myAttributeB.id), literal(5)), // myAttributeB equals 5
    greaterThan(attribute(this.props.myAttributeC.id), literal(new Date())) // myAttributeC greaterThan current date and time
);
```

#### Or

The `or` helper is used to combine other conditions in *logical or* operation. Takes 2 or more arguments.

The following usage example specifies that *at least one condition have to be true* for an object in order to appear in the resulting filtered set:

```ts
const filterCondition = or(
    endsWith(attribute(this.props.myAttributeA.id), literal("Z")), // myAttributeA ends with string "Z"
    graterThan(attribute(this.props.myAttributeB.id), literal(10)), // myAttributeB greater that 10
    equals(attribute(this.props.myAttributeC.id), literal(true)) // myAttributeC equals True
);
```

#### Not

The `not` helper inverts a condition. It takes one argument.

The following usage example specifies that `myAttributeA` have to start with any letter except `"X"` by inverting `startsWith` condition:

```ts
const filterCondition = not(
    startsWith(attribute(this.props.myAttributeA.id), literal("X")),
);
```
