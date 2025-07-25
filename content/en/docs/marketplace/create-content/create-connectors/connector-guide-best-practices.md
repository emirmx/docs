---
title: "Best Practices for Creating Connectors"
url: /appstore/creating-content/connector-guide-best-practices/
weight: 7
---

## Introduction

This document provides details and recommended best practices for building, testing, and distributing connectors. 

## App Setup {#app-setup}

This section dives into best practices for setting up your app in Studio Pro. For basic instructions, see the [Setting Up Your App](/appstore/creating-content/connector-guide-build/#app-setup) section of *Creating Connectors*.

### Studio Pro Version

For people to use your connector, ensure that you are not only targeting end-users that have the latest version Studio Pro. Try to support the latest [LTS](/releasenotes/studio-pro/lts-mts/) version and, if possible, even older supported versions. 

The only reason to deviate from this is when your connector requires a certain platform functionality that is only available in a newer version, or requires another module that is only available on a newer version. In that case, use that newer version as your Mendix required version.

### Modules in the App

Create a Mendix app in Studio Pro with the main connector module and a testing module. You can also add an example module showing some use cases.

* The main module contains all the logic needed to let your connector function in apps that import it. Give the main module the name you want to appear in apps after others import your module.
* The testing module has microflows, pages, and Java code to test your module's functionality. This means that you will not have to add the test logic to the module that you will later export and publish.
* You can publish the example module next to the connector main module and testing module to help end-users better understand the implementation of your connector module, or to try it out without having to do too much configuration from the start.

#### Setting Up the Main Connector Module {#main-setup}

Mendix recommends that the main module for your connector include the following:

* **_Docs** (folder) – Contains documentation or a reference to documentation, and a version indicator.
    * **ReadMe** (snippet) – Used to give a reference to documentation and/or some direct documentation on how to use the module.
    * **[ModuleName]_[Version]** (string constant) – Replace [ModuleName] with the name of your module, and [Version] with the version of the module. Use the same standard as the Mendix Marketplace 1.2.3. See the [Versioning](#versioning) section below.
        * As an alternative to the version constant, you can include a subfolder with the version indication.
* **Private** (folder) – This folder contains all the logic that other developers should not touch when implementing your module.
* **UseMe** (folder) – Contains everything the implementing developer could use to implement your module in their application. It can include subfolders for **Microflows**, **Pages**, **Snippets**, **Templates**, and **Constants**.

Your **App Explorer** should look like this after the initial setup:

{{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/app-initial-setup.png" class="no-border" >}}

{{% alert color="info" %}}
This app folder structure example is based on the **Blank Web App** starter app, and contains Mendix Marketplace modules that your app might not have. The important Marketplace module that is shown is the [Unit Testing](/appstore/modules/unit-testing/) module for testing purposes.
{{% /alert %}}

#### Setting Up the App Root {#root-setup}

Mendix recommends adding additional folders to the root of your app, on disk. These include the following:

* **DIST** – This contains the releases of your module kept for reference.
* **MarketplaceResources** – This contains all the assets used in the Mendix Marketplace, such as screenshots, videos, and reference documents. It does not contain the release packages.

The app root of your **App Explorer** should look like this after root setup:

{{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/app-post-setup.png" class="no-border" >}}

#### Importing Dependency Modules

Use as few dependency modules as possible. When you must use one, make sure it is either platform supported or, if you are creating a company-only connector, maintained by your own company. This ensure that users are are always supported. Otherwise, you might end up having to maintain those public non-platform supported modules to ensure that your connector operates properly.

### Adding the App to Version Control

If you have not already added your app, add it to the [Team Server](/refguide/version-control/#team-server). An alternative solution that Mendix recommends is having a separate public Git repository next to your private Team Server Git repository. This way, you can continue committing and applying changes while you only push to the public repository for a new release to the Marketplace. The automated integration with GitHub from the Marketplace makes this a good option for your public-facing Git repository.

### Working with Java-Intensive Apps

For Java-intensive apps, consider creating an **Implementation** submodule to keep core implementation separate. You can also make the module a Gradle project in order to better manage dependencies and the release pipeline. Create the *gradle.build* file to manage Java library dependencies and the cleanup of the **userlib** folder for export. For further information, see [Extending App Setup for Building Connectors with Java](#extend-app-java) below.

## Development

This section explores best practices for connector development. You can develop connectors using Java or Mendix.

### Developing Connectors Using Java

Implementing the bulk of the functionality in Java has the following benefits: 

* Complexity is hidden from end-users, making apps easier to understand.     
{{% alert color="info" %}}
Mendix apps should focus on solving business requirements. Generic technical constructs should be hidden from developers. Currently, Java is the best way to achieve this. A module does not have to ship the Java source for its Java functionality. You can also make a (hidden) *.jar* in another app, and place that in the connector module **user lib**.
{{% /alert %}}
* End-users are less likely to change it.
* You can use [unit tests](#unit-testing).

#### Extending App Setup for Building Connectors with Java {#extend-app-java}

You can extend your app to work with Java and Gradle. Follow these steps to do that:

1. Add an **Implementation** folder next to other app folders. 

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/implementation-folder.png" class="no-border" >}}

2. Set up an  **Implementation** folder as a Gradle `java-library` project. For details, see the Gradle documentation on [Building Java Libraries Sample](https://docs.gradle.org/current/samples/sample_building_java_libraries.html#run_the_init_task). This is the expansion of the **Implementation** folder:

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/gradle-library.png" class="no-border" >}}

    The **Implementation** folder is now a self contained Gradle project. It can be opened in any IDE that understands Gradle, and expanded with any code you like.

3. Build the **Implementation** folder and place the resulting *.jar* in the **userlib** folder. 

    If your implementation has additional dependencies, consider creating one `fat jar`  containing all dependent *.jar* files.     
{{% alert color="info" %}}
All modules have their *.jar* files in the **userlib** folder. In order to specify that this *.jar* is being used by your connector module, add an additional text file named `<jar-file-name>-<modulename>.requiredLib` .
{{% /alert %}}

4. After implementation, libraries are placed in the **userlib** folder, and are available in the Mendix application classpath so they can be used in a Java action in your connector module.

You can also extend your app with Gradle and add the **Implementation** project as a sub-module. In this case, your implementation project becomes part of your Mendix app. You can use it in Java actions without having to copy the *.jar* files to the **userlib** folder. 

{{% alert color="warning" %}}
You must have the *.jar* file placed in the  **userlib** folder before exporting the connector module. Otherwise, **Implementation** *.jar* files will not be packaged with the module.
{{% /alert %}}

A self-contained Gradle project can do unit testing and integration testing in the same way as you would do in any other Java project. See the [Testing](#testing) section of this document for best practices.

### Developing Connectors with Mendix

While Java is likely going to be a primary choice for building your connector modules, you can also use Mendix.

#### Making Microflows Available as Microflow or Workflow Activities

To ensure that end-users can reuse your Mendix build logic as easily as possible, you need to make microflows available as microflow or workflow activities. For details, see the [Triggering a Workflow via a Microflow](/refguide/perform-workflow-basic-functions/#trigger-microflow) section of *Performing Workflow Basic Functions*).

Ensure that a microflow is visible in the **Toolbox**, in the [Expose as microflow action](/refguide/java-actions/#expose-microflow-action) section of the microflow properties. You can do this by right-clicking in your microflow working area. Additionally, you can specify a caption for the action, a category for the **Toolbox**, and an icon. These will be used in the **Toolbox**, and also in the microflows, so they need to be easy to read for the end-user:

{{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/microflow-action.png" class="no-border" >}}

After you [expose as microflow action](/refguide/java-actions/#expose-microflow-action), this can be dragged and dropped inside another microflow.

If you create functionality that requires REST API integrations or complex data processing, make sure you have well-documented microflows connecting the public part of your module to the  private part. This way, the implementing developer will not have to understand the intricacies of the integration.

If you create functionality that requires REST API integrations or complex data processing, make sure you have well-documented microflows connecting the public part of your module to the  private part. This way, the implementing developer will not have to understand the intricacies of the integration.

Follow these guidelines for names and icons:

* The name of your category must match the name of the category which covers similar functionality.
* The name of the caption for your microflow or workflow exposed action does not overlap with others.
* The icon should give instant recognition to your function. Make sure that it is square and 16 by 16 pixels. This size is very small, so try not to add too many details.

#### Structuring the Usage of Your Connector

As stated in the [Setting Up the Main Connector Module](#main-setup) section, each connector build will have a  **UseMe** folder where you expose the logic that users of your connector module need when implementing your module. You can expose your microflows and Java actions in the toolbox. This means that users will be able to use your microflows and Java actions as microflow activities or workflow system tasks. You can also add page templates that are reusable.

When exposing your microflows and Java actions, you need to ensure that end users can find them easily. As an example, take a look at how the category, icon, and name are listed for platform-supported items. Add documentation to your input parameters and for the full Java action/microflow. This is part of the platform end users will be able to access easily while using your Java action/microflow activities.

For page templates, make sure to use a representative image, define a category that makes sense based on your module usage, and give a name that makes the template easily recognizable.

### Logging

When something goes wrong or the module needs to expose information that it is unable to expose via regular outputs, logging is a good option. 

Make sure your connector uses one log node for all log messages if they are logged from the Java code or the Mendix code. 

Ensure you utilize the right log levels:

* `CRITICAL` – The error harms the stability of the application.
* `ERROR` – The process cannot recover from the error.
* `WARNING` – Something went wrong, but the process can finish anyway.
* `INFO` – Useful information that should always appear in log files.
* `DEBUG` – The user needs to determine why the connector is not working as expected.
* `TRACE` – Determine why the implementation is not working as expected.

### Data Storage

Depending on your module's functionality, you might need to store data in the end user's application. The downside is that end users might delete your module to resolve issues such as **userlib** conflicts. 

We advise limiting the amount of data stored in persistable entities, and passing the data via your exposed logic to the end user's application logic instead. Then, they may decide to store it, or to only use it during a process. 

### Task Queue

Actions that may be batched or divided over multiple threads due to a large load or volume should be offloaded to the [task queue](/refguide/task-queue/). Using this method, you can simultaneously offload multiple tasks to multiple instances, and over multiple threads. You can also run these tasks in the background without having to wait for a user request. Make sure that the end user is aware that something is going on by setting a certain Task Status, or by implementing a progress log.

If you are using Studio Pro [8.18](/releasenotes/studio-pro/8.18/) or older, you can consider using the process queue. See the [Replacing Process Queue](/refguide9/task-queue/#process-queue) section of *Task Queue* for the difference between these functionalities.

### ConnectionDetails Entity

Use a **ConnectionDetails** entity for all general connection and security settings for the call. Use a **ConnectionDetails_Get** microflow in all your operations. This makes it easy to change all the settings in one location. Individual settings can be stored in a constant or in the database, and set during creation. Using constants is recommended because this avoids a dependency on the encryption module.  

{{% alert color="warning" %}} Using either the default value of a constant, or the project’s configuration setting is unsafe. Both these places are readable by others and visible in the version management. If you can share these settings with other developers, set them in the project configuration and leave the default values blank. This will limit the risk of accidentally exposing the settings when exporting the module.{{% /alert %}}

If you need to store sensitive information in the database, always use the [Encryption](/appstore/modules/encryption/) module to encrypt and store, and to retrieve and decrypt the information.

### Persistable Entities

If possible, avoid storing persistable data in persistable entities for your connector module. Developers will remove modules when troubleshooting Java compiling issues. If they add them back, data stored in persistable entities is lost. 

It is better to use non-persistable entities, and let the user decide how to store any data passed back from your connector in their own domain model.

### Toolbox Actions and Non-Persistable Entities (NPEs)

Toolbox actions need clear naming, consistent categorization, and documentation.

NPEs should be well-organized visually.

### Attributes

Consider doing the following for all entity attributes:

* Set all string values to unlimited.
* Check all date values. If the service only returns a date (no time), then set **localize** to **No**.
* Check all number values (decimal, integer, long), and remove the default value of 0.

{{% alert color="info" %}}
Databases like SAP HANA and Oracle do not support CLOBs for order by or group by clauses. Because of this, attributes that are used for sorting or group by operations should not be set to unlimited.
{{% /alert %}}

### Dependencies

Given that there is no dependency management between Mendix modules, try to minimize the number of dependencies your module has on other modules. If you do have to depend on other modules, make sure those modules are well-maintained by you or by Mendix themselves. Introducing another community-supported module as a dependent module might be too much of a risk for developers wanting to use your module.

Any dependencies your module has should be well documented, including the minimum required version to be used. It is also recommended to use the *.RequiredLib* files that Mendix uses for platform-supported modules, best handled with a build script like Gradle. To learn more about working with Gradle, see [Extending App Setup for Building Connectors with Java](#extend-app-java).

### IP Protection

Members of the [Mendix Partner Program](/appstore/partner-program/) and of the [Mendix Commercial Solution Partner Program](https://www.mendix.com/partners/become-a-partner/isv-program/) can protect their intellectual property (IP) or prevent end users from changing any logic shipped in a module using Mendix tooling. For details, see [Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/).

If protecting your IP or preventing end users from changing your logic is not a requirement, you can build a connector using available Mendix tools. If you want IP protection today, you need to implement the sensitive parts of the module in a hidden Java library. 

### Performance Considerations

Assuming your connector will be processing large sets of data, be aware of how you want to have that data flow through. 

#### Memory Usage

When importing large datasets into Mendix application, use streamable formats to avoid overconsumption of memory.

#### Pagination

If using NPEs, data retrieved by the connector is loaded into memory, so it is important to avoid requesting large amounts of data from an underlying system in a single call. Limiting or paginating the size of the retrieved data will lead to better performance for the Mendix app. Use server-side pagination for search results.

To provide a proper user experience, the amount of data loaded and rendered on the page needs to be limited. A typical page size of 25 objects or 100 objects is recommend, with options to load more and filter or search for optimized navigation through the dataset.

#### Import Mappings (Deep Structures)

Mendix does not support import mappings for recursive structure. Consider simplifying the connector domain model so that import mapping is possible and does not cause performance overhead when data is serialized into Mendix entities.

#### Caching

Use caching to reduce redundant database action for frequently retrieved objects.

#### Domain Model

A connector exposes the data of the underlying system using its domain model. Consider the following during the domain model design to ensure proper app performance:

* Do not expose the full data model complexity of the underlying source system in the connector domain model to facilitate easy data exchange. Try to only expose those pieces that are relevant for implementing your connector in the application of the implementing developer.
* Keep connector logic, such as Java actions code and microflows, as light as possible. An overcomplicated connector logic is more difficult to implement.
* Limit levels of inheritance and associations to no more than two, using persistable entities. This is not an issue for NPEs, so consider using them where possible.
* Minimize the use of reference set (many-to-many) associations. Mendix retrieves the IDs (per row for a list retrieve) on every query. As a result, many references, and especially reference sets, cause extra queries, and thus extra load on the database.
* Consider adding indices where relevant. Make sure to add them in a way that they make sense with the querying logic in your connector and/or use cases for implementing apps.
* Keep your persistable entities from growing too large. You may do this by adding a scheduled cleanup event, which you should make configurable via constants. An example of this is removing {x} amount of objects at a time.
* Add a flexible but secure set of entity access rules based on the module roles that you add. Keep in mind the use cases for your connector. Mendix recommends you avoid any persistable data. 

## Configuration

You should set up your configuration in such a way as to ensure that your connector can be used in different settings without changes to the module itself. This means that upon deployment or after deployment your connector can be configured to connect to the relevant services. 

Using constants is the way to deal with configuration that aligns with the [Twelve-Factor Architecture](https://www.mendix.com/evaluation-guide/enterprise-capabilities/twelve-factor-architecture/) cloud-native approach.

### Simple Configuration

When you are looking for a simple configuration, such as a URL, username, or password, you can use [constants](/refguide/constants/). Constants are ideal for simple flat configurations.

#### Simple Configuration with a Free App Environment

When using constants in combination with a Free App, you can use the settings profile to allow for different configurations. Follow these steps to do that:

1. Create a constant.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/simple-config-constant.png">}}

2. Set the value of the constant to the value you want to use in your free cloud node.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/simple-config-value.png" >}}

3. Open your application **Settings**. 

4. Click **Duplicate** or **New** to create a new configuration for your local usage.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/simple-config-settings.png">}}

5. In your configuration, open the **Constants** tab and click **New**.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/simple-config-new-constants.png">}}

6. Look up and select your constant.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/simple-config-select-constant.png">}}

7. Change the configuration value of your constant to the value you want to use in your local environment.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/simple-config-change-value.png">}}

8. Save the configuration and publish the application to your free node. When you run locally, Studio Pro will now use the **Active** configuration, while the free cloud node will keep using the value you specified in the **App Explorer**.

### Complex Configuration

You might need a more sophisticated configuration to connect to external systems, either because you will need to have dozens of constants or you want to be able to connect to multiple endpoints without having to replicate your constants each time. You will have to maintain these through configuration tables.

#### Disadvantages of Complex Configuration

The following are the disadvantages of complex configuration:

* Configuration in the database or the codebase makes it difficult to deploy your connector/app to any new environment.
* Manual configuration could cause more mistakes.
* Restoring a database to a different environment when the configuration is stored in the database could cause unwanted behavior.
* One big risk of using the database to store configuration is test data going out to production users.

#### Advantages of Complex Configuration

The following are the advantages of complex configuration: 

* It provides more complexity than constants.
* You can easily perform runtime changes on the configuration.
* You can add wizards/helper flows to guide the user with the configuration.

#### Setting Up Complex Configuration

Follow these steps to set up complex configuration: 

1. Set up a **Configuration** entity.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/complex-config-entity.png">}}

2. Create the microflow.  
   Have a single microflow called **DS_GetOrCreateSettings**. It will retrieve your settings from the database, and create it with appropriate default values if it does not exist.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/complex-config-create-microflow.png">}}

3. Set up security on the entity and the microflow.

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/complex-config-security.png" >}}

    {{< figure src="/attachments/appstore/create-content/create-connectors/connector-guide-best-practices/complex-config-security-2.png" >}}

4. Set up the administrator page.  
   Have an administration page to manage the configuration. For maximum reusability, have all configuration settings available in a single snippet so your consumer can combine all admin sections from all components into a single area in their application.

5. Encrypt the password and other sensitive information.

##### Further Considerations

When possible, create a microflow to set up the default or starting configuration for your consumers. 

Consider adding logic so the configuration can easily be set up from your codebase.

If possible, add an export/import option for the configuration to safely move the configuration between environments. This could be achieved via a JSON export/import of the configuration data.

## Security

Security for components should be set up in a generic and granular way. When choosing your module roles, think about the most granular configuration which also allows end users maximum flexibility. 

### Module Security

The following recommendations apply to module security:

* Module roles should always be set up, even if there is limited or no UI to allow the user to extend your component without having to customize. 
* Use clear granular roles, in which the **Documentation** field clearly explains the purpose of each role.
* Keep security as simple as possible. Find a balance between granularity and simplicity. 
* Always keep configuration and data access separate.  
In most organizations, administrators should not have access to the data. If your organization is different, you can always combine module roles, but consumers cannot split them.
* Make sure to [implement best practices for app security](/howto/security/best-practices-security/).

See [Module Security](/refguide/module-security/) for more details.

### Entity Security

Think about how to apply access rules and read or write permissions to your domain model. For example, if you do not give any rights to objects, developers have to use a data transformation layer and create their own objects to build their pages, or enrich their own objects with results coming from the connector.

### Passwords and Other API keys

If you store a password or API keys for your endpoint, always encrypt them using the [Encryption](/appstore/modules/encryption/) module.

### Typical Security Schemes

There are several security schemes you might encounter when building a connector to an external application.

#### Client Credentials

Security via client credentials is a very basic security method. Given that you use the login name and password, the only protection when sending it to the service you are integrating with is an encrypted connection over SSL. If that is unavailable, make sure to never use this type of encryption. 

#### API Tokens

API tokens help when securing an API. However, an API payload sent in plain text could still be intercepted. Only use API tokens when you have at least an SSL connection. 

#### OAuth

OAuth comes in two types:

* An Authorization Code flow – The user logs in to the service through OAuth authentication, thus giving access to their data on a per-user basis. 
* Client Credentials – Provide a public and private key with access tokens for server-to-server communication. 

OAuth is a secure schema because the secret key is never exchanged during API requests. As long as you store the secret key safely in your own app, and you use HTTPS, your credentials for the API provider cannot be hijacked.

#### SAML

The [SAML](/appstore/modules/saml/) module, available on the Mendix Marketplace, can be used as a replacement or extension of your supported authentication methods.

## Testing {#testing}

You can use the [Unit Testing](/appstore/modules/unit-testing/) module to test Mendix by calling microflows for whole pieces of functionality. Use unit tests for smaller, more targeted tests at the method-level of your Java code.

As with any automated testing, it is a great supplement for capturing known, expected behavior. For capturing unknown behavior, Mendix recommends risk-based exploratory testing.

### Testing Microflows

Use the Mendix app where you built the module to test your module. The Unit Testing module can help call microflows and actions to aid in testing. See [Testing Microflows with the Unit Testing Module](/refguide/testing-microflows-with-unit-testing-module/) for more information.

### Java Unit Tests {#unit-testing}

Java unit tests are a good way to automate the testing of your Java code. This is helpful when you extend your Mendix app or connector with Java actions.

#### Java Unit Testing Tools

The following tools are part of the testing process of custom Java code in your Mendix app:

* [JUnit](https://junit.org/junit5/) — This allows you to run Java unit tests easily.
* [Mockito](https://site.mockito.org/) — This allows you to mock Java classes. You can fake a class during a test so that it thinks that it deals with a real class, while it is actually just behaving like one.
* [JaCoCo](https://www.jacoco.org/jacoco/trunk/index.html) (Java Code Coverage) — This helps you check how well your code is covered by tests.
* [Gradle](https://gradle.org/) — This pulls everything together to manage Java dependencies, and provides a way to work with JUnit and Mockito.  
   Gradle can handle managing Java dependencies and running [JUnit](https://junit.org) tests. Read about setting up Gradle in [Extending App Setup for Building Connectors with Java](#extend-app-java). If you have a pipeline, Mendix recommends running your tests as part of it. 

#### Using the Java Unit Test Reference

Mendix apps need `Core` classes, and the **Class Core** [Runtime API](/apidocs-mxsdk/apidocs/) allows you to do things that you usually do in a microflow, like committing an object. `CoreProxy` serves as a mockable layer between your Java logic and the **Core** API. 

Our [Java unit test reference](https://Github.com/mendixlabs/javaunittestreference) is available to help you through this process. `MendixUnitTestBase.java` is extendable and reusable for your own purposes. When extended, it enables Mockito on your test classes and mocks `Core` API behavior.  This does the following:

* Checks that your code sends a certain log message to a log node.
* Verifies that your code calls a microflow with correct parameters.
* Makes constants available to your Java code.

{{% alert color="info" %}}
The Java unit test reference has been tested up to and including Studio Pro [9.12](/releasenotes/studio-pro/9.12/).
{{% /alert %}}

#### Writing Java Code

When writing Java code, try to use as much Mendix-independent logic as possible. This helps with testability, so you do not have to mock Mendix `Core` to unit test the Java code. 

If you code the bulk of the behavior in generic Java classes that can run without Mendix, development and testing becomes a lot simpler and faster. There is no need to run your Mendix app to test, and you can also rely on Java JUnit tests for testing.

For another example of a Java unit test, see the [Slack Connector test](https://Github.com/ako/SlackConnector/blob/master/javasource/testslackconnector/tests/TestSlackConnector.java).

## REST Error Handling

When calling a REST service, you can run into an error. This can be one of two types:

1. Error with a response
2. Error without a response
   
If there is no response, the default error handling is enough. This will typically occur when the endpoint is down or when you get a timeout.

If there is a response, the error message will contain the error code and the reason, but not the message. For that reason, add an additional log message with the response, then rethrow the error. The log message should include details about the request that will help the developer.

Any input, such as objects or path parameters, might trigger an error event in the REST call. It is not necessary to check them for empty values in the connector itself, but do make sure to check them before using [urlEncode](/refguide/string-function-calls/#urlEncode).

In some situations, the error response from the service has its own structure that you want to leverage. For example, when there is a bad request, you might want to pass that message back to the user instead of logging it. However, you cannot return two different objects from one microflow. In those situations, combine the response from the error message with the regular message. In all other situations, the error is unexpected, and you can return the error.

## Releasing and Versioning

This section addresses best practices for releasing your connector and using a versioning system. For the basic instructions for releasing, versioning, and distributing your connector, see the [Exporting the Connector](/appstore/creating-content/connector-guide-build/#export) and [Distributing the Connector](/appstore/creating-content/connector-guide-build/#distribute) sections of *Creating Connectors*. 

### Releasing

Ensure a new release includes the following:

* An easy upgrade path
* The version number of the package

### Versioning {#versioning}

The [Updating Existing Marketplace Content](/appstore/submit-content/#updating) section of *Uploading to the Marketplace* provides explanations of the recommended versioning system. The following points go into more detail on the versioning number system:

* Major version — This is the first digit. This number goes up when you release major breaking changes, or at the very least a major new feature, as part of your connector. Determining what is major is up to you. You can deploy minor/patch versions for older major versions when you have bugs that need resolving.

* Minor version — This is the second digit. This number goes up when you release minor changes that do not break backwards compatibility, and that fix some bugs, or add small new features. Depending on your development cycle, this could be intermediate releases in smaller batches.

* Patch version — This is the third digit. This number goes up when you really have to patch a bug for a specific version and it cannot be released as part of your next major or minor version. While it is allowed to add new patch versions to older major versions, you cannot add patch versions to older minor versions within the Marketplace.

## Licensing

Follow your Java Libraries and default to Mendix EULA. Also, Mendix recommends that you do not use copy-left licenses for commercially available connectors.
