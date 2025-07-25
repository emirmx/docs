---
title: "Navigation In Native Mobile Apps"
linktitle: "Navigation"
url: /refguide10/mobile/designing-mobile-user-interfaces/navigation/
weight: 20
description: "General information for native navigation in Mendix."
aliases:
    - /refguide10/native-navigation/
---

## Introduction

Although you model native mobile apps in similar ways to web applications, some aspects of navigation in native mobile apps differ from those in web applications. This guide will explain those navigation differences.

### Web Apps

Generally in web apps, there is only one page open at any given time. This is because when you open a page, the previous page is replaced with a new one. When you navigate back, the page you started on is loaded again to replace the current page. This pattern of page loading slows performance.

### Native Mobile Apps

In native mobile apps, pages are kept open by default. This makes it possible to instantly navigate back (for example by tapping the back button or by swiping) while retaining the state of previous pages such as scroll location and active tab information. This greatly benefits your app's user experience. However, Mendix recommends taking open pages into account when developing mobile apps. Specifically, make sure that there will not be too many pages open at once (which will result in bad performance), and that data is refreshed or updated when needed. To achieve these ends, Mendix gives you granular control over your exact navigation flow.

## Layout Types

### Default

After opening a page that has a default layout, you can navigate back to the previous page by using the back button in the header, the back button of the device (for Android), or by swiping from the left side of the screen (for iOS). When the app’s home page is open, pressing the Android back button closes the app.

### Pop-Up

A page with a pop-up layout looks like a default page, but it shows up with a distinct animation and has a close icon instead of a back icon. On iOS it can also be closed by swiping down from the top of the pop-up. On Android it can be closed using the back button of the device. Pop-ups cannot have a bottom bar. A page with a pop-up layout cannot be used as the default home page, nor can it be added to the bottom bar.

Pop-ups can be very useful when asking for input in certain contexts. For example, they work well when you must scan a QR code. They also make wizards (forms with multiple steps) easier, as you can return to previous steps and change things until the process is finished and all pop-ups are closed.

## Navigation-Related Layout Components

Native layouts have helpful properties that enable the most common patterns used in native apps.

### Header{#header}

A layout that has the header property enabled will always show a bar at the top of the screen. A header consists of three parts: 

* Left part: not configurable, but can show a back icon or a close icon depending on the layout type
* Center part: shows the page's title 
* Right part: configurable with widgets and is often used to contain buttons

This is an example of the default header on iOS:

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/header.png" alt="An example of the default header on iOS."   width="250"  class="no-border" >}}

### Bottom Bar

You can add bottom bar items in the native navigation profile. These items will show up on any page that has a layout with the bottom bar property enabled.

Every item in the bottom bar has its own navigation stack. This means that if you open a few pages in the first tab, then switch to the second tab and back to the first tab again, your first tab's pages will still be open as you left them.

{{% alert color="warning" %}}
Pages without a bottom bar are created in a separate stack. If you navigate from a page *without* a bottom bar to a page *with* a bottom bar, then all pages in that stack are closed.
{{% /alert %}}

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/bottombar.png" alt="An example of the default bottom bar on iOS."   width="250"  class="no-border" >}}

### Sidebar

You can use the **Sidebar** in your native projects' navigation systems. It is a common pattern in mobile applications to use the **Drawer** from left side to navigate between screens IT is also common to keep useful links and application settings there.

There are only a few steps needed to be done to create a **Sidebar** in your native project. First of all, you need to include **NativePhone_SideMenu**, which you can find under **Phone Layouts** in **Atlas Core**:

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/includesidebar.png" alt="Include a sidebar" >}}

Then, decide which screen in your project will have a **Sidebar** and apply the appropriate layout — **NativePhone_SideMenu** in our case:

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/selectsidebarlayout.png" alt="Select a sidebar layout" >}}

