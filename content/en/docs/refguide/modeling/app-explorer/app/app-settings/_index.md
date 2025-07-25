---
title: "App Settings"
url: /refguide/app-settings/
weight: 10
description: "Settings which apply to the app as a whole."
aliases:
    - /refguide/project-settings/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

View your App Settings by clicking **App Explorer** > **App** > **Settings**:

{{< figure src="/attachments/refguide/modeling/app-explorer/app/app-settings/app-settings-location.png" width="200px"  >}}

In the **App Settings** dialog box, you can alter the settings that are applicable to the whole app:

{{< figure src="/attachments/refguide/modeling/app-explorer/app/app-settings/app-settings-configuration.png" width="300px" class="no-border" >}}

The categories described below are available.

## Configurations Tab {#configurations}

A configuration is a group of settings. You can define any number of configurations. The active configuration (meaning, the one that will be used when running your application) is determined by the drop-down menu in the toolbar of Studio Pro.

For more information on settings in a configuration, see [Configuration](/refguide/configuration/).

## Runtime Tab

These settings influence the behavior of the Runtime when running your application.

### Use React Client {#react-client}

This setting enables the React version of the Mendix Client. In Mendix 11.0 and above, the React Client is the default for new applications and the legacy Dojo Client is deprecated. 

The available configuration options are as follows:

* **No**: Do not use the React client. This option will trigger a deprecation warning, as the Dojo client is deprecated.
* **Yes**: Use the React client (default). In this mode, you will get consistency errors for incompatible widgets.
* **Migration mode**: Use the React client and ignore incompatible widgets. Placeholders are displayed in the case of incompatible widgets. Recommended when trying out the new client.

### Static Resources from Disk

If this option is enabled, the static resources for your mobile application are downloaded as soon as you open your application rather than bit by bit as you navigate through the app. This can drastically cut down the number of network requests, as the files can be retrieved from the disk rather than from the server.

The resources are downloaded to the device once for each deployment and are reused for subsequent runs of your app. This affects a number of files, including your theme, the JavaScript client, CSS files, and pages.

### Optimize Network Calls {#optimize-network-calls}

If this option is enabled (**true** by default), Mendix analyzes every microflow that can be triggered from the client to minimize the number of objects required to be sent. This speeds up your app significantly.

If you experience an issue while running your app in which objects seem to be lost, this option can be disabled to resolve that issue. If this does resolve the issue, please file a bug report so that we can fix the issue in the platform.

### URL Prefix{#url-prefix}

Here you have the option to change the default URL prefix for all pages and microflows in your application. The default prefix value is `/p/`.

{{< figure src="/attachments/refguide/modeling/app-explorer/app/app-settings/url-prefix.png" width="300px" class="no-border" >}}

The URL prefix must be alphanumeric. It cannot be empty, contain whitespace, or contain any of the following values: 

* "api-doc"
* "file"
* "odata"
* "odata-doc"
* "reload"
* "rest-doc"
* "ws"
* "ws-doc"
* "xas"

Furthermore, static files are served on `/`. So any prefix that has the same name as a static folder located in `/deployment/web/` will cause an error.

If the URL prefix breaks any of the rules mentioned above, then you will get a consistency error.

### Java Version{#java-version}

Here you can select which Java version to use for you application. For information on how the Java version can influence the behavior of an application, see [Java Version Migration](/refguide/java-version-migration/).

{{% alert color="info" %}}
For Studio Pro 11, you should choose Java 21.
{{% /alert %}}

