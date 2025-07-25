---
title: "Preferences"
url: /refguide/preferences-dialog/
weight: 50
---

## Introduction

The **Preferences** option in the menu opens a dialog box where you can set user-specific preferences which apply to the whole of Studio Pro:

{{< figure src="/attachments/refguide/modeling/menus/edit-menu/preferences-dialog/preferences.png" alt="Preferences" width="600" class="no-border" >}}

## General Tab {#general}

### Enable Autosave {#autosave}

When enabled, this option automatically saves your changes on various actions, such as running and deploying your app, executing version control operations (for example, committing, merging, or reverting), closing files or apps, exporting modules, or exiting Studio Pro.

Additionally, you have the option to enable autosave for any of these actions when you have unsaved changes. In such cases a dialogue box is opened, where you can choose to save or discard your changes. You can speed this process up by checking the **Do not ask me again** checkbox, which prevents further prompts and enables the autosave functionality.

### Interface {#interface}

#### Keep Active Document Selected in the App Explorer

When enabled, **App Explorer** automatically highlights the document that is currently active. When this property is disabled, the selection in **App Explorer** remains unchanged when the active document changes.

#### Show Styling For

The **Show styling for** setting configures which styling files are shown in the App Explorer. 

You can select one of the following options:

* **App Only** – This option covers the most common case and lets you customize your app-specific styling. By choosing this, you can see all files from the **theme** folder in App Explorer > **App** > **Styling**.
* **App and UI resources modules** (*Default*) – This option targets developers who would like to create or edit UI resources modules, for example, to build or extend your design system/theme. This option also adds a **Styling** folder to modules that are marked as UI resources, containing the files from the corresponding **themesource** folder.
* **App and all modules** – Additionally to the option above, this one adds a **Styling** folder to all modules, regardless of whether they are marked as UI resources or not.