After that, you will see the **Sidebar** appear on your screen. You can place any content inside of it, and you can see it using **Sidebar Toggle** button (which will appear in the left part of your [Header](#header)):

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/sidebarlayout.png" alt="Sidebar layout" >}}

## How does Mendix Native Navigation Work?

By default pages are kept open to provide a better user experience, but they must also be managed correctly for your app to have great performance and a logical flow through the app.

### Stacks

To keep pages open, the navigation system uses multiple stacks of pages. A mobile app can contain multiple stacks at the same time. A stack can be compared to a pile of cards: you can add cards to the top, and you can remove cards from the top.

#### Single Stack

If your app does not use a bottom bar, then there is a single stack. This situation resembles navigation on the web most closely. However, it is important to keep in mind that all pages in the stack are kept open until you explicitly close them or press the back button.

The first page on the stack is always the home page. When you tap a button that opens a page, then there are two pages on the stack, and so on.

When you close a page (via a back button, a close action, or swipe to go back (iOS)), only the current page is closed and the previous page becomes visible again.

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/singlestack.gif"   width="250"  class="no-border" >}}

#### Multiple Stacks

If an app has bottom bar items, every item in the bottom bar will have a separate stack. Within a stack you can navigate by opening and closing pages.

If a bottom bar item is not focused, pressing it will focus that item. Switching to another bottom bar item will not close pages in the focused one. If the item is already focused, pressing it again will dismiss all pages from its stack.

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/multiplestacks.gif"   width="250"  class="no-border" >}}

#### Pop-Ups

When a page with a pop-up layout is opened, a new stack is created and all pages in this stack fully cover the screen. To get back to the previous stack, the pop-up has to be closed.

It is possible to open other pop-up pages inside the pop-up, and all of those together behave as a single stack. When opening a normal page from the pop-up, the pop-ups will be closed first.

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/popups.gif"   width="250"  class="no-border" >}}

### Opening and Closing Pages

Often you need to have control of page history to determine, for example, which page is shown when you navigate back. By default, the Close page action only closes a single page in the current stack, but it can be configured to close more pages.

#### Closing Multiple Pages

You can navigate back through multiple pages in history at once by setting the appropriate number of pages in your Close page action. A Close page action that closes multiple pages results in a single animation.

You can configure this behavior either on Close page actions that you have modeled on your pages, or on the Close page activity in your nanoflows. This number can be set using an expression so that you can set either a static number, or a snapshot-calculated one for more complex cases.

The **All** option can be used to close all pages in the current stack.

#### Combining Closing and Opening Pages

For more complex scenarios, it is possible to transparently remove one or more pages from the history from the current stack when navigating to a new page. Doing so ensures that users cannot inadvertently navigate back to pages that are no longer relevant. For example, when a user reaches the end of a three-step wizard flow, you can configure the Open page activity to open the third page and close the previous two pages at the same time. As a result, the user will see a single transition, and navigating back will not lead them to the first two pages.

The **All** option can be used to close all pages in the current stack.

In addition, you can configure the Open page action to **Clear history** when navigating to a page. This effectively resets the entire navigation system, not just the current stack, and the user will not be able to navigate back from the target page.

### Navigation Transitions

In order to add more flexibility and customization to mobile apps, Mendix offers different options for how an app's opening screen or popup animation can look.

You can find and customize these options in [Navigation Profile](/refguide10/navigation/):

{{< figure src="/attachments/refguide10/mobile/native-mobile/native-navigation/navigationtransition.png" >}}

By default, all screens and popups will inherit the appropriate behavior of their operating system (either iOS or Android). 

Among the available options for screen transitions, you can choose between **"Slide from right"** or **"Scale from center"**. For popups, we have **"Modal Presentation"** and **"Bottom Sheet"** as well. Once you select these options, they will apply to the native mobile app on both operating systems.

For specific cases, we also offer an option **"Apply screen options also for popups"**. You can use it if you need your popups to look and behave the same as usual screens. Once you select this option, any popups options will be disabled and dismissed.

{{% alert color="info" %}}
Please remember; whenever you make changes in this section, a new build of the native mobile app has to be created and distributed. 
{{% /alert %}}

### Updating Data

It is important to remember that any changes you make to your data will be immediately reflected on all active screens. However, this does not mean that these changes are also committed to the database; that remains an explicit action. Another consequence of this is that your app is responsible for keeping track of (and possibly reverting) changes to data when the user decides to navigate back (for example by pressing the back button) without saving their changes. 

## Best Practices and Patterns

### Make it Simple for the User

It is important for a user to always understand where they are in the app, and what their next options are. Overall it is useful to show the bottom bar so that the user sees where they are and where they can go. However, in some flows it makes sense to create focus and remove distractions by using a layout without a bottom bar or by using a pop-up layout.

### Minimize Open Pages to Improve Performance

While the user navigates through an app, all pages stay open in the background. This uses the device's resources, and having too many open pages can cause performance issues. The consequence of this is that the choice between using an Open page or a Close page action should be made more deliberately than with web applications.

Where possible, use the Close page action (or the above-mentioned variants of the Show page action) as often as possible to ensure that you do not keep too many unnecessary pages open.

### Use Pop-Ups for Modal Interactions

Most applications consist of a set of primary user flows, and each primary flow may have secondary flows, for example to request some additional user input. In the case of a calendar app, browsing upcoming events would be the primary flow, and editing one event might be a secondary flow. In general, secondary flows should not disrupt the primary flow, meaning that after the user completes this secondary flow, they should end up where they were in the primary flow.

Although such behavior can be modeled using the ability to close multiple pages, Mendix recommends using pop-up pages for such scenarios. Pop-up pages exist entirely outside of the navigation stack, and can be opened and closed without disrupting your app's navigation history.

### Startup Flows

The Open page action's **Clear history** option can be used to model out complex behavior, but one of its most common use cases is a startup flow. Imagine an app that should show a series of tutorial screens on startup. After these screens, the user should land on the main page. From these, it should not be possible to navigate back. This can be achieved by using the Open page action with the **Clear history** option to navigate from the final tutorial screen to the main page.

## Customization

Out of the box, the bottom bar and top bar have a default styling. To change this styling, see the [Pages](/refguide10/mobile/designing-mobile-user-interfaces/widget-styling-guide/#pages) section of the *Widget Styling Guide*. However, if you want to further customize the styling to match your design, you can create pluggable widgets to replace some of the default navigation components. For more information, see the [Pluggable Widgets API Documentation](/apidocs-mxsdk/apidocs/pluggable-widgets/)

## Read More

* [Show Page Guide](/refguide10/show-page/)
* [Close Page Guide](/refguide10/close-page/)
* [Navigation Widget](/refguide10/mobile/designing-mobile-user-interfaces/widget-styling-guide/#navigation-widget) section of the *Widget Styling Guide*
