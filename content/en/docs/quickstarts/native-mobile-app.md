---
title: "Add a Native Mobile App"
url: /quickstarts/part2/
weight: 20
description: "Learn the basics of making a native mobile app."
tags: ["hello world", "microflows", "widgets", "app", "nanoflow", "app development"]
aliases:
    - /refguide/quickstart-part2/
    - /refguide9/quickstart-part2/
---

## 1. Introduction 

This guide is a continuation of part one, and in it, you will recreate the web application you created in part one as a native mobile application.  The guide aims to get you started with Mendix Native Mobile and walks you through the steps on how to develop your first native app using the Studio Pro IDE.

By following this guide, you will:

* Learn how to **create, run, and deploy** a native mobile application
* Use **nanoflows** to build logic
* Learn about **offline-first** development and **data synchronization**
* Test your native app using the **Make it Native app**

## 2. Mobile development essentials

The Mendix Platform enables you to build apps of many different kinds, including web, native mobile, and PWA. When choosing to build native mobile applications, there are some special requirements to keep in mind:

* [Offline-first development](https://docs.mendix.com/refguide/mobile/building-efficient-mobile-apps/offlinefirst-data/): Native mobile apps run on devices that cannot guarantee a stable connection to the internet all the time. When developing these types of apps, it's important to design your app with this in mind. Data is stored in a local database on the mobile device and is periodically synchronized with the server.

* [Data synchronization](https://docs.mendix.com/refguide/mobile/building-efficient-mobile-apps/offlinefirst-data/synchronization/): In order to send and receive updates to the server, the native mobile application will need to synchronize its local database with the cloud environment’s server. This is triggered using the synchronize action in nanoflows and the synchronize to device action in microflows. You can configure your sync behavior to only update what is needed for each specific user to minimize data use and load times.

## 3. Prerequisites

Before starting this tutorial, make sure you have completed the following prerequisites:
* Download and install the [Make It Native](/refguide/getting-the-make-it-native-app/) app on your mobile device, and confirm that the Mendix version you are using is [compatible](/refguide/mobile/getting-started-with-mobile/prerequisites/#get-min-app) with your Make It Native app version
* Complete part one of this guide series, [Building a Responsive Web App](/quickstarts/responsive-web-app/)
* If you are working on a Mac, complete [Configuring Parallels](/refguide/using-mendix-studio-pro-on-a-mac/) to install Studio Pro on your Mac


## 4. Creating the App

To get started building your first native mobile app, click **Create New App** in the Studio Pro **Select App screen**. Then select the **Blank Native Mobile App** template as the starting point for your app.

{{< figure src="/attachments/quickstarts/part2/4.a Create New App.png" width="450px" alt="Create Blank Native Mobile App">}}

Next, click **Use This Starting Point**.

{{< figure src="/attachments/quickstarts/part2/4.b Blank Native Mobile Template.png" width="450px" alt="Use this starting point">}}

In the pop-up window that appears, enter a **name** for your app. Leave all other options as default and click **Create app**.

{{< figure src="/attachments/quickstarts/part2/4.c Confirm Create App.png" width="450px" alt="Confirm your choices and create app">}}

## 5. Creating the Domain Model

Just as in part one, we will need to create the entity to store employee data. In the **domain model** for the Native Mobile Module, add a **new entity**.

1. Name the entity Employee.

2. The entity should be a generalization of the Account entity.

3. Add three attributes to the entity:
    * FirstName as String
    * LastName as String
    * JobRole as String

4. Click **OK** to confirm your changes.

{{< figure src="/attachments/quickstarts/part2/5. Creating The Domain Model.png" width="450px" alt="Create the Domain Model">}}

## 6. Creating the User Interface

With the domain model finished, it’s time to start creating a user interface for your app. To get started, open the page **Home_Native** in the app explorer.

1. **Delete** all prefilled “Welcome” text and containers from the page.

2. From the **Toolbox**, drag a **List View** onto your page.

3. Double-click on the **List view** to open its **properties**.

4. In the **properties window**, go to the **Data source tab**, ensure **Database** is selected next to **Type**.

5. Under entity, click the **Select** button.

6. Choose the **Employee** entity we created earlier and click **Select**.

7. Click **OK** to confirm your choices.

{{< figure src="/attachments/quickstarts/part2/6.a Creating the UI.png" width="450px" alt="The User Interface">}}

8. When asked if you would like to automatically fill in the content, select **Yes**.

9. Delete all the content except for the attributes we created (**FirstName, LastName, JobRole**).

10. Add a **Layout Grid** to the page (4x4x4), then move each of the remaining labels into their **own cells** in the layout grid.

{{< figure src="/attachments/quickstarts/part2/6.b Layout Grid.png" width="450px" alt="add the layout grid">}}

11. Add a **Create** button from the toolbox in the empty header section of the page.

12. After adding the button to the page, a window will appear for you to select the entity that will be created when the button is clicked. Select the **Employee** entity and click **Select**.

{{< figure src="/attachments/quickstarts/part2/6.c Select Entity.png" width="450px" alt="Select entity to display">}}

13. Next we must select the on-click page that will be displayed when the button is clicked. In the properties for the button, in the events section, next to **on-click** page, click **Select**.

14. In the pop-up window that appears, click to create a **New page**.

15. In the next window, enter the name for the new page as Employee_NewEdit. Leave the layout as is and select the **Edit With Dataview template** before clicking **OK** to confirm your choices.

## 7. Application Logic

Now that we have finished with the domain model and user interface, we can continue to create logic using nanoflows. Let’s create a nanoflow to replace the default save button for the Employee_NewEdit page. This nanoflow will validate that the details have been entered correctly, and then commit the changes and synchronize with the server.

{{% alert type="info" %}}
**What is a nanoflow?**

Nanoflows are similar to microflows, as they allow you to build complex logic for your application. Visually, they appear similar, and some actions can be used in both microflows and nanoflows (You can also convert microflows to nanoflows automatically!). Nanoflows run on the client and are based on JavaScript. Microflows run on the server and are based on Java
{{% /alert %}}

1. Open the **properties** of the **save button**.

2. Under **on-click** action, select **Call a Nanoflow**.

3. In the window that appears, click New to create a new nanoflow.

4. **Name** the new nanoflow ACT_Employe_NewEdit

5. In the newly created nanoflow, look for an **orange Decision** in the toolbox and drag it onto the flow.

6. **Double-click** the decision to open its **properties**.

7.Enter the caption Has Firstname?

8. Under **Expression**, add 'trim($Employee/FirstName)!= empty'. This will remove any whitespace from the string and then check to ensure there are characters in the string attribute.

9. As the expression above results in a true and false result, we need to **create a branching path** from the decision - a path for each possible result. Select the **decision**, then from a corner **click and drag away** to create the new alternative path. Ensure to define which path is for the true result and the false result by **right-clicking** them both and under condition value choosing **true** and **false**.

10. On the **false** path, add a **validation feedback action**. Double click to open its **properties**, enter the following details, and click **OK**:

    * Variable ->Employee
    * Member → FirstName
    * Template → Please enter a Firstname for the employee

11. Now on the **true path**, repeat this step for the **LastName** and **JobRole** attributes as well.
    * You can copy and paste the decision and validation feedback action to save time (But don’t forget to update the actions for each attribute)

12. At the end of the flow on the true path, add a **commit action**. Open its **properties** by double-clicking the action and ensure **Employee** is selected for **Object or List**, and change **refresh in client** to **Yes**. Click **OK** to close to window.

13. Add a **synchronize action** after the commit action and choose to synchronize only unsynchronized objects.

14. Add a **close page action** after the synchronize action.

{{< figure src="/attachments/quickstarts/part2/7. Completed Nanoflow.png" width="450px" alt="Completed nanoflow">}}

## 8. Deploying Your App

In order to easily test your application on a device, run your application in Studio Pro and then open the **Make it Native app** on your mobile device. Once your app is running, click the **dropdown arrow** next to **View App** and choose **View on a Device**. Locate the **QR code** used to view your native mobile app, and then scan it using the Make it Native app (If you are using [Parallels](https://docs.mendix.com/refguide/using-mendix-studio-pro-on-a-mac/), you may need some special configurations).

Once you scan the **QR code** using **Make it Native**, the app should load, and you will be able to test your application. In order to deploy your application to the app stores (Apple and Android), you will need to create a signed build of your application. Mendix recommends using [Bitrise](https://docs.mendix.com/refguide/mobile/distributing-mobile-apps/building-native-apps/bitrise/) to package your app for distribution.

## 9. Finished

Congratulations on completing and deploying your very first Native Mobile App! Next, head over to the [Academy](https://academy.mendix.com/link/home) and get started with our **Crash Course** learning plan, or continue learning here in our docs pages by searching for topics you are interested in. For videos on the latest news and updates, head to our [YouTube page](https://www.youtube.com/c/MendixCommunity). Or see what our awesome community is up to on our [Medium publication](https://medium.com/mendix). 

Looking to get in touch with us or the community? Join our [Slack community workspace](https://join.slack.com/t/mendixcommunity/shared_invite/zt-39m9sfzsl-so7j70WRyj_4gJ33gaVXOw) and get involved.

## 10 Read More

* [Studio Pro Overview](/refguide/studio-pro-overview/) – describes Studio Pro tabs, menus, and shortcut keys
* [Mendix Best Practices for Development](/refguide/dev-best-practices/) – a reference for adopting consistent naming and modeling conventions while developing your Mendix apps
* [Starting with App from a Spreadsheet](/refguide/app-from-spreadsheet/) – describes importing a Microsoft Excel spreadsheet and building an app using your data
* [Mendix Academy Become a Rapid Developer](https://academy.mendix.com/link/paths/31/Become-a-Rapid-Developer) – the learning path recommended for new Mendix users who want to create their first app using low-code
* [Mendix Academy Crash Course](https://academy.mendix.com/link/paths/82/Crash-Course) – the learning path recommended for new Mendix users who are also experienced developers
