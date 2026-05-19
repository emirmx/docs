---
title: "Data Transformer How-Tos"
url: /refguide/data-transformer-how-tos/
weight: 60
description: "Provides practical examples of common JSLT transformation patterns for Data Transformers."
---

## Introduction

These how-tos provide practical, example-driven walkthroughs of common JSLT transformation patterns. Each guide focuses on a specific use case you may encounter when working with real-world API responses, such as restructuring nested data, renaming fields, or combining metadata with data. Rather than covering every detail of the JSLT language, the guides are designed to be hands-on and immediately applicable, showing a concrete input, the expected output, and the transformation that connects the two.

## Filtering Out Unused Fields {#filtering-unused-fields}

It is common for an API to return payloads that contain more fields than your Mendix app or a downstream system need. Rather than passing the entire payload along, this transformation selects only the fields that are relevant, effectively dropping everything else. This keeps the output clean, reduces payload size, and avoids exposing unnecessary data.

### Example

**Input:**

```json
{
  "orderId": "ORD-4521",
  "customerId": "CUST-001",
  "orderDate": "2024-05-10T09:15:00Z",
  "shippingAddress": "456 Elm Street, Berlin, Germany",
  "totalAmount": 149.99,
  "currency": "EUR",
  "paymentMethod": "credit_card",
  "paymentGatewayReference": "PGW-REF-88821",
  "internalFraudScore": 12,
  "warehouseId": "WH-03",
  "status": "shipped"
}
```

**JSLT:**

```jslt
{
  "orderId": .orderId,
  "customerId": .customerId,
  "orderDate": .orderDate,
  "shippingAddress": .shippingAddress,
  "totalAmount": .totalAmount,
  "currency": .currency,
  "status": .status
}
```

**Output:**

```json
{
  "orderId": "ORD-4521",
  "customerId": "CUST-001",
  "orderDate": "2024-05-10T09:15:00Z",
  "shippingAddress": "456 Elm Street, Berlin, Germany",
  "totalAmount": 149.99,
  "currency": "EUR",
  "status": "shipped"
}
```

### Explanation

In JSLT, the output object is always constructed explicitly: only the fields you name in the query will appear in the output, just like SQL or OQL. This means that dropping fields requires no special syntax; any field from the input that is simply not referenced in the JSLT is automatically excluded from the result. In this example, fields such as `paymentMethod`, `paymentGatewayReference`, `internalFraudScore`, and `warehouseId` are dropped by simply not being referred to in the query. The seven relevant fields are carried over directly from the input.

## Simplifying Nested Structures {#simplifying-nested}

Sometimes a JSON structure contains nested sub-objects that group related fields together, but you just need a simpler, flat representation. This transformation moves fields from nested sub-objects up to the top level, merging them into a single flat object.

### Example

**Input:**

```json
{
  "id": "USR-001",
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "profileImage": {
    "url": "https://example.com/images/jane.jpg",
    "altText": "Profile photo of Jane Doe"
  },
  "address": {
    "street": "123 Main St",
    "city": "Munich",
    "country": "Germany"
  }
}
```

**JSLT:**

```jslt
{
  "id": .id,
  "name": .name,
  "email": .email,
  "profileImageUrl": .profileImage.url,
  "profileImageAltText": .profileImage.altText,
  "addressStreet": .address.street,
  "addressCity": .address.city,
  "addressCountry": .address.country
}
```

**Output:**

```json
{
  "id": "USR-001",
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "profileImageUrl": "https://example.com/images/jane.jpg",
  "profileImageAltText": "Profile photo of Jane Doe",
  "addressStreet": "123 Main St",
  "addressCity": "Munich",
  "addressCountry": "Germany"
}
```

### Explanation

The transformation is straightforward. Each field in the output is explicitly mapped from the input using its full path. Fields that were previously nested inside a sub-object are accessed using dot notation (for example, `.profileImage.url`) and assigned to a new top-level key that reflects their origin (for example, `profileImageUrl`). The result is a flat object where all fields sit at the same level.

## Normalising Objects to Arrays (Working with Dynamic Keys) {#normalising-objects}

