---
title: "Debug Java Actions"
url: /refguide/debug-java-actions/
weight: 4
description: "Describes how to set breakpoints and debug Java actions in Eclipse."
aliases:
    - /howto/monitoring-troubleshooting/debug-java-actions/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the Evaluation Guide. See Mapping to Products for more details.
# Linked from https://www.mendix.com/evaluation-guide/enterprise-capabilities/extensibility/
---

## Introduction

Mendix Studio Pro has a built-in debugger to solve errors on the microflow level. A microflow can be extended with custom Java actions, but because these actions are text-based, they can only be checked on compile errors. If you run into an error in any of the Java actions, you can easily debug them by utilizing the debugger of Eclipse.

This how-to teaches you how to do the following:

* Set breakpoints
* Debug in Eclipse

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* Install [Eclipse](https://eclipse.org/)
* Add a Java action and open the app in Eclipse
* Read [Extending Your Application with Custom Java](/refguide/extending-your-application-with-custom-java/)
* Deploy the application for Eclipse by selecting **Deploy for Eclipse** from the app menu in Studio Pro (you should redo this every time you make changes in Studio Pro):
    {{< figure src="/attachments/refguide/java-programming/debug-java-actions/18581045.png" class="no-border" >}}

## Setting Breakpoints

1. Open Eclipse and locate the app in the **Package Explorer**.
2. Double-click **ReverseCustomerName.java**:
    {{< figure src="/attachments/refguide/java-programming/debug-java-actions/18581041.png" class="no-border" >}}
3. Place the cursor on the line that needs debugging, press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> to enable a breakpoint. A blue dot in front of the line will appear: 
    {{< figure src="/attachments/refguide/java-programming/debug-java-actions/18580059.png" class="no-border" >}}
    {{% alert color="info" %}}
    You can also use <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> to disable a breakpoint.
    {{% /alert %}}

## Debugging in Eclipse

1. Select the app root node in the package explorer and click the debug icon in the Eclipse toolbar:
    {{< figure src="/attachments/refguide/java-programming/debug-java-actions/18580062.png" class="no-border" >}}

    The application will now be started with Eclipse attached as debugger.

2. As soon as the deployment process is ready, open the application in your browser and trigger the Java action:
    * As an end-user of the application, you will see a progress bar on your application
    * As a developer, you will see the Eclipse icon flashing on the Windows task bar
3. Open Eclipse. You should now see the "debug" perspective of Eclipse.
4. Click **Step into** (or press <kbd>F5</kbd>) or **Step over** (or press <kbd>F6</kbd>) to move on the next step in the microflow:
    {{< figure src="/attachments/refguide/java-programming/debug-java-actions/18580056.png" class="no-border" >}}

    {{% alert color="info" %}}With debugger options, the difference between **Step into** and **Step over** is only noticeable if you run into a function call. **Step into** means that the debugger steps into the function, and **Step over** just moves the debugger to the next line in the same Java action. With **Step return** (pressing <kbd>F7</kbd>), you can instruct the debugger to leave the function; this is basically the opposite of **Step into**. Clicking **Resume** (pressing <kbd>F8</kbd>) instructs the debugger to continue until it reaches another breakpoint.{{% /alert %}}

5. Place your cursor on any of the variables in the Java action to see its value in a pop-up window:
    {{< figure src="/attachments/refguide/java-programming/debug-java-actions/18580057.png" class="no-border" >}}

    {{% alert color="warning" %}}A current limitation is that local variables may not be visible when debugging Java actions in Eclipse. This can affect your ability to inspect variable values during execution.{{% /alert %}}

## Limitations

There is the following limitation:

* If there are add-on modules imported into Studio Pro and the application is deployed for Eclipse using menu option **App > Deploy for Eclipse**, there are known build failures in Java source code.

## Read More

* [Find the Root Cause of Runtime Errors](/howto/monitoring-troubleshooting/finding-the-root-cause-of-runtime-errors/)
* [Clear Warning Messages in Mendix](/howto/monitoring-troubleshooting/clear-warning-messages/)
* [Test Web Services Using SoapUI](/howto/testing/testing-web-services-using-soapui/)
* [Monitor Mendix Using JMX](/howto/monitoring-troubleshooting/monitoring-mendix-using-jmx/)
* [Debug Java Actions Remotely](/howto/monitoring-troubleshooting/debug-java-actions-remotely/)
* [Log Levels](/howto/monitoring-troubleshooting/log-levels/)
* [Debugging Microflows and Nanoflows](/refguide/debug-microflows-and-nanoflows/)
* [Debugging Microflows Remotely](/refguide/debug-microflows-remotely/)
* [Java Actions](/refguide/java-actions/)
