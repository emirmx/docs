---
title: "Customization options"
url: /partners/siemens/bom-component-customization/
weight: 3
description: "Describes the customization capabilities that allow developers to add in data from other external systems and how these can be combined into the Teamcenter BOM component."
---

## Augmenting with Mendix Data
Client Columns allow you to display custom data next to Teamcenter properties in the BOM view. The widget calls two microflows:
* `CUSTOM_RetrieveClientColumnDefinitions`: Microflow to retrieve the client column definitions (client column name, width and column order)
* `CUSTOM_RetrieveClientColumnData`: Microflow to retrieve the additional cell-data to display for the BOM lines and client columns. 

As a developer you are free to implement this to your own needs. Additional data can be (but is not limited to) calculated values, third-party data or data from the database.

### CUSTOM_RetrieveClientColumnDefinitions

This microflow is invoked by the TcBOM widget to retrieve the definitions of the custom columns (ClientColumns) to display in the TcBOM widget.

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/custom-retrieveclientcolumndefinitions.png">}}

#### Input parameter
* **Object:** `MendixColumnRoot`
* **Attributes:**
  * `WidgetId` is a unique identifier for the specific TcBOM widget instance making the request. This is useful if you have multiple TcBOM widgets in your application that need to display different columns. You can use this ID to discriminate and return the appropriate data.

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/domain-model-mendixcolumnroot.png">}}

#### Return value
* **Object:** `List of ClientColumn`
* **Attributes:**
  * `Name`: Name of the Client Column. This name is used in the UI of the TcBOM widget as the column title
  * `ColumnOrder`: Order of the column in the widget, respectively. Teamcenter columns also have their own order, so this needs to be filled in conjunction with the Teamcenter columns. Teamcenter columns will take priority in case they have an identical order.

	The first column should be object_string, which is already defined.
All Mendix client columns must start with `columnOrder 2` and would appear before the Teamcenter columns.
Mendix columns are sorted based on their `columnOrder`.
Teamcenter columns appear after the client columns.
The `columnOrder` for Teamcenter columns is defined by the Teamcenter administrator using the column configuration utilities in the **Teamcenter Admin Console**.

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/domain-model-clientcolumn.png">}}


### CUSTOM_RetrieveClientColumnData

This microflow is invoked by the TcBOM widget to retrieve the data to be rendered in the BOM tree. It gathers cell-data for specific BOM lines and any custom columns (`ClientColumns`).

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/custom-retrieveclientcolumndata.png">}}

#### Input parameter: 
* **Object:** `MendixDataRoot`
* **Attributes and associations:**
  * `WidgetId`: A unique identifier for the specific TcBOM widget instance making the request. This is useful if you have multiple TcBOM widgets in your application that need to display different data. You can use this ID to discriminate and return the appropriate data.
  * Associated `MendixDataColumnName` objects: This list specifies which custom columns the widget is configured to display. Each object has a `Name` attribute, representing the identifier of a custom column.
  * Associated `MendixDataBOMLine` objects: This list contains the specific BOM lines for which the widget requires data. Each object includes attributes such as `ItemId`, `RevisionId`, `RevisionUId`, and `BOMlineUid` to identify the BOM line and its ItemRevision.

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/domain-model-mendixdataroot-with-associations.png">}}

#### Return value:
* **Object:** `List of ClientDataResponse`
* **Attributes:**
  * `Value`: the value to display in a targeted cell in the BOM”
  * `ClientColumnName`: the name of the column of the targeted cell
  * `ItemRevisionUID`: the item revision UID of the corresponding BOM line of the targeted cell

{{< figure src="/attachments/partners/siemens/teamcenter-bom-component/domain-model-clientdataresponse.png">}}

### ClientColumn overview page
For convenience, the `TcBOMModule` includes overview and edit pages to define and manage `ClientColumns`.

## Upgrade note
Please be aware that any direct modifications made to this specific microflow within the `TcBOMModule` will be overwritten and lost during future upgrades of the `TcBOMModule`.

To ensure your custom data retrieval logic is preserved across upgrades, we highly recommend you to create two new microflows in your own module that fetches the column definition and cell-data and returns a list of `ClientColumn` objects and `ClientDataResponse` object respectively. These microflows can then be called in `CUSTOM_RetrieveClientColumnDefinitions` and `CUSTOM_RetrieveClientColumnData`.