Some APIs return collections of records as a keyed object, where each key acts as a unique identifier for that record. Mendix works more naturally with lists of objects, so this transformation converts that keyed structure into a flat, normalised array that can be directly mapped to a Mendix entity list.

### Example

**Input:**

```json
{
    "data": {
        "Tag1": {
            "TagName": "TQ-02-FIT-WA123123",
            "Value": 0
        },
        "Tag2": {
            "TagName": "TQ-02-WIT-WA123123",
            "Value": 41.7807087
        },
       "Tag3": {
            "TagName": "TQ-03-FIT-WA123123",
            "Value": 45.812341
        },
        "Tag4": {
            "TagName": "TQ-04-FIT-WA123123",
            "Value": 0
       }
   }
}
```

**JSLT:**

```jslt
[for (.data)
  {
    "TagId": .key,
    "TagName": .value.TagName,
    "Value": .value.Value
  }
]
```

**Output:**

```json
[ {
  "TagId" : "Tag1",
  "TagName" : "TQ-02-FIT-WA123123",
  "Value" : 0
}, {
  "TagId" : "Tag2",
  "TagName" : "TQ-02-WIT-WA123123",
  "Value" : 41.7807087
}, {
  "TagId" : "Tag3",
  "TagName" : "TQ-03-FIT-WA123123",
  "Value" : 45.812341
}, {
  "TagId" : "Tag4",
  "TagName" : "TQ-04-FIT-WA123123",
  "Value" : 0
} ]
```

### Explanation

When you use a `for` expression to iterate over an object, JSLT converts each key-value pair into:

```json
{ "key": "<the key>", "value": <the value> }
```

So `for (.data)` iterates over each entry, exposing `.key` (for example, `"Tag1"`) and `.value` (for example, `{ "TagName": "...", "Value": ... }`). We then construct a new flat object per entry, promoting `.key` into its own `"TagId"` field alongside the fields from `.value`.

## Zipping Metadata with Data {#zipping-metadata}

Some APIs return data and its metadata separately: the metadata describes the structure (for example, column names), while the data is returned as raw arrays. This is the case with, for example, Snowflake SQL REST APIs. To make the data meaningful and easy to consume, the two need to be combined so that each value is associated with its corresponding column name.

In this example, a Snowflake SQL REST API response (with input fields irrelevant to this use case omitted) contains employee records returned as arrays, alongside a separate metadata block that defines the column names. The transformation zips these together to produce a list of objects.

### Example

**Input:**

```json
{
  "resultSetMetaData": {
    "numRows": 4,
    "rowType": [
      { "name": "EMPLOYEE_ID", "type": "fixed" },
      { "name": "FULL_NAME",   "type": "text"  },
      { "name": "DEPARTMENT",  "type": "text"  },
      { "name": "SALARY",      "type": "fixed" }
    ]
  },
  "data": [
    ["1001", "Anna Müller",  "Engineering", "72000"],
    ["1002", "James Carter", "Finance",     "65000"],
    ["1003", "Sofia Rossi",  "Engineering", "78000"],
    ["1004", "Liam O'Brien", "HR",          "58000"]
  ]
}
```

**JSLT:**

```jslt
let cols = .resultSetMetaData.rowType

{
  "data": [for (.data)
    let row = .
    {for (zip($cols, $row))
      .[0].name : .[1]
    }
  ]
}
```

**Output:**

```json
{
  "data" : [ {
    "EMPLOYEE_ID" : "1001",
    "FULL_NAME" : "Anna Müller",
    "DEPARTMENT" : "Engineering",
    "SALARY" : "72000"
  }, {
    "EMPLOYEE_ID" : "1002",
    "FULL_NAME" : "James Carter",
    "DEPARTMENT" : "Finance",
    "SALARY" : "65000"
  }, {
    "EMPLOYEE_ID" : "1003",
    "FULL_NAME" : "Sofia Rossi",
    "DEPARTMENT" : "Engineering",
    "SALARY" : "78000"
  }, {
    "EMPLOYEE_ID" : "1004",
    "FULL_NAME" : "Liam O'Brien",
    "DEPARTMENT" : "HR",
    "SALARY" : "58000"
  } ]
}
```

### Explanation

The transformation starts by storing the column definitions in a variable `$cols` for later use inside the loops. It then iterates over each row in `.data`, capturing the current row as `$row`. For each row, `zip($cols, $row)` pairs every column definition with its corresponding value by position, producing two-element arrays like:

```json
[
  [{"name": "EMPLOYEE_ID", "type": "fixed"}, "1001"],
  [{"name": "FULL_NAME",   "type": "text"},  "Anna Müller"],
  ...
]
```

These pairs are then fed into an object `for` expression, which builds the output object by using the column name (`.[0].name`) as the key and the row value (`.[1]`) as the value.

## Flattening Bill of Materials (BOM) {#flattening-bom}

A Bill of Materials (BOM) is naturally represented as a tree structure, where each assembly can contain child sub-assemblies, which can themselves contain further children. While this hierarchical format is intuitive for authoring, it can be difficult to work with downstream: for example, when loading data into a flat table, a database, or a reporting tool.

This transformation flattens the nested BOM tree into a flat array of assembly objects. Each entry in the flat list is enriched with two new fields:

* `parentSubassembly` – the `partNumber` of the direct parent, or `null` if the assembly is a top-level root.
* `childrenSubassemblies` – an array of `partNumber` values of the direct children.

### Example

**Input:**

```json
{
  "name": "HDM Auto Process A",
  "rootSubAssemblies": [
    {
      "attributes": {
        "baseID": "UID-001",
        "name": "Derivative Electrical",
        "partNumber": "PN183",
        "children": [
          {
            "attributes": {
              "baseID": "UID-002",
              "name": "Fuse ATO 10A",
              "partNumber": "Fuse-ATO-10A",
              "children": []
            },
            "properties": { "value_1": 2, "value_2": "string", "value_3": true }
          },
          {
            "attributes": {
              "baseID": "UID-003",
              "name": "Derivative Final Assembly",
              "partNumber": "PN184",
              "children": [
                {
                  "attributes": {
                    "baseID": "UID-004",
                    "name": "C-13",
                    "partNumber": "C-012-SASP-Y",
                    "children": []
                  },
                  "properties": {}
                }
              ]
            },
            "properties": {}
          }
        ]
      },
      "properties": { "value_1": 2, "value_2": "string", "value_3": true }
    }
  ]
}
```

**JSLT:**

```jslt
def object-to-key-value-array(obj)
  if ($obj)
    [for (array($obj)) {
      "key": .key,
      "value": string(.value)
    }]
  else
    []

def flatten-assemblies(assemblies, parentAssembly)
  [for ($assemblies)
    let parentWithoutChildren = if ($parentAssembly)
      $parentAssembly.attributes.partNumber
    else null
    let childrenWithoutGrandchildren = [for (.attributes.children)
      .attributes.partNumber
    ]
    let currentAssembly = {
      "baseID": .attributes.baseID,
      "name": .attributes.name,
      "partNumber": .attributes.partNumber,
      "parentSubassembly": $parentWithoutChildren,
      "childrenSubassemblies": $childrenWithoutGrandchildren,
      "properties": object-to-key-value-array(.properties),
    }
    let childAssemblies = if (.attributes.children and size(.attributes.children) > 0)
                            flatten-assemblies(.attributes.children, .)
                          else
                            []
    [$currentAssembly] + $childAssemblies
  ] | flatten(.)

{
  "name": .name,
  "flattenedAssemblies": flatten-assemblies(.rootSubAssemblies, null)
}
```

**Output:**

