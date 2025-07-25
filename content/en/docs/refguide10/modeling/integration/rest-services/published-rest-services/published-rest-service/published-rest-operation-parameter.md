---
title: "Operation Parameters for Published REST"
url: /refguide10/published-rest-operation-parameter/
weight: 20
description: "Configure a published REST Operation by adding parameters to an operation "
# If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

When a client calls a published REST operation, it calls a URL with an optional query string and possibly a body. These parameters can be passed to the microflow and import mapping as query parameters, path parameters, body parameters, header parameters, and form parameters.

When you add or edit a parameter in a published REST operation, you can specify the settings described below. These settings are in the **Add parameter** section of the **Add operation for resource** dialog box.

## General

### Parameter Type

Specify where the parameter comes from. Possible values are the following:

* **Query** – When the request contains a query string such as `?name=John&age=42`, you can pass these to the microflow by adding query parameters. For more information, see [Published REST Query Parameters](/refguide10/published-rest-query-parameters/).
* **Path** – The operation path can contain parameters as well. If you add a path parameter, make sure you also add it to the operation. For more information, see [Published REST Path Parameters](/refguide10/published-rest-path-parameters/).
* **Body** – The microflow can have 0 or 1 body parameters. A body parameter is taken from the body of the request. If the body is a file document or an image, the contents will be filled with the body of the request. If the body parameter is another type of object or a list, an [import mapping](/refguide10/import-mappings/) is needed to convert the body content of the request into an object or a list. `GET`, `HEAD`, and `OPTIONS` operations should not have body parameters.
* **Header** – The value of a header parameter is taken from the (first) request header with that name.
* **Form** – The value of a form parameter is taken from the body part with that name (these are available for `multipart/form-data` requests).

### Name

The name of the parameter. For a header parameter, this should be the name of the request header.

### Type

Specify the type of the parameter. Object or list parameters can only come from the body of the request.

### Microflow Parameter

Specify the microflow parameter that will be filled with the value from this operation parameter. You should always select one, except for when you defined a path parameter to be passed to the import mapping.

## Mapping

The mapping group is only shown for body parameters.

### Import Mapping

Specify the import mapping that converts the body of the request (JSON or XML) into an object or a list.

You can use an import mapping that takes a primitive parameter (string, integer, etc.) if the operation has no more than one path parameter with that type. The value of that path parameter will be passed to the microflow. If there is no path parameter, an empty value will be passed to the import mapping.

### If No Object Was Found

This sets the behavior of the operation when a find operation does not find an existing object.

If the top-level of an [import mapping](/refguide10/import-mappings/) has **Decide this at the place where the mapping gets used** unchecked, the behavior is set in the import mapping.

If the import mapping has **Decide this at the place where the mapping gets used** checked, you can define the **If no object was found** action in the REST operation itself. This means you can use the same import mapping in multiple operations, but have a different behavior for each of them. The options are:

* **Create** – Create an object of the correct entity to map to, typically used for `POST` operations.
* **Ignore** – Do not map this element and continue parsing.
* **Error** – Stop parsing the XML and throw an error, typically used for `PUT` and `PATCH` operations.

### Commit

You can indicate whether the import mapping should commit the objects that it creates or changes. You can choose between the following:

* **Yes** – Commits the changes and triggers events, such as validation rules.
* **Yes without events** – Commits the changes without triggering events, such as validation rules.
* **No** – Does not commit the changes, so you can commit them in your microflow. This is useful if you want to add additional checks in your microflow and skip the commit if one of those checks fail.

## Public Documentation

Provide a **Description** of the parameter. You can use [GitHub-flavored Markdown](/refguide10/gfm-syntax/) for rich text.

This is used in the service's [OpenAPI (Swagger) documentation page](/refguide10/published-rest-services/#interactive-documentation).
