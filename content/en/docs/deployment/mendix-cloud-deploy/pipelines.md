---
title: "Pipelines"
url: /developerportal/deploy/pipelines/
weight: 8
description: "Describes how to design, implement, and review pipelines using the Pipelines feature in the Mendix Portal"
---

## Introduction

From the **Pipelines** page, once you have designed and activated a pipeline, you can use it for automated, zero-click builds, testing and deploying your apps. Each pipeline runs automatically according to the trigger conditions defined in your [Start Pipeline](#pipeline-steps) step.

To access the **Pipelines** page, open your app in [Apps](https://sprintr.home.mendix.com/). Then select **Pipelines** in the navigation pane. (To view this page, you must have a [role](/developerportal/general/team/) with cloud access.)

The **Pipelines** page has four tabs: **Runs**, **Designs**, **Variables**, and **Settings**. You can see all four tabs as soon as your app has its first pipeline saved.

{{% alert color="info" %}}
If your app does not have any pipelines yet, skip to the [Designing a New Pipeline](#design-pipeline) section below.

You need to configure your user settings so that you can run a pipeline for the first time. For more information, skip to the [Configuring User Settings](#configure-settings) section.
{{% /alert %}}

## The Runs Tab{#runs-tab}

For each run, you can view the following information:

* **Status** – Whether the run succeeded, failed, or is in progress
* **Run ID** – The unique identifier of the run
* **Pipeline ID** – The unique identifier of the pipeline design used for the run (column hidden by default)
* **Branch** – The branch, if **Teamserver push (Git)** was used as the pipeline's trigger
* **Pipeline Name** – The name of the pipeline design used for the run
* **Triggered By** – The user who triggered the pipeline run
* **Triggered** – The number of days since the pipeline run
* **Duration** – The time the pipeline took to complete the run
* **Trigger Type** – The trigger type for the run

Use the Column Selector ({{% icon name="view" %}}) at the top of the **Runs** table to customize which of these columns appear in the table.

The search and filter options allow you to review specific run types. You can do the following:

* Search by keywords, such as run ID, branch, or pipeline name
* Filter by trigger: All triggers, recurring schedule, Teamserver push (Git), or manual
* Filter by status: All statuses, pending, succeeded, in progress, or failed

### Run Results

To see the results of a particular run, click **Details** ({{% icon name="paper-clipboard" %}}).

{{< figure src="/attachments/deployment/mendix-cloud-deploy/pipelines/run-details.png" alt="" >}}

This page provides an overview of the run details. You can also expand each executed step to see more information about it; this is especially useful for debugging when a pipeline run fails. When you expand a step, you can also see a **View Parameters** button, which you can use to view the input and output parameters as well as the values of the step.

At the top of the page, there are several cards that display important information, such as whether the run succeeded or failed. If it failed, it includes information about the error that caused the run failure. There are two types of errors: step-level errors and system-level errors.

#### Step-Level Errors

Step-level errors occur when a step in the pipeline fails, either because the step itself failed or because the user had insufficient rights to run a step.

For example, if you do not have the [permissions](/developerportal/deploy/node-permissions/) to deploy to the target environment, then the pipeline will fail at the Deploy step (or earlier, if there is also an earlier step where your permissions are insufficient).

If a step fails, that step is flagged with an error icon ({{% icon name="alert-triangle-filled" color="red" %}}). The step's logs indicate the cause of failure.

#### System-Level Errors

System-level errors occur if [user settings](#configure-settings) have not been configured or if an internal Mendix component is down.

If a system-level error occurs, the card in the upper-left corner of the **Details** page identifies the error that caused the run to fail. Click **See details** on the card to view more information about the error.
 
## The Designs Tab{#designs-tab}

On the **Designs** tab, you can see all existing pipeline designs.

For each design, you can view the following information:

* **Pipeline Name** – The name of the pipeline design
* **Trigger Type** – The trigger type for the design
* **Branch** – The branch, if **Teamserver push (Git)** is set as the design's trigger
* **Last modified By** – The user who last modified the design
* **Last modified** – The date and time the design was last modified
* **Status** – Whether the pipeline is active or inactive
* **Design ID** – The unique identifier of the pipeline design

Click **Copy ID** ({{% icon name="copy" %}}) on a pipeline design to copy the design ID.

Click **More Options** ({{% icon name="three-dots-menu-horizontal-filled" %}}) > **Edit** on a pipeline design to go to its **Details** page. From there, you can view and edit the pipeline details, as described in the [Editing a Pipeline](#edit-pipeline) section below.

Click **More Options** ({{% icon name="three-dots-menu-horizontal-filled" %}}) > **Delete** to delete a pipeline design. You cannot delete a pipeline design if it is used in any currently running pipelines. Instead, you must wait for the run to finish before you delete the design.

{{% alert color="warning" %}}
If you delete a pipeline design in the **Designs** tab, any run history associated with that pipeline design is deleted from the **Runs** tab.
{{% /alert %}}

### Designing a New Pipeline{#design-pipeline}

To design a new pipeline, click **Design a Pipeline** from any tab. That launches the **Design a Pipeline** dialog box. You can choose to start from a **Template Pipeline** or an **Empty Pipeline**.

{{% alert color="info" %}}
If you start from the template, you can still add, remove, and configure its steps to match your needs; it is just intended to help you get started quickly. The template pipeline is pre-populated with the following basic steps:

1. Start Pipeline
2. Checkout
3. Build
4. Publish
5. Deploy
{{% /alert %}}

Give your pipeline a name. You can use up to 40 alphanumeric characters in the name. Then click **Next** to go to your new pipeline design's **Details** page.

### Editing a Pipeline Design{#edit-pipeline}

From your pipeline design's **Details** page, you can add, remove, and configure the steps in your pipeline. You can also click **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) next to the pipeline name to edit the name.

{{% alert color="info" %}}
For pipelines that are currently running, the page shows the current state of the steps (finished, running, or not started).

Active pipelines cannot be edited; if you want to edit an existing pipeline, make sure it is deactivated.
{{% /alert %}}

#### Pipeline Steps{#pipeline-steps}

To add a step, launch the **Pipeline Steps** dialog box by clicking **Add Step** ({{% icon name="add" %}}). Expand each step to configure it, delete it, or view its outputs. You can expand or collapse any step in your pipeline by clicking the step's name.

Your pipeline can include the following steps:

##### Start Pipeline

This is a mandatory step for each pipeline; you cannot delete this step. This step defines the conditions that will automatically trigger the pipeline to run. To configure this step, define the conditions on which the pipeline should start. You can set the pipeline to run in response to the **Teamserver push** trigger, the **Recurring schedule** trigger, or the **Manual** trigger:

* Teamserver push (Git) – The pipeline runs when a new push is made to Teamserver (Git) for the specified branch. For details on specifying the branch in the **Branch Expression** field, see [Branch Expression](#branch-expression), below.
* Recurring schedule – The pipeline runs on a recurring weekly schedule, on the days and times you specify. This works for both Git and SVN repositories. Times are set in UTC.
* Manual – The pipeline runs when you click **Run Manual Pipeline** from either the **Runs** or **Designs** tabs of the Pipelines page. 

{{% alert color="info" %}}
The Teamserver push (Git) command requires a Git repository. Customers using an SVN repository for their Mendix app cannot use this trigger.

Ensure that you only create one pipeline per app with the Teamserver push (Git) trigger and the same [branch expression](#branch-expression). If you create multiple pipelines with the Teamserver push (Git) trigger and the same expression, only the pipeline that you created first is executed.
{{% /alert %}}

##### Checkout

Check out a branch. To configure this step, use the drop-down menu to select the branch to check out. You can select either the main branch or one of your most recently used branches.

##### Build

Build a deployment package based on the latest major, minor, or patch version of the branch you checked out. The highest version is incremented based on the increment settings specified in this step.

##### Maia Best Practice Recommender{##recommender}

Evaluate results of the [Maia Best Practice Recommender](/refguide/best-practice-recommender/) within your pipeline. You can configure this step to fail the pipeline if errors, warnings, deprecations, and/or recommendations are detected.

##### Unit Testing{#unit-testing}

[Unit Testing](/appstore/modules/unit-testing/) module can perform regression testing on an environment in which a new deployment package has been deployed. This step executes the Unit Testing module in a running environment. If any unit test fails, the pipeline will be marked as failed, with the run details and output parameters showing the failure count and relevant information. Ensure to add the prerequisites below before you add the Unit Testing pipeline step:

* Import the [Unit Testing](https://marketplace.mendix.com/link/component/390) module into your Mendix application from the Marketplace.
* The environment in which Unit Testing needs to happen should be in a running state.

Since a remote API password is required to trigger Unit Tests and it is not advisable to have sensitive credential information in the pipeline definition, use variables within pipelines. These variables can then be easily referenced in the pipeline design. For more information, see the [Running Unit Tests Through the Remote API](/appstore/modules/unit-testing/#using-the-remote-api) section of *Unit Testing*.

Using **Timeout (in seconds)** field, users can restrict the execution time of unit tests. If the tests exceed the predefined duration or timeout, the test step fails. It helps to avoid unnecessary waiting if tests are stuck in a loop.

##### Publish

Publish the newly built deployment package to a repository.

##### Start Environment

Start a selected environment.

##### Stop Environment 

Stop a selected environment.

##### Create Backup 

Create and store a backup of an existing environment before deploying a new deployment package.

##### Deploy 

Deploy to a selected environment. In this step's configuration, there is a **Use default values for new constants** toggle that you can use to fetch the default values of new constants and scheduled events from Studio Pro and apply them to the environment. (To adjust an environment-specific configuration, see the [Environments](/developerportal/deploy/environments/) page.)

##### Promote Package

Promote a deployment package from a source environment to a target environment. To configure this step, specify a source environment and a target environment.

#### Branch Expression{#branch-expression}

If you select **Teamserver push (Git)** as the trigger in the Start Pipeline step, you need to specify the relevant branch (or branches) in the **Branch expression** field.

If you want the pipeline to run when there is a push to one specific branch, you can type the name of that branch. Also, you can specify multiple branches by using an asterisk (`*`) as a wildcard in this field. The asterisk means "match zero or more of any character."

Here are some examples of valid branch expressions:

* `main` – The branch named "main"
* `*` – All branches
* `main*` – All branches that start with "main"
* `*main` – All branches that end with "main" 

Keep the following in mind:

* Branch expressions are case sensitive.
* White spaces are allowed.
* Do not use multiple asterisks in the branch expression. For example, `**main` is an invalid expression.
* Do not use the asterisk between two words. For example, `main*main` is an invalid expression.

#### Pipeline Variables and Dependent Steps

Some steps depend on the outputs of other steps. Therefore, you must add Checkout before Build, Build before Publish, and Publish before Deploy.

{{% alert color="info" %}}
If you try to add a dependent step without the step that creates the output it depends on, a validation error will display and prompt you to add the missing step first.
{{% /alert %}}

There are two types of variables:

1. Mendix-defined variables

    * These are provided by Mendix. Every pipeline step results in some outputs which can be referenced in subsequent steps. For example, Publish uses the output of Build as `$Build.DeploymentPackage`. Similarly, Deploy uses `$Publish.DeploymentPackage` to deploy to the selected environment. Click **Outputs** inside a Pipeline **Designs** tab to view a step’s output variables. Step outputs are always written in the format `$StepName.OutputName` for easy reference across other steps.
    * The scope of these variables is specific to a particular pipeline design within a Mendix app.

2. User-defined variables

    * These are defined by project members who have access to create pipelines using the **Variables** tab. These user-defined variables can be used to easily reference values such as API Keys, third-party tool’s app IDs, and more.
    * The scope of these variables is limited to the project they are created for. These variables can be referenced in multiple pipelines. For example, if a variable 'Unit Test Remote API Password' has been setup as a variable, it can be used across multiple pipeline definitions for the same Mendix application.

### Saving, Activating, and Deactivating a Pipeline

#### Saving

When you are ready, click **Save**. This saves your design as a draft but does not activate it. You can continue editing your saved draft at any time.

#### Activating

When you are finished editing your pipeline design, click **Save & Activate**. This does the following:

1. Saves your design.
2. Checks if all mandatory fields in your pipeline steps have been filled out. If any of the steps are missing information, an error message displays and the step with the missing information is highlighted.
3. Checks if your user settings have been configured. If you have not yet added a personal access token and API key in the [**Settings** tab](#settings-tab), you will be prompted to add your user settings before activating the pipeline.
4. If the above conditions are met, activates your pipeline.

Once activated, your pipeline runs automatically according to the trigger defined in the [Start Pipeline step](#pipeline-steps).

#### Deactivating

To deactivate an active pipeline, click **Deactivate** in the pipeline's design. If you want to use the pipeline again, you can always click **Save & Activate** to reactivate it.

Note that active pipelines cannot be edited. If you want to edit an existing pipeline, you must deactivate it before making changes.

The pipeline design's status (**Active** or **Inactive**) is displayed in the overview table on the **Designs** tab.

## The Variables Tab{#variable-tab}

The **Variables** tab allows you to create user-defined pipeline variables, for example, Unit Testing Remote API Key which can be referenced in Pipeline designs for the application. It is useful when you want to use the same value in multiple pipelines or avoid storing secret credentials as plain text in your pipeline step.

### Creating a New Variable

To create  a new variable, click **Create New Variable** from the **Variables** tab and open a dialog box. Enter a name for your variable in the **Name** field. The variable name must begin with a letter or underscore. (`_`) and must be unique from existing variables.

Click **Save Variable** to save your variable. You can now select it in the unit testing step and in other steps that allow the use of variables in the future.

Click **More Options** ({{% icon name="three-dots-menu-horizontal-filled" %}}) > **Edit** on a saved variable to edit and update the variable or its value.

### Masked Variables

Select **Mask > Yes** for confidential values. It will never be printed in the logs or error messages.

While editing a saved variable, selecting **Mask > No** will allow you to verify the value. Make sure to set it back to **Mask > Yes** before saving the variable.

## The Settings Tab{#settings-tab}

The **Settings** tab lets you configure user settings. You must add your API key and personal access token (PAT) before you can activate or run your first pipeline. If you still need to configure these user settings, the **Settings** tab is marked with an alert icon ({{% icon name="alert-circle-filled" color="red" %}}).

Adding your API key and PAT allows your existing permissions to be used to run the pipeline. For example, if the trigger in your Start Pipeline step is set to **Teamserver push (Git)**, then the pipeline runs whenever someone pushes to the specified branch. However, if that user has not configured their user settings yet, then the pipeline run will fail.

Or, if the user has configured their user settings but does not have sufficient [permissions](/developerportal/deploy/node-permissions/) for all the steps in the pipeline, then the pipeline will stop running once it reaches the step where the permissions are lacking.

{{% alert color="info" %}}
You only need to add your API key and PAT once; the settings configured here apply to all future pipeline runs across all of your apps.
{{% /alert %}}

### Configuring User Settings{#configure-settings}

To configure your user settings, click **Setup** on the **Settings** tab. This launches the **Setup** dialog box, where you can enter your email, API key, and PAT.

{{% alert color="warning" %}}
Your pipeline runs will fail if these user settings are not configured. All team members who run pipelines need to add these user settings.
{{% /alert %}}

Your PAT should have the following scope:

* Deployment Mendix Cloud – `mx:deployment:write`
* Model Repository – `mx:modelrepository:repo:read` and `mx:modelrepository:write`
* Webhook Portal – `mx:webhook:read` and `mx:webhook:write`

If your PAT scope does not include all five of these, your pipeline runs may fail.

For security reasons, the API key and PAT values are not displayed once they are saved; instead, you see a {{% icon name="checkmark-circle" %}} icon in the overview table if the values are saved and a {{% icon name="remove-circle" %}} icon if no values are saved.

To change your API key and PAT, click **Delete** and then **Setup** to relaunch the **Setup** dialog box. Then provide your new API key and PAT values.

### Notifications

If the pipeline fails, it sends a notification to the user who triggered the pipeline. The notification is sent via email or the **Notifications** ({{% icon name="alarm-bell" %}}) menu in the Mendix Portal, depending on the user's [notification settings](/mendix-profile/user-settings/#notifications).

{{% alert color="warning" %}}
Pipeline failure notifications only send if the user who triggered the pipeline has previously saved a pipeline or added their API key and PAT in the **Settings** tab.
{{% /alert %}}

## Additional Notes

Pipelines time out if they run for more than three hours. In other words, if the operations in your pipeline cumulatively take longer than three hours to complete, then the pipeline will fail.

Mendix automatically creates a webhook on your behalf to trigger pipelines based on [Teamserver push (Git)](/developerportal/deploy/webhooks/#teamserver-push-git):

* This webhook is not visible to first-time pipeline users. 
* For existing pipeline users, you can view this webhook by clicking **Webhooks** in the [navigation pane](/developerportal/#navigation-pane). Do not delete this webhook. Deleting it causes failures for pipelines that rely on the Teamserver push (Git) trigger.

### Known Issues and Limitations

Pipeline runs are not picked up and executed if the Pipelines functionality is undergoing maintenance.

It is not currently possible to add the same pipeline step more than once in a pipeline. This will be improved in the future.

In case of a system outage, pipelines triggered during that time are not executed.

## Read More

* [Learning Path: Choose the Right Software Delivery Approach](https://academy.mendix.com/link/paths/156/Choose-the-Right-Software-Delivery-Approach) – This learning path provides a structured, hands-on introduction to Pipelines, as well as a couple of other methods for building and deploying Mendix apps.
* [Implement a Simple CI/CD Pipeline with Mendix APIs](/howto/integration/implement-cicd-pipeline/) – This document describes how to use Mendix APIs to set up your CI/CD process; this is possible if you use Jenkins, GitLab, or another CI/CD tool.