```json
{
  "name" : "HDM Auto Process A",
  "flattenedAssemblies" : [ {
    "baseID" : "UID-001",
    "name" : "Derivative Electrical",
    "partNumber" : "PN183",
    "parentSubassembly" : null,
    "childrenSubassemblies" : [ "Fuse-ATO-10A", "PN184" ],
    "properties" : [ {
      "key" : "value_1",
      "value" : "2"
    }, {
      "key" : "value_2",
      "value" : "string"
    }, {
      "key" : "value_3",
      "value" : "true"
    } ]
  }, {
    "baseID" : "UID-002",
    "name" : "Fuse ATO 10A",
    "partNumber" : "Fuse-ATO-10A",
    "parentSubassembly" : "PN183",
    "childrenSubassemblies" : [ ],
    "properties" : [ {
      "key" : "value_1",
      "value" : "2"
    }, {
      "key" : "value_2",
      "value" : "string"
    }, {
      "key" : "value_3",
      "value" : "true"
    } ]
  }, {
    "baseID" : "UID-003",
    "name" : "Derivative Final Assembly",
    "partNumber" : "PN184",
    "parentSubassembly" : "PN183",
    "childrenSubassemblies" : [ "C-012-SASP-Y" ],
    "properties" : [ ]
  }, {
    "baseID" : "UID-004",
    "name" : "C-13",
    "partNumber" : "C-012-SASP-Y",
    "parentSubassembly" : "PN184",
    "childrenSubassemblies" : [ ],
    "properties" : [ ]
  } ]
}
```

### Explanation

The transformation relies on two helper functions working together. The first, `object-to-key-value-array(obj)`, converts each assembly's properties object into an array of `{ "key": ..., "value": ... }` entries, with all values cast to strings. If properties is `null` or absent, it safely returns an empty array. See [Normalising Objects to Arrays](#normalising-objects) for a detailed explanation of this helper.

The second and central function, `flatten-assemblies(assemblies, parentAssembly)`, is a recursive function that walks the BOM tree and builds the flat output list. For each node it visits, it first determines the parent's `partNumber` (`parentWithoutChildren`), or `null` if the node is a root assembly. It then collects the `partNumber` values of the node's direct children only, without descending further (`childrenWithoutGrandchildren`). Using these, it constructs a flat entry object (`currentAssembly`) for the current node, combining all extracted fields with the normalized properties array.

After building the current entry, the function checks whether the node has any children. If it does, it calls itself recursively on those children, passing the current node as the new parent. If there are no children, it simply returns an empty array, which is the base case that stops the recursion.

Because each iteration produces a nested array structure (`[$currentAssembly] + $childAssemblies`), `flatten(.)` is applied at the end to collapse everything into a single, flat list of assemblies.

The main expression ties everything together. It constructs the output object by passing `.rootSubAssemblies` — the top-level array of root nodes — into `flatten-assemblies`, with `null` as the initial parent since root nodes have no parent. The function then traverses the entire tree from there, and the resulting flat list is assigned to `flattenedAssemblies`. The `name` field is carried over from the input as-is.

## Extracting Information from a String {#extracting-string}

Sometimes multiple pieces of information are encoded within a single structured string, such as a file path, an identifier, or a URL, and you need to extract a specific piece of that information for use downstream or in your own Mendix app. JSLT's string functions allow you to extract each component into its own dedicated field. This makes the data easier to consume, filter, and process without placing any additional burden on the downstream step. In this example, a file path string is broken down into its individual components: the root folder, department, year, and file name, each mapped to a dedicated output field.

### Example

**Input:**

```json
{
  "filePath": "reports/finance/2024/annual-report.pdf"
}
```

**JSLT:**

```jslt
{
  "folder": split(.filePath, "/")[0],
  "department": split(.filePath, "/")[1],
  "year": split(.filePath, "/")[2],
  "fileName": split(.filePath, "/")[3]
}
```

**Output:**

```json
{
  "folder": "reports",
  "department": "finance",
  "year": "2024",
  "fileName": "annual-report.pdf"
}
```

### Explanation

The `split` function breaks the file path string into an array of segments using `/` as the separator, producing the following array:

```json
["reports", "finance", "2024", "annual-report.pdf"]
```

Each segment is then accessed by its index: `[0]` for the first element, `[1]` for the second, and so on. This allows each component of the path to be mapped to a clearly named output field.

For other useful built-in functions, refer to the [JSLT functions documentation](https://github.com/schibsted/jslt/blob/master/functions.md).
