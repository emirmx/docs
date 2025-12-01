---
title: "Project Categories"
url: /control-center/project-categories/
description: "Manage Project Categories."
weight: 50
---

## Introduction

The **Project Categories** page allows you to manage the categories that can be assigned to your company's projects, so as to improve classification and searchability. 

Project categories can be assigned to each project individually on the app's [Settings](/developerportal/general-settings/) page. Additionally, you can use the [Projects API](/apidocs-mxsdk/apidocs/projects-api/) to edit the category assignment.

## Category Management

On the landing page, you can manage project categories. A list of preconfigured categories is displayed, along with all their corresponding values. Next to the preconfigured **Country** category, you can configure a maximum of five categories with 250 values each. 

Use the **Add Country Category** to make the preconfigured **Country** category available. This specific category contains a list of 250 countries.

{{% alert color="info" %}}
It is not possible to edit the preconfigured **Country** field.
{{% /alert %}}

Click the **Add Category** button or the **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) button within a category to open the category wizard. The wizard allows you to manage the category name and its entries.     

Each entry consists of a unique name and code.     
If you add a new value without a code, the system automatically generates a unique code for you.     

You can add multiple new values as a comma-separated list. 

You can also manage project categories through the [Project Category API](/apidocs-mxsdk/apidocs/project-category-api/).