For local development the Java version configured here needs to have a corresponding JDK configured in the [Studio Pro preferences](/refguide/preferences-dialog/#jdk).

Applications deployed to the cloud will use this setting to select which Java version to use.

### After Startup{#after-startup}

Here you can select a microflow that is automatically executed immediately after the application has been started up.

{{% alert color="warning" %}}
There is a timeout of *11 minutes* on the after startup microflow. If your after startup microflow takes longer than 11 minutes your whole app will fail to start.

**After startup** is designed to initialize the app and therefore runs *before* the app is able to respond to incoming service requests (for example, published REST services).
{{% /alert %}}

### Before Shutdown

Here you can select a microflow that is automatically executed when a shutdown command has been given, just before the application shuts down.

### Health Check

Here you can select a microflow which performs the checks on a running app that you think are required to assess the app's health.

The result of each check is returned as a string, which is displayed in the [Mendix Portal](/developerportal/deploy/environments/). When the microflow returns an empty string, the application is healthy; otherwise, the string presents an explanation of why the application is not healthy.

This microflow gets called every 10 seconds to check if the app is still healthy. This is done by executing it using m2ee on the admin port of your app. For more information, see the section [Health Check](/refguide/monitoring-mendix-runtime/#check-health) in *Monitoring Mendix Runtime*.

{{% alert color="info" %}}

The health check microflow is specific to the [Mendix Cloud](/developerportal/deploy/mendix-cloud-deploy/). For other clouds, the admin port can be called, or the health check microflow can be exposed through a REST API.

{{% /alert %}}

### First Day of the Week {#first-day-of-the-week}

The **First day of the week** setting determines the first day of the week in the date picker widget.

| Option | Description |
| --- | --- |
| **Default (based on locale)**  *(default)* | The first day of the week in date picker widgets is based on the locale of the user. |
| **Sunday** | Use Sunday as first day of the week in date picker widgets. |
| **Monday** | Use Monday as first day of the week in date picker widgets. |
| **Tuesday** | Use Tuesday as first day of the week in date picker widgets. |
| **Wednesday** | Use Wednesday as first day of the week in date picker widgets. |
| **Thursday** | Use Thursday as first day of the week in date picker widgets. |
| **Friday** | Use Friday as first day of the week in date picker widgets. |
| **Saturday** | Use Saturday as first day of the week in date picker widgets. |

### Default Time Zone

The **Default time zone** determines the time zone for newly created users. If your application is only used in one time zone, setting this default will make sure that users of your application never have to worry about setting their time zone.

### Scheduled Event Time Zone {#scheduled}

The **Scheduled event time zone** defines under which time zone scheduled events run. The default is UTC. If you would like to run scheduled events under another time zone (such as the time zone of the company office or the app default time zone), you can select it here.

This affects time zone-related operations, such as parsing and formatting dates from/to strings and obtaining the beginning of the current day.

If you run on-premises, then you can select the time zone to which the server is set. However, please note that no guarantees are given for the whereabouts of application servers in the cloud.

### Hash Algorithm{#hash-algorithm}

The **Hash algorithm** is used to generate hash values for attributes of the hashed string type, such as the password of a user. Mendix offers two recommended hashing algorithms:

| Option | Description |
| --- | --- |
| **BCrypt** (default, recommended) | Resistant to brute-force search attacks. |
| **SSHA256** | Salted Secure Hash Algorithm 2, digest length 256 bits. |

Mendix believes both algorithms are secure enough to store passwords within Mendix. The main difference between **BCrypt** and **SSHA256** is that the BCrypt algorithm has been configured so that it is relatively slow on purpose, since it was designed specifically to stop brute force attacks. That's why this results in a slight performance difference with the SSHA256 algorithm.

#### BCrypt Cost {#bcrypt-cost}

**BCrypt cost** is used to specify the cost of the BCrypt algorithm. The default value is 12, and can go up to 30. The higher the value is, the slower the process of hashing values. For more information, see the subsections below.

#### Performance

If the BCrypt cost is low, the performance difference is hardly noticeable to a single user when signing in (meaning, the password you enter when signing in is hashed using the selected algorithm). This means performance alone is not a reason to choose **SSHA256** over **BCrypt**. The situation can change when dealing with high concurrency of hashing operations, for example, published web services exposing operations that compute quickly, like short-running microflows.

#### Performance Tests

A (web service) user will sign in to execute a web service operation, wait for the operation to finish, and finally get the result back (if any).

Imagine an empty microflow that returns nothing at all exposed as a published web service. We ask one user to execute this operation as many times as they can in one minute (simulated with SoapUI). First we set the hashing algorithm to **BCrypt** (with cost value 10), then we set it to **SSHA256**. Any extra overhead here (on top of establishing the connection, building the XML message and so forth) is basically the hashing algorithm, as the operation should take near zero milliseconds and there is no result. So that leaves only the login, or, more precisely, the hashing of the password.

| Hashing Algorithm | Total Operations Executed | Operation per Second | Overhead in Milliseconds |
| --- | --- | --- | --- |
| BCrypt | 654 | 10.88 | 91.9 |
| SSHA256 | 7163 | 119.36 | 8.4 |

So 80 milliseconds per operation is not that much, right? Well, that depends on how long the operation itself takes.

| Operation Duration in Seconds | Operations per Hour (BCrypt) | Operations per Hour (SSHA256) | Difference % |
| --- | --- | --- | --- |
| 0.1 | 18760 | 33210 | +77% |
| 0.25 | 10529 | 13932 | +32% |
| 1 | 3297 | 3570 | +8% |
| 5 | 707 | 719 | +1.67% |
| 15 | 239 | 240 | +0.5% |

The difference is noticeable when the operation takes less time. So if you expect a very high amount of concurrency in operations where hashing takes place (most commonly any place where login operations are involved), you might want to consider changing your hashing algorithm.

{{% alert color="info" %}}
It is important to remember when changing hashing algorithms that any hashed attribute (like the `System$User` password attribute) has its algorithm set on hashing. In other words, for the hashing type to take effect, any existing hashed attribute will have to be reset using the new hashing type.
{{% /alert %}}

### Rounding Numbers{#rounding}

The **Round Numbers** setting is used to select how to round numbers when performing calculations.

The rounding methods **Half away from zero** and **Half to the nearest even number** indicate how rounding is performed in the case of a tie (for example, 2.5).

This table presents the results of rounding the input to one digit with the given method of rounding numbers:

| Input Number | Half Away from Zero  *(default)* | Half to the Nearest Even Number |
| --- | --- | --- |
| 5.5 | 6 | 6 |
| 2.5 | 3 | 2 |
| 1.6 | 2 | 2 |
| 1.1 | 1 | 1 |
| 1.0 | 1 | 1 |
| -1.0 | -1 | -1 |
| -1.1 | -1 | -1 |
| -1.6 | -2 | -2 |
| -2.5 | -3 | -2 |
| -5.5 | -6 | -6 |

### OQL version 2 {#oql-version-2}

If this option is set to **Yes**, your app will use version 2 of the OQL syntax. This setting must be enabled to use [view entities](/refguide/view-entities/). Make sure your app is ready to use the new syntax before making the switch. 

For more information about the differences, see [OQL Version 2 Features](/refguide/oql-v2/).

Default: *No*

### Multiple Sessions per User {#multiple-sessions}

If this option is enabled, users can sign in multiple times through different clients (for example, desktop browser and tablet). Otherwise, an existing session for a user is signed out when the user signs in somewhere else.

{{% alert color="warning" %}}
In production, this only works with licenses based on concurrent users.
{{% /alert %}}

{{% alert color="info" %}}
When set to *No*, there may still be instances when a user remains signed in even though you wish them to be signed out (for example, if a user clicks the **Home** page from the navigation). This is because logging out is only triggered by a query to the Runtime. Navigating between pages is not enough to trigger a query to the Runtime.

To force a query to the runtime, use microflows. For example, create a microflow that shows the **Home** page, then configure your app's navigation to call this microflow rather than relying on the navigation to directly show the page itself. This will ensure the Runtime is queried and the user is logged out of their session.
{{% /alert %}}

Default: *Yes*

### Foreign Key Constraints {#database-fkc}

If this option is enabled, database [foreign key constraints](/refguide/data-storage/#fkc) will be used. An attempt to commit a dangling reference will throw a runtime exception.

Default: *Yes*

### SSL Certificate Algorithm

Choose between **PKIX (recommended)** and **SunX509 (for backwards compatibility)** as the Java validator and trust manager. According to [this JDK issue](https://bugs.openjdk.org/browse/JDK-8169745), the PKIX validator/trust manager supports richer extensions and features, and the use of SunX509 is discouraged.

Default: **SunX509 (for backwards compatibility)**

## Languages Tab {#languages-tab}

For more information about using different languages in your app, see [Language Menu](/refguide/translatable-texts/).

### Default Language

The **Default language** indicates the language that is used when a user has not chosen a language. The default language is also used as a fall-back language when a certain text is not translated to another language.

### Languages {#languages}

This is the list of languages in which your application will be available for users.

For each language, you can configure whether to check that all mandatory texts have a value. The default language is always checked. If a language is not checked and certain texts are not translated in Studio Pro, the default language is used as fall-back language. This means that you can run your application even though you have only partially translated your interface into a new language.

## Certificates Tab

Certificates are used to connect to web services over HTTPS when the following requirements are met:

* The server uses a self-signed certificate authority, and/or
* A client certificate (certificate with a private key) is required

These certificates can be imported into Studio Pro using the **Import** button. Certificate authority files usually have a *.crt* extension, and client certificates usually have a *.p12* or *.pfx* extension. After importing, use **View details** to acquire more information concerning the certificate.

Client certificates added here will be used whenever a server accepts a client certificate. If you upload more than one client certificate, one of them will be chosen based on the requirements of the server. If you need more control over client certificates, you should not upload the certificates here, but use the [Runtime customization](/refguide/custom-settings/) *ClientCertificates*, *ClientCertificatePasswords*, and *ClientCertificateUsages* settings.

{{% alert color="warning" %}}

When running from Studio Pro or from Eclipse, the certificates will be used automatically to connect over *HTTPS*. When running on a server, the location of the certificate files has to be specified in the configuration file.

{{% /alert %}}
{{% alert color="warning" %}}

Be aware that during local deployment, the certificate files will be located in the **deployment** folder, under **model/certificates**. Therefore, do not use production certificates during development.

{{% /alert %}}
{{% alert color="info" %}}

Certificates can be installed in the Windows Certificate Store using the **Install Certificate** wizard in the **View details** form. This can be useful when trying to access a WSDL-file using an *HTTPS* connection which requires a client certificate.

{{% /alert %}}
{{% alert color="info" %}}

When an SSLException occurs at runtime with the message `HelloRequest followed by an unexpected handshake message` or when a web service does not respond (Java 6 update 21 and above) when using the imported certificates, this is caused by either the client or server not being [RFC-5746](https://www.ietf.org/rfc/rfc5746.txt)-compatible.

If updating the client and server to be compatible with RFC-5746 is not feasible, the following should be added to **Extra JVM parameters** in the **Server** tab to avoid this exception:

`-Dsun.security.ssl.allowUnsafeRenegotiation=true`

Be warned that this does make the client-server communication vulnerable to an exploit which has been fixed in RFC-5746.

When client and server are RFC-5746 compatible at a future point in time, this JVM parameter can be removed.

For background information, see [Transport Layer Security (TLS) Renegotiation Issue Readme](https://www.oracle.com/technetwork/java/javase/documentation/tlsreadme2-176330.html).

{{% /alert %}}

## Theme Tab

### UI Resources Package

The look and feel of a Mendix application is governed by the [UI resources package](/refguide/ui-resources-package/). This package supplies the app with all the required theme information accompanied by matching page templates and building blocks. The module which is designated as the UI resources package is governed by the **UI resources package** setting. Generally, this is automatically updated when a new UI resources package is imported. However, with this setting, the desired module can also be set manually.

### ⚠ Theme ZIP File

{{% alert color="warning" %}}
The use of a ZIP file to configure an app's theme is deprecated. A [UI resources package](/refguide/ui-resources-package/) is the preferred method of sharing themes.
{{% /alert %}}

Older apps may still use a theme ZIP file as the basis for their theme. In this situation, the **Theme ZIP file** setting can be used to switch between any ZIP files found in the **theme** folder. 

{{% alert color="warning" %}}
This practice is deprecated and will be removed in a future version.
{{% /alert %}}

Switching from a ZIP file to a UI resources package is straightforward:

1. Firstly, replace the contents of the theme folder with the contents of the desired ZIP file.

2. Then, use the **UI resources package** setting described above to select a module. Ideally, this module should only contain UI documents, such as page templates and building blocks. This will allow you to export and import the module to other apps without worrying about reference errors.

3. Lastly, set the **Theme ZIP file** setting to **None**.

### Marking as a UI Resources Module

Modules that contain theme styling should be marked as UI resources modules. To do so, right-click the **Module {name}** in the App Explorer, then click **Mark as UI resources module**. This will give the modules a green icon, which makes it easy to distinguish theme modules from other modules, and also influences the order in which styling will be applied from those modules:

{{< figure src="/attachments/refguide/modeling/app-explorer/app/app-settings/green-module.png" alt="green module" class="no-border" >}}

### Ordering UI Resource Modules

When a module contains styling (SCSS/CSS), be sure it is added to the compiled CSS file in the correct order relative to other files. For example, if a theme module should overwrite styling that is defined in **Atlas_Core**, it is important that the theme module is added *after* **Atlas_Core**. 

You can set an explicit order in the theme settings (**App Settings** > **Theme**). This contains a list of all modules that are marked as UI resource modules, and allows you to set the explicit order in which they are added to the CSS file. Note that the lower a module is ordered in the list, the higher its precedence. For example, an app that uses a company theme module could be ordered as follows:

{{< figure src="/attachments/refguide/modeling/app-explorer/app/app-settings/app-theme-settings.png" alt="app theme settings" class="no-border" >}}

## Workflows Tab {#workflows}

### User Entity

**User entity** defines the entity which is used in [target-users-using](/refguide/user-task/#target-users). If you assign a user task using an XPath, you can use attributes of this entity. If you are using a microflow, the entity defines the return type the microflows expects. For more information, see the [Targeted Users Section](/refguide/user-task/#users) section in *User Task*.

### Optimization

Allows you to configure the maximum number of workflow and microflow threads that can be executed simultaneously by the Runtime. This is an advanced setting that gives developers control over app performance. Change these settings when you face performance issues on executing workflow instances or workflow-initiated microflows. The two values indicate the amount of threads that process the queues containing workflow instances or workflow-initiated microflows, for more information see [Workflow Instance Threads](#workflow-instance-threads) and [Microflow Threads](#microflow-threads) sections below. 
App performance can be tracked (from Mendix 9.19 and above) using the following Task Queue metrics:

* `mx.runtime.stats.taskqueue.queue-wait-time` – the amount of time a task has to wait for execution
* `mx.runtime.stats.taskqueue.queue-active-threads` – the actual amount of threads executing tasks from the queue
* `mx.runtime.stats.taskqueue.task-execution-time` – time needed to execute a task from the queue

These metrics have a tag-named queue which has the following values relevant to workflow execution:

* `System.MendixWorkflows-WorkflowExecution` – a queue for workflow instance execution
* `System.MendixWorkflows-DefaultTaskExecution` – a queue for workflow-initiated microflows execution

If the waiting time of the queue increases and active threads in the queue reach the current maximum, it is advised to increase the maximum in settings.

#### Workflow Instance Threads {#workflow-instance-threads}

Defines the maximum number of threads that can process active workflow instances simultaneously. This setting does not relate to the amount of workflow instances that are active in the system.

#### Microflow Threads {#microflow-threads}

Defines the maximum number of workflow-initiated microflows that the Runtime executes simultaneously. Workflow-initiated microflows are microflows defined as event handlers or microflow call activities defined in workflows. This setting has no influence on microflows executed by pages or other parts of the system.

### Event Handlers {#event-handlers}

An event handler allows you to specify a microflow which is triggered when the subscribed event (or events) occur. Each event handler can subscribe to multiple events and there can be multiple event handlers. An event is triggered when the workflow or its activity goes through transitions which warrant the event. This setting is app-wide; you can override it by setting workflow-specific event handlers in [workflow properties](/refguide/workflow-properties/#event-handlers).

An event handler has the following configuration:

* **Name** – describes the event handler
* **Documentation** – provides more information regarding the usage of the event handler
* **When** – allows you to select the [workflow event types](/refguide/workflow-events/#workflow-event-types), for which the handler should be triggered
* **Microflow** – allows you to select a microflow that should be triggered for each of the above selected workflow event types

You can use the data from the event handler microflow to build audit trails or for logging purposes. For example, you can define an event handler that only collects data from user task events.

For more information on workflow events, see [Workflow Events](/refguide/workflow-events/).

### ⚠ Events (Deprecated) {#events} 

{{% alert color="warning" %}}
State-change events are deprecated and replaced with the new [event handlers](#event-handlers) above that also contain events for state changes. It is suggested to migrate the microflows to the new event handlers.
{{% /alert %}}

Events allow you to set a microflow for workflow and user task state changes in your app. 

Security settings of workflows and user tasks allow you to access workflow or user task data only if you have Admin rights or if the workflow/user task is targeted to you. Data from events allows you to build a dashboard or audit trails. For example, it can be useful for a manager to see progress of an employee onboarding process. 

#### Workflow State Change {#workflow-state-change}

A microflow selected for this setting will start every time a workflow changes its state, for example, when the workflow is completed or has failed. This setting is app-wide; you can override it by setting a workflow-specific microflow in [workflow properties](/refguide/workflow-properties/#events).

#### User Task State Change {#user-task-state-change}

A microflow selected for this setting will start every time a user task changes its state, for example, when a user task is completed or paused. This setting is app-wide; you can override it by setting a workflow-specific microflow in [workflow properties](/refguide/workflow-properties/#events).

## Dependencies Tab {#deployment}

This tab can be used to view the managed dependencies in your app in one place and to manage the dependencies in the userlib directory. It contains three tabbed sections.

### Overview

This shows all the direct managed dependencies in your app listed by group and artifact. It shows which versions of the dependencies you have and which modules they are coming from. If your app reports multiple versions of the same group and artifact then the highest version is used, so having multiple versions of a dependency is not necessarily a problem.

### Managed Dependency Exclusions

This shows all the managed dependencies in your app listed by package name. This overview includes both direct and transitive dependencies. If you have conflicts between different dependencies, you can uncheck here any files which you want to exclude. Ensure you leave at least one dependency which supports any calls made by your app or its dependencies.

### Userlib Exclusions

This shows the libraries from the userlib directory and allows you to exclude them from deployment. Use this, for example, if there is an add-on module that ships with a different version of a library that is already in your 'userlib' folder.

## Solution Tab {#solution}

Settings on the **Solution** tab allow you to configure application distribution as an [adaptable solution](/appstore/creating-content/sol-solutions-guide/). 

If you want to distribute your app as an adaptable solution package and allow upgrading it from the [implementer's side](/appstore/creating-content/sol-solutions-impl/), you need to **Enable solution adaptation** on this tab. The title of your app in the App Explorer will change to *Solution* and the solution version will be displayed after the app name.

A distributable app must have a **Solution version** that you can set on this tab.

If you are implementing a solution, **Based on** setting shows the version of the solution package your app is currently based on.

## Miscellaneous Tab {#miscellaneous}

These settings determine the behavior of Studio Pro for this app. The settings apply to everyone that is working on this app.

### Bundle Widgets When Running Locally

When deploying to the cloud, custom widgets are bundled to optimize client-server communication. When deploying locally, this step is skipped to accelerate startup duration. In some cases, this may obfuscate errors triggered by faulty custom widgets.

If this option is set, custom widgets will also be bundled locally. This mimics the production deployment, eliminating risk at the cost of start-up time.

### Use Data Grid 2, Combo Box, and Image Widgets for Content Generation{#use-dg-cb-i}

If this setting is enabled, modern widgets like [Data Grid 2](/appstore/modules/data-grid-2/), [Combo Box](/appstore/widgets/combobox/), and [Image](/appstore/widgets/image/) will be used when generating overview pages or the content of data views. Existing generated content remains as is. 

See the list below for detailed information on which widgets are generated in various circumstances:

* A Data Grid 2 module is generated instead of a Data Grid 1 module
* A combo box is generated instead of a combination of dropdown, reference selector, and input reference set selector widgets
* An image widget is generated instead of a static image widget and a dynamic image widget

### Default Association Storage

You can decide how associations are stored in the database.

This option allows you to change the default for new associations. The initial defaults will be as follows:

* **New projects** – one-to-many and one-to-one associations are implemented as direct associations
* **Upgraded projects** – for projects which are upgraded from an older version of Mendix, all new associations continue to be implemented as association tables

For more information, including which types of association this applies to, see [Association Storage Options](/refguide/association-storage/).

### Suggest Lower-Case Variable Names in Microflows

When enabled, the names that Studio Pro suggests in microflows will start with a lower-case letter instead of an upper-case letter.

### Activity Default Colors

This table allows you to select a default color for each microflow activity type that is available in your app. The selected color will be used as the background color for all microflow activities of that type in your app. It is possible to override this default value for individual activities in the microflow editor. If you change the default color for an activity type, and there are activities of that type present in the app that have an individual background color specified, a dialog will be shown that allows you to apply the new default color to these activities as well.
