---
title: "Navigation Consistency Errors"
url: /refguide10/consistency-errors-navigation/
description: "Describes consistency errors in Mendix Studio Pro and the way to fix them."
#To update screenshots in this document, use the Consistency Errors app.
---

## Introduction 

In this document, we explain how to solve the most common consistency errors that can occur when configuring navigation in Studio Pro. An example of a consistency error is when you set a page that has a data view as a menu item. 

{{% alert color="info" %}}
This document does not describe *all* the errors, as there are a lot of errors that can occur, some of which are simple and do not need extra explanation, others are rare and/or heavily dependent on a use-case. 
{{% /alert %}}

Some errors have error codes and if these errors are described in documentation, Studio Pro has a clickable link to the corresponding document. Others do not have an error code, in this case, you can manually search whether a particular error is described in documentation (you can search by a message you see in the **Errors** pane).

## Navigation Consistency Errors 

The most common errors you can come across when configuring a navigation item are described in the following sub-sections.

### Error Code: CE1568

CE1568 error message: *The selected page {Name of the page} expects an object of type {type of object}, which is not available here.*

You get CE1568 if the page has a page parameter that expects an object of a particular type to be passed to it.

To fix CE1568, pass an object to the page by changing the **On click** property of the menu item from **Show a page** to **Create object**. For more information, see the [Error Fix Example for CE1568](#page-expects-an-object) section below.

#### Error Fix Example for CE1568 {#page-expects-an-object}

When you set a page with a data view as a menu item, you get a consistency error, because the page expects an object to be passed to it. 

For example, you have created a menu item called **Program** for a **Responsive** [profile](/refguide10/navigation/#profiles). This menu item opens the **Program** page. However, the **Program** page has a data view on it and expects a *ProgramItem* object to be passed to it, so that it can show the program details of a specific *ProgramItem* on the page. As a result, you get a consistency error, as no object is passed to this page from the navigation.

{{< figure src="/attachments/refguide10/modeling/consistency-errors/consistency-errors-navigation/page-expects-an-object-error.png" alt="Scheme Showing the Menu Item Error" class="no-border" >}}

To fix the error, you can create an object and pass it to the page. Do the following:

1. Open the navigation for the responsive profile.
2. Open properties of the **Program** menu item, and do the following: 
    1. Change the **On click** property from **Show a page** to **Create object**.
    1. Set **ProgramItem** as **Entity (path)**. 
    1. Set **Program** as **On click page**. 
Now when an end-user clicks the menu item, a new *ProgramItem* object will be created and passed to the page.

### Error Code: CE0529

CE0529 error message: *The selected {Name of the page} expects an object of type {type of object} and cannot be used as a home page. Change the page or use a microflow to provide the page with an object.*

You get CE0529 if you have set a page that expects an object to be passed to it (for example, a page with a data view) as a home page. But the home page has no object that is passed to it, because it is the starting point of a flow.

To fix CE0529, you can use a microflow as the home page that opens the preferred page and pass a specific object to the home page. For more information, see the [Error Fix Example for CE0529](#home-page-expects-an-object) section below.

#### Error Fix Example for CE0529 {#home-page-expects-an-object}

If you set a page that expects an object to be passed to it as a home page for a [navigation profile](/refguide10/navigation/#properties), you will get a consistency error.

For example, you have added a data view that expects an object of type *Customer* to the home page of the responsive profile, and you get a consistency error. 

{{< figure src="/attachments/refguide10/modeling/consistency-errors/consistency-errors-navigation/home-page-error.png" alt="Home Page Error" class="no-border" >}}

You can fix this error by creating a microflow that will that will create a new *Customer* object and pass it to the page. Do the following:

1. Open the responsive navigation profile.
2. In **Default home page field** click **Select**.

    {{< figure src="/attachments/refguide10/modeling/consistency-errors/consistency-errors-navigation/default-home-page-field.png" alt="Default Home Page Setting" class="no-border" >}}

3. In the **Select Navigation Target** dialog box, click **New**, then select **Create Microflow**.
4. Name the microflow *ACT_Open_HomePage*.
5. Open the created microflow, add a **Create object** activity to it 
6. For the **Create object** activity, set **Entity** to **Customer**. 

    {{< figure src="/attachments/refguide10/modeling/consistency-errors/consistency-errors-navigation/create-object-properties.png" alt="Create Object Properties" class="no-border" >}}

7. Add Show Page activity to the microflow and do the following in the **Show Page** pop-up dialog:<br/>

    1. Set **Object to pass** to **NewCustomer**.<br/>
    1. Set **Page** to **Home**.

Now the new object of type *Customer* will be created and passed to the home page.

{{< figure src="/attachments/refguide10/modeling/consistency-errors/consistency-errors-navigation/open-home-page-microflow.png" alt="Open Home Page Microflow" class="no-border" >}}

### Error Code: CE0548

CE0548 error message: *Items with subitems cannot have an action themselves.*

You get CE0548 if you have assigned an [on-click event](/refguide10/on-click-event/) to a menu item that has a sub-item, when menu items with have sub-items cannot have on-click events assigned to them.

To fix CE0548, you need to either set the on-click event of the menu item to *Nothing*, or delete/move the sub-item.

## Read More

* [Navigation](/refguide10/navigation/)
* [Microflows](/refguide10/microflows/)
* [Microflow Properties](/refguide10/microflow/)
