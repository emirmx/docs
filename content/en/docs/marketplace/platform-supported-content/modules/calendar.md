---
title: "Calendar"
url: /appstore/modules/calendar/
description: "Describes the configuration and usage of the Calendar module, which is available in the Mendix Marketplace."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

The [Calendar](https://marketplace.mendix.com/link/component/245304/) module can be used to display and manage calendar events.

### Features

* Add and edit calendar events
* Drag and drop calendar events
* Change calendar event colors
* Retrieve events based on selected date ranges

## Basic Configuration

The calendar module provides default calendar building blocks. Drag the calendar building blocks from the toolbox into the page editor. This will provides basic configuration using calendar's module **CalendarEvent** entity.

### Domain model

The calendar module provides default **CalendarEvent** entity in the domain model.

### Data Source Tab

* **Events** – determines retrieval of the calendar events via context, database, microflow, nanoflow, or association
* **Title attribute** – the String attribute containing the calendar event's title
* **All day attribute** – the Boolean attribute indicating if a calendar event takes a full day
* **Start attribute** – the DateTime attribute indicating the start of a calendar event
* **End attribute** – the DateTime attribute indicating the end of a calendar event
* **Color attribute** – the String attribute affecting the background of a calendar event
    * All HTML supported color formats are supported (for example "red", "#FF0000", "rgb(250,10,20)" or "rgba(10,10,10, 0.5)")
* **Start date attribute** – the DateTime attribute that being used on initial load to be shown in the view.

### View Tab

* **View** – determines the calendar's views:
    * **Standard** – day, week, and month view only
    * **Custom** – custom views configured in **Custom views** tab
    * Default: **Standard**
* **Initial selected view** – determines the view when the calendar becomes visible for the first time
    * Default: **Month**
    * Options are **Day**, **Week**, **Month**, **Custom** (configurable as a custom work week view in the **Custom views** tab), **Agenda**
* **Show event date range** – show the start and end date of the event
* **Time format** – set display time format
* **Day start hour** – the hour of which the day starts (choose between 0 to 23)
* **Day end hour** – the hour of which the day ends (choose between 0 to 24)
* **Show all event** – if set to **yes** the calendar will display all events in a day without "more" links.

### Custom View Tab

#### Visible views
* **Day** – show day button view in the toolbar
* **Week** – show week button view in the toolbar
* **Custom work week** – show custom work week button view in the toolbar. The visible days can be configured on the **Custom view visible days** section
* **Custom view caption** – configure the **Custom work week** button and title
    * Default: **Custom**
    * Configurable when **Custom work week** is set to **Yes**
* **Month** – show month button view in the toolbar
* **Agenda** – show agenda button view in the toolbar

#### Custom view visible days

Configure visible days on **Custom work week** view.

* **Monday** – show monday in the custom week view
* **Tuesday** – show tueday in the custom week view
* **Wednesday** – show wednesday in the custom week view
* **Thursday** – show thursday in the custom week view
* **Friday** – show friday in the custom week view
* **Saturday** – show saturday in the custom week view
* **Sunday** – show sunday in the custom week view

### Events Tab

* **On edit** – specifies action to execute when the user select an existing event in the calendar
    * Default: call the **Calendar.Event_NewEdit** page and set the selected event object as the page's parameter
* **On create** – specifies action to execute when the user select an empty slot in the calendar
    * available variables: **$allday**, **$startDate**, **$endDate** 
    * Default: call the **Calendar.ACT_CreateEvent** nanoflow to create a new **CalendarEvent** entity and set the default variables
* **On drag/drop/resize** – specifies the action to execute when user move or drag an item or change the start or end time by resizing the item
    * available variables: **$newStart**, **$newEnd**, **$oldStart**, **$oldEnd**
    * Default: call the **Calendar.OCH_DragResize** microflow to update existing **CalendarEvent** object based on the available variables
* **On view range change** – specifies the action to execute when calendar view range changes    

### Dimensions Tab

* **Width unit** – the width of the widget.
    * **Percentage** – specifies the width in relation to the rest of the elements on the page.
    * **Pixels** – specifies the width in pixels.
* **Width** – used as an appropriate CSS value.
* **Height unit** – the height of the widget.
    * **Percentage of width** – specifies the height in relation to the width.
    * **Pixels** – specifies the height in pixels.
    * **Percentage of parent** – specifies the width in relation to the rest of the elements on the page.
* **Height** – used as an appropriate CSS value.
* **Minimum height** – applicable if height is relative to parent's percentage.

## Flows

### ACT_CreateEvent

This nanoflows will create a new CalendarEvent entity and show the Event_NewEdit page.

### OCH_DragResize

This microflow will update a given CalendarEvent entity to a new start and end date.

## Page

### Event_NewEdit

This page will show CalendarEvent entity attributes for update or delete purposes.

* If **All day** set to **Yes** then **Start** and **End** will use **Date** format only, otherwise it will use **Date and time** format.
