---
title: "Date Range Field"
url: /refguide8/date-range-field/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

A **Date range field** can be placed inside a [Report date parameter](/refguide8/report-date-parameter/) to make it easier for an end-user to select a date range, rather than having to specify a from and to date. The report date parameter is then updated with the from and to dates of the selected period.

To add a date range field, right-click the widget and choose **Add field** from the pop-up menu.

{{< figure src="/attachments/refguide8/modeling/pages/report-widgets/report-date-parameter/date-range-field/add-field.png" alt="Add a date range field to a report date parameter" class="no-border" >}}

## Date Range Field Properties

An example of date range field properties is represented in the image below:

{{< figure src="/attachments/refguide8/modeling/pages/report-widgets/report-date-parameter/date-range-field/date-range-field-properties.png" alt="Date range field in structure mode"   width="300"  class="no-border" >}}

Date range field properties have only a [General](#general) section.

### General Section{#general}

#### Label

The **Label** property specifies the text that is displayed beside the date range field.

#### Type

**Type** determines the sort of range which the end-user can select.

| Type | Behavior | Example | Range |
| --- | --- | --- | --- |
| Year | Allows the end-user to select a calendar year.¹ | 2019 | 1 January 2019 to 31 December 2019 |
| Quarter² | Allows the end-user to select a quarter of the year. | 2019 > 2 | 1 April 2019 to 30 June 2019 |
| Month² | Allows the end-user to select a month of the year. | 2019 > May | 1 May 2019 to 31 May 2019 |
| Week² | Allows the end-user to select a week of the year. | 2019 > Week 19 | 5 May 2019 to 12 May 2019 |
| Period² | *The Period date range field is being deprecated. It is recommended that you use one of the other types of date range field.*  | | |

| **Notes** |
| --- |
| ¹ The year will be between the **Min. year** and **Max. year** (inclusive) specified in the [report date parameter](/refguide8/report-date-parameter/) widget. |
| ² You also need to add a **Year** date range field if you use a date range field of this type.<br />– The end-user will need to choose the year before they can choose a date range field of this type.<br />– The end-user can only choose one of these types, plus the year. |
