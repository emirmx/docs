---
title: "Project Categories"
url: /control-center/project-categories/
description: "Manage the Project Categories "
weight: 50
---

## Introduction

The Project Categories page allows you to manage the Project Categories which can be assigned to your companies projects to improve classification and searchability. Project Categories can be assigned to each Project individually in the [Project Settings](/developerportal/general-settings/). Additionally, you can use the [Projects API](/apidocs-mxsdk/apidocs/projects-api/) to patch the category assignment.

## Category Management

On the landing page, you can manage the Project Categories. A list of configured categories is shown, together with all values per category. Next to the preconfigured Country category, you can configure a maximum of five categories with 250 values each. 

Use the **Add Country Category** to make the preconfigured country category available. The country category contains a list of 250 countries.

{{% alert color="info" %}}
It is not possible to edit the pre-configured Country field.
{{% /alert %}}

By clicking the **Add Category** button or the **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) inside a category you open the category wizard in which you can manage a category. You can manage the category name and its entries. Each entry exists of a unique name and code. Whenever you add a new value without a code, the system will automatically generate an unique code for you. It is possible to add new values in a comma seperated list. 

Additionally, a Mendix Admin can manage the Project Categories through the [Project Category API](/apidocs-mxsdk/apidocs/project-category-api/).