For more information about the styling editor, see the [Using the Styling Editor](/howto/front-end/customize-styling-new/#styling-editor) section in *How to Customize Styling*.

### Error List

#### Auto-Check Delay

The **Auto-check delay** is the number of milliseconds that Studio Pro waits after a change, before starting the consistency checks. Increase this setting for large apps if Studio Pro responses slowly after changes.

#### Highlight Shown Errors and Warnings in the Editor

When **Highlight shown errors and warnings in the editor** is checked, the errors and warnings which are currently shown in the Error List (as determined by the **Errors** and **Warnings** toggle buttons and the suppression rules) will be highlighted on the affected elements in the editor.

### Disk Location

#### Default App Directory {#default-directory}

This is the directory where new apps are stored. It is used in the [App Settings](/refguide/new-app/#app-settings) dialog box when you create a new app. If a different directory is selected in the **App Settings** dialog box, that directory is stored as the new default app directory.

## Deployment Tab {#deployment}

In the **Deployment** tab you can configure various settings related to application deployment.

{{< figure src="/attachments/refguide/modeling/menus/edit-menu/preferences-dialog/preferences-deployment-tab.png" alt="Preferences" width="600" class="no-border" >}}

### JDK {#jdk}

Here the directories of the Java Development Kit (JDK) for each supported Java version can be selected.
These JDKs are used when you locally deploy an application.
Usually, the correct directories are located automatically.

A JDK is necessary to run Mendix applications, because the Mendix Runtime runs on the Java Virtual Machine (JVM).

### Deployment

#### Enable Run Optimizations

Enable this setting to increase the speed at which a running application is updated after changes have been made in Studio Pro. If only pages, layouts, or snippets have been changed, then an entire restart of the application is skipped to decrease deployment time. Also, no Java compilation is performed when no relevant files have changed.

#### Optimize Bundle {#bundle}

This setting is visible and effective when the React client is enabled. By default, it is already enabled to reduce the size of the generated app bundle and speeds up the bundling process by disabling source maps generation. Source maps are used for debugging the bundled code during development. If you want to generate source maps for debugging pluggable widgets, you can disable this setting.

### Build

#### Generate Full Debug Info {#debug-info}

Enabling this option includes debug information about local variables in the Java actions when building them, which aids debugging from Eclipse. Without this option, only line numbers and source information will be included.

#### Gradle Synchronization {#gradle-synchronization}

This option controls the synchronization of managed dependencies. For more information, see the [Offline Usage](/refguide/managed-dependencies/#disabling-synchronization) section in *Managed Dependencies*.

#### Gradle Directory

A directory where Gradle is located on the computer that you use to deploy the application. The correct directory is usually located automatically.

#### Extra Arguments

This option can be used to add extra arguments to the Gradle process.

#### Use Custom Repositories

Enabling this option allows you to specify which repositories should be used by Gradle. For more information, see the [Custom Repositories](/refguide/managed-dependencies/#custom-repos) section in *Managed Dependencies*.

#### Repositories

This option becomes available if **Use custom repositiories** is set to **Yes**. 

You can specify which repositories to use for Gradle. The content of this field should be specified using Groovy syntax and is what is inside the `repositories { }` section in a Gradle build file. By default, this field contains: `mavenCentral()` and `gradlePluginPortal()`. 

## Maia Tab

### Generative AI

The **Generative AI** section contains the following settings:

* **Enable Start with Maia** – When enabled, you can use the **Start with Maia** starting point in Studio Pro to help you create an app. For more information on how to use this feature, see [Start with Maia](/refguide/start-with-maia/). It is enabled by default.
* **Enable Maia Explain** – When enabled, you can use it to explain a microflow or a nanoflow in the logic editors. For more information on how to use this feature, see [Maia Explain](/refguide/maia-explain/). It is enabled by default.
* **Enable Maia for Domain Model** – when enabled, you can use it to generate a new domain model or to explain and improve an existing one. For more information on how to use this feature, see [Maia for Domain Model](/refguide/maia-for-domain-model/). It is enabled by default.
* **Enable Maia for OQL**– when enabled, you can use it to generate and manage OQL (Object Query Language) queries through an intuitive interface. For more information on how to use this feature, see [Maia for OQL](/refguide/maia-for-oql/). It is disabled by default.
* **Enable Maia for Pages** – when enabled, you can use it to generate a new page or to explain and improve an existing one. For more information on how to use this feature, see [Maia for Pages](/refguide/maia-for-pages/). It is disabled by default.
* **Enable Maia for Workflows** – when enabled, you can use it to generate workflows by adding and configuring activities. For more information on how to use this feature, see [Maia for Workflows](/refguide/maia-for-workflows/). It is disabled by default.
* **Enable Translation Generator** – when enabled, you can use it to translate your model into different languages through the **Batch translate** option under the **Language** menu, or to translate [system texts](/refguide/system-texts/) for you. For more information, see [Translation Generator](/refguide/translation-generator/). It is disabled by default.

### In-Editor Recommender

The **In-Editor Recommender** section contains the following settings:

* **Enable for microflow, nanoflow, and rule editors** – this allows you to enable and disable [Logic Recommender](/refguide/logic-recommender/) in the microflow, nanoflow, and rule editors. It is enabled by default. 
* **Enable for workflow editor** – this allows you to enable and disable [Workflow Recommender](/refguide/workflow-recommender/) in the workflow editor. It is enabled by default.
* **Enable for page editor** - this allows you to enable and disable the [UI Recommendor](/refguide/ui-recommender/). It is enabled by default.

### Best Practice Recommender

The **Best Practice Recommender** section contains the following settings:

* **Show recommendation in editors** – when enabled, [Best Practice Recommender](/refguide/best-practice-recommender/) highlights elements that contain anti-patterns in visual editors. It is enabled by default.

* **Automatically run an inspection after opening an app** – when enabled, [Best Practice Recommender](/refguide/best-practice-recommender/) automatically runs an inspection when an app is opened. If the app contains errors, the inspection will not run. It is enabled by default.

## Model Tab

### When Prompted by a Widget to Automatically Fill Its Contents, Select 'Yes' by Default

This setting defines the default response to the question whether to automatically fill the contents of a data widget. This question is asked, for example, when dragging an Entity onto a Data View widget. When enabled, the pre-selected answer is **yes**; otherwise, it is **no**.

## Version Control Tab {#version-control}

### General

#### Executable

This is the path and name of the program that is used to view the details of file changes in the [commit dialog box](/refguide/commit-dialog/).

For example, you can see how to set up file comparison for Visual Studio Code below: 

```text
{path to VS Code}/Code.exe --wait --diff "{0}" "{1}"
```

#### Argument Pattern

This is the pattern from which the arguments are derived that are passed to the file comparison program. The following two placeholders can be used in this pattern:

* `{0}` – this is replaced with the name of the original file before the arguments are passed to the file comparison program
* `{1}` – this is replaced with the name of the changed file before the arguments are passed to the file comparison program

#### Solution Warning

Select **Show warning on updating marketplace modules** to display a warning message when updating a Marketplace module. 

### Git{#git}

#### Git Location {#git-location}

This setting allows users to specify a path for Git installation in their system. By default, Studio Pro automatically detects installed Git, ensuring a smooth setup for most users. However, there may be cases where manually configuring the path is beneficial, such as when multiple versions of Git are installed or when the default detection does not align with specific project requirements.

#### Enable Private Version Control with Git {#enable-with-Git}

Select this option when you want to work on an app that is not stored in [Mendix Team Server](/developerportal/general/team-server/), but in a private Git server, which you have access to. This allows you to specify the location of the app on the Git server when opening, downloading, or uploading the app. With this setting you also need to specify [name and email](#name) that will be used to identify your commits with Git.

#### Name and Email {#name}

Specify your name and email for Git to use in commit messages and make them more informative. These values are stored in the global Git configuration, which can also be used for other applications.

{{% alert color="warning" %}}

**Name** and **Email** settings are not used for authenticating with the version control server and can be changed freely by the user.

The commit is made on the user's workstation, so no authentication is required at that stage. However, when pushing changes to the version control server, it is expected that the user is fully aware of the content they are pushing. Pushes to the server are only allowed for authorized users.

{{% /alert %}}

{{% alert color="info" %}}

Changing these values for another application will also affect the commits made from Studio Pro. If you notice a suspicious value in the commit history, it is likely a private email address set through another tool in the global Git configuration.

{{% /alert %}}

#### Enable Current Windows User Authentication {#enable-windows-authentication}

When this option is selected, the application automatically uses credentials of the currently logged-in Windows user to authenticate and connect to the on-premises Git server. This feature streamlines the authentication process by eliminating the need for users to manually enter their credentials, enhancing both security and user convenience. By leveraging Windows authentication, organizations can ensure that access to the Git server is seamlessly integrated with their existing IT infrastructure, providing a smooth and efficient workflow for development teams.

#### Clone {#clone}

Select a [Clone type](/refguide/clone-type/) to use for future clone operations, such as downloading an app or checking out another branch of an app you already downloaded. Changing this setting does not affect apps that you have already downloaded.

#### Combine Local and Remote Changes

When [combining changes](/refguide/merge-algorithm/), for example when doing a Git pull, the user can choose between **Rebase** and **Merge** as the default action. This preference can be overridden for each merge which contains conflicts.

#### Git Version Warning

Select **Do not show warning about Git version** to suppress a warning shown when Studio Pro starts.

### Background Tasks

#### Enable Automatic Fetching from a Remote Repository {#enable-auto-fetch}

Select **Enable automatic fetching from a remote repository** to enable the [Automatic fetch mechanism](/refguide/auto-fetch/).

#### Fetch Interval, Minutes

The number of minutes to wait after a fetch has started before performing another fetch. This must be between 1 and 120 minutes.

#### Enable Automatic Repository Optimization {#optimization}

Select **Enable automatic repository optimization** to run Git repository optimization automatically on a regular basis. This helps you maintain the storage structure providing benefits from both performance and repository size perspectives. 

#### Number of Commits

This option is available when the [Enable automatic repository optimization](#optimization) setting is on. Studio Pro keeps track of the number of commits made in the local repository. You can manually specify the minimum number of commits needed to start the background optimization. For more information, see [Git Storage Optimization](/refguide/git-storage-optimization-dialog/).

## Work Environment Tab

### Studio Pro Theme {#studio-pro-theme}

This option allows user to choose between Studio Pro themes: **Auto (System theme)**, **Light**, or **Dark**. The default is **Auto (System theme)**, which detects the theme set in the operating system and uses an appropriate theme for Studio Pro (**Light** or **Dark**). Changing this option requires a restart of Studio Pro to take effect.

### Default Page Editor {#default-page-editor}

This option sets the default page editor mode that your page opens in: **Structure mode** (the default) or **Design mode**. For more information on page editor modes, see the [Page Editor Modes](/refguide/page/#page-editor-modes) section in *Page*.

### Language {#language}

This feature is currently in beta.

This option allows you to change the user interface language you work in while using Studio Pro. At this time, English, Japanese, Chinese, Korean, and Brazilian Portuguese are supported. You must restart Studio Pro in order to use this feature.

### Editor Tabs Closing Policy {#closing-policy}

It is recommended to limit the number of open tabs (open documents) to improve the performance of Studio Pro. This option is enabled by default and set to a limit of 15 tabs. However, you can disable this behavior completely or change the limit value.

{{% alert color="info" %}}
The closing policy is not applied to tabs with unsaved changes.
{{% /alert %}}

### Navigation

This setting allows you to move forward and backward through your editing history to show the documents you have recently worked on. This feature is enabled by default. 

## Advanced Tab

### Rendering {#rendering}

Hardware and driver issues may cause performance problems when running Studio Pro. These issues can appear in form of dialog boxes opening and closing much more slowly than expected, and general slowness of the UI. If case the hardware problems cannot be solved, it is possible to mitigate these issues by turning the **Enable software rendering mode** setting on. This option is available for Native UI and Web Content. For Web Content, you can select **Auto**, which enables software rendering mode automatically when running on Parallels or another virtual machine.

Enabling this setting requires a restart of Studio Pro to take effect. Running the application with this setting on may increase the CPU usage.

### Proxy Server

Sometimes the computer running Studio Pro cannot access the internet directly, but has to connect to a proxy server that requires authentication. If this is the case, then these settings can be used to specify the user name and password to connect to the proxy server.

### Usage Data {#usage-data}

When the **Send Studio Pro usage data to Mendix** setting is enabled, Studio Pro sends usage data to Mendix that allows Mendix to identify issues and improve the user experience. The usage data does not contain sensitive information. It is possible to disable this feature, but it may affect the behavior of some features, prevent Mendix from identifying issues reported by the user, or affect tracking issues that are not reported yet. This setting is machine-specific and changing this feature does not affect any existing installed version.

### Visual Builder for XPath Constraints{#visual-builder}

**Visual Builder** is the default way to construct XPath constraints, but you can change the default by clearing **Enable the XPath Builder as the default XPath constraint editor**.

### Feedback Survey

When the **Show feedback survey periodically** setting is enabled, Studio Pro will periodically display a feedback survey to gather input on your experience. This helps Mendix improve the product based on user feedback. For more information, see [Feedback Survey](/refguide/feedback-survey/).

### Antivirus Exclusion

When the **Do not show antivirus exclusion notifications** setting is enabled, Studio Pro will not display notifications related to antivirus exclusions. For more information, see [Antivirus Exclusion](/refguide/antivirus-exclusion/).

## New Features Tab {#new-features}

### App Explorer

Select this option to use the modernized version of the App Explorer. You must restart Studio Pro to use this feature.

### Errors Pane

Select this option to use the modernized version of the Errors Pane. You must restart Studio Pro to use this feature.

### Expression Editor

This setting is enabled by default. The editor allows the user to write rich text statements and get instant feedback on their validity. In Studio Pro, it is often used to write an expression for a decision or to write an XPath expression for data filtering.

### GraphQL {#graphql}

You can publish data as a GraphQL service. When you enable this feature, you can indicate that a published OData service [also supports GraphQL](/refguide/published-odata-services/#supports-graphql).

### Mapping Editor

This setting allows you to set the beta version as the default editor.

### Online Synchronization Mode

This setting, currently in beta, allows entities to be used in offline navigation profiles without requiring data synchronization. This enables app developers to use entity data on pages without synchronizing the data of those entities to the offline database. This mode requires an available connection to the server. For more details, see [Online Synchronization Mode](/refguide/mobile/building-efficient-mobile-apps/offlinefirst-data/online-sync-mode/).

### System Texts Editor

The web version of the system texts editor is currently an experimental feature. Enable this option to use the web version of the editor. 

If Translation Generator is also enabled, you can use it in this editor as well. For more information, see the [Generating Translation for System Texts](/refguide/translation-generator/#translate-system-text) section in *Translation Generator*.

### Toolbox

Enable this setting to use the modernized toolbox. This requires restart of Studio Pro.

## Read More

* [Upload to Version Control Server](/refguide/upload-to-version-control-dialog/)
