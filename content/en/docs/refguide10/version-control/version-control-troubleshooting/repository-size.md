---
title: "Troubleshooting Repository Size"
url: /refguide10/troubleshoot-repository-size/
linktitle: "Repository Size"
weight: 20
description: "Explains consequences and root cause of a large repository size and how to mitigate this."
---

## Introduction

In case you are experiencing performance issues when cloning or pulling your app, this may be caused by a large repository size. This document explains why your repository may be large, how Git handles it, and what you can do about this issue.

## Causes of a Large Repository 

There are several reasons why your repository may be large. The most common reasons for a Mendix app are the following:

* Version control systems tend to store the differences between revisions instead of full copies of each revision. Mendix apps are stored in an *.mpr* binary file, and version control systems are traditionally not well-equipped to deal with large binary files. This means that Git stores a larger difference between revisions than it would for text files. This results in rapid repository growth.
* Large files committed to version control, such as *.mp4*, *.pdf*, or *.zip* files.
* Frequent reimporting of modules. Regular module updates do not cause issues, but a module-import heavy workflow can cause a large repository size.

## Issues with a Large Repository

Issues with a large repository are typically observed when cloning an app or a branch from the repository. You may experience a long download or even a timeout. Having a large repository is not new in Git, but surfaces as Git has different cloning/checkout behavior than SVN. In the centralized SVN system, a local checkout only contains a specific revision and the server needs to be contacted for each change. In the decentralized Git system, a local clone by default contains the full history of the repository.

Other places where you might encounter performance issues or timeouts are the following:

* Retrieving a list of branches
* Cloning on the command line or in a CI/CD pipeline
* [Team Server](/developerportal/general/team-server/) page in **Apps** in the Mendix Portal, if you are using Team Server as your Git server

## Preventing and Mitigating a Large Repository Size

### MPR Storage Format {#mpr-format}

#### Traditional MPRv1 Format

An app modeled in Mendix is traditionally stored in a single *.mpr* file. This is essentially a database which contains data for all documents, such as microflows, workflows, pages. As the Mendix app is stored in a single file, your version control system only sees that a single file is changed. To show the exact documents that have changed inside the *.mpr* file a tool that comprehends the format is required, such as Studio Pro.

#### Effect on Repository Growth

Version control systems like Git do not store a full copy of a document for every commit. Instead, they store the difference between the two revisions, also called a delta. For binary files such as the *.mpr* file Git cannot effectively calculate the delta. When a microflow is changed, a small delta of a couple of kilobytes is expected, but the storage format results in a delta of a megabyte or more. The consequence of this is that your Git repository grows more rapidly than you expected.

#### MPRv2 Format {#mprv2}

{{% alert color="info" %}}
MPRv2 was released in Studio Pro 10.18.0 as [Public Beta](/releasenotes/release-status/). In Studio Pro 10.21.0, MPRv2 was released for General Availability (GA) and is automatically used for new apps. When opening an existing app in Studio Pro 10.22 and above, it automatically converts to the MPRv2 format.
{{% /alert %}}

Studio Pro 10.18 introduced a new version of the *.mpr* format: MPRv2. The key difference is that all documents, such as microflows, are no longer stored as part of the *.mpr* file but as separate files in the *mprcontents* directory. The *.mpr* file functions as an index file pointing to all the different files on disk. 

This means that when you change one document, for example, a page, only a small file representing that page will change on disk. This allows Git to calculate an efficient delta and results in a more appropriate repository growth compared to MPRv1. Functionally there is no differences between the split (v2) or the combined (v1) format inside Studio Pro. 

{{% alert color="info" %}}
Collaborating within one app on MPRv1 and MPRv2 branches is possible. To limit repository growth as much as possible, Mendix recommends migrating the most active branches to MPRv2 the first.
{{% /alert %}}

{{% alert color="warning" %}}
Merging MPRv2 apps using the command line with `git merge` or using third-party tools is currently not supported.
{{% /alert %}}

#### Converting MPR Storage Format {#convert}

In Studio Pro 10.18 and above upgrading to the new format is optional and can be done per branch via the [File menu](/refguide10/file-menu/). As of Studio Pro 10.22, conversion happens automatically when opening the app. 

To manually upgrade to MPRv2, open your app and select **File > Upgrade app to split MPR format (v2)**. After a confirmation the app will be converted and you will be prompted to commit.

You can also downgrade branches that are already on MPRv2 to MPRv1 by selecting **File > Downgrade app to combined MPR format (v1)**.

{{% alert color="info" %}}
Before converting the MPR format it is recommended to commit your local work to ensure there is an isolated commit with the conversion step for debugging, in case conversion fails.
{{% /alert %}}

### Decreasing MPRv1 File Size

When a file exceeds the Git compression threshold, 512 MB by default, Git will store a full copy of the file with each new revision instead of only storing the delta. This results in extremely rapid repository growth with both client and server-side consequences.

As the Mendix model is stored in a single file, this threshold can be exceeded by the *.mpr* file. To decrease the MPR file size, consider doing the following:

* [Convert to MPRv2](#convert) – By splitting the single binary file into multiple files, your file size will drop below the compression threshold.
* Remove [excluded and unused documents](/refguide10/dev-best-practices/#excluded-and-unused-documents) – If you have a large number of unnecessary documents in your app model, this can significantly increase the size of the MPR file.
* Decrease duplication in pages – If you have a number of pages featuring the same content, such as an advanced datagrid, consider extracting this piece of logic to a widget. Reusing a widget on multiple pages prevents the data from being saved several times and can have a large impact on the size of the MPR file.
* You can use [analyze-mpr](/refguide10/mx-command-line-tool/analyze-mpr/) of the [mx Command-Line Tool](/refguide10/mx-command-line-tool/) to analyze how your MPR file builds up. The output shows how many documents of a certain type (for example, the number of pages) exist and how much disk space they represent within the MPR file. Mendix recommends starting with a quick scan to see whether there is an unexpected number of occurrences (for example, 1500 pages) or a large number of bytes (over 50 000 000 bytes) for a unit type.

### Working with a Large Repository Size

When cloning an app, the default behavior of Git is to download the full history. As Mendix uses different folders on disk for different branches, downloading full history is done for each branch. To mitigate that, Mendix uses local cloning for subsequent branch downloads. When cloning a new branch, data from a local branch you already have is used to reduce data that needs to be downloaded. 

Starting from Studio Pro 10.12 it is possible to prevent downloading the full history, by changing the [Clone type](/refguide10/clone-type/) to use partial clones. A partial clone downloads all data for a specific revision without downloading the contents of all historical commits.

### Mitigating Large Repository Size

In cases when a repository has already grown to a significant size and you encounter performance issues, you can also consider starting with a clean slate. First, you receive a backup of your Git repository, which can be used locally, or you can restore it onto a Git server. The second step is wiping the history on the Git server, after which only the last commit of the main branch is available. Time for cloning is then reduced tremendously and you can continue working without performance issues.

Mendix has developed a cleanup tool to assist you in shrinking your repository. For more information, see the [Cleanup Tool](#cleanup-tool) section below.

## Cleanup Tool {#cleanup-tool}

Mendix has developed a cleanup tool called git-fixer. It is a Python-based command-line tool which copies a local Git repository to a new folder and removes the history, resulting in a small new repository that only contains the last commit of the main branch.

After cleaning up and pushing the results to the server, team members can run the reset tool to unlink local copies from Studio Pro.

{{% alert color="info" %}}
The tool is currently in public beta. The cleanup tool can be downloaded [here](https://artifacts.rnd.mendix.com/git-fixer/git-fixer-essentials.zip), the reset tool for team members can be found [here](https://artifacts.rnd.mendix.com/git-fixer/git-fixer-sp-reset.zip).
{{% /alert %}}

{{% alert color="info" %}}
This tool is executed on a Mendix Git repository. If your Mendix app is still on SVN you will first have to migrate to Git. In case the Migrate button is not showing on the Team Server page after you open your app in [Apps](https://sprintr.home.mendix.com/), because of the size restrictions, you can reach out to your CSM to get your app safelisted.
{{% /alert %}}

### Cleanup Process

The cleanup tool will reduce the size of the repository to a minimum, by only retaining the latest commit of the main branch. This means that all work on branches that have been merged to main branch are kept, but the commits themselves (author, changes per commit, ...) are not.

{{< figure src="/attachments/refguide10/version-control/troubleshoot-version-control-issues/git_fixer_mode.png" class="no-border" >}}

{{% alert color="info" %}}
Uncommitted work, or work committed to branches that have not been merged to the main branch, will be permanently removed from the repository.
{{% /alert %}}

#### Deciding on the Cleanup

The cleanup is intended to shrink your repository size to mitigate performance issues. We advise to first check whether you and your team are affected by performance issues, as that largely depends on your situation. 

To conclude whether the situation is acceptable for you, follow these steps:

* Ensure the Git app you are downloading is not yet on your machine
* Download the branch through Studio Pro, while manually measuring how long the download takes
  
The first download of a branch on a device is a good indication of the maximum waiting time you or your team member can experience. Subsequent branch downloads use data that is already available locally and will, therefore, be a lot faster.

If the download time was acceptable, or if you have a process where team members do not change often and they do not have to download an app for the first time, you can skip the cleanup.

#### Planning the Cleanup

The cleanup is executed on your local machine on a local version of the repository. A new cleaned repository is created locally and you will need to push it to the Git server after validating the results.

{{% alert color="info" %}}
When planning the cleanup, note the following:

* Changes made by other users after you made your local copy are lost after pushing the cleaned repository to the Git server.
* All users need to get fresh clones from the server after the cleanup is completed. This means all changes that have not been committed and pushed to the server before you download the repository you will use for cleaning, will be lost.
* The process can take up to an hour, so we recommend to align with your team before you start. First do a test run to get familiar with the tool before planning the actual cleanup.
{{% /alert %}}

#### Backing Up the Full History

As you are overwriting the history on the Git server, the history currently stored there will be permanently lost. To preserve this history, for example, to comply with auditability requirements, you can store a copy elsewhere.

To make a backup of your full repository, you can download a local clone, zip the entire folder, and store it in a safe location.

If you need easier access to the repository, for example from Studio Pro, you can choose to push the full history to another (read-only) repository outside Mendix Team Server that can be used as an archive.

#### Executing the Cleanup

The tool itself is a multi-step command line script. Before the cleanup actually starts, the script asks for a confirmation of the settings.

{{% alert color="info" %}}
The script only makes changes to a copy of the original repository. The original repository will not be modified so you can run the script multiple times until you are satisfied with the result. Pushing to the original repository is a separate step. For more information, see the [Pushing Your Results to the Server](#push) section below.
{{% /alert %}}

#### Validating the Results

After cleaning up the tool does an initial check to validate the results. Once that is completed you need to open the app in Studio Pro to check the results manually. Open the *.mpr* file in Studio Pro to check whether the latest commit of the main branch is as expected.

#### Pushing Your Results to the Server {#push}

To store the results of the cleanup in your Git server, you need to push the cleaned repository to the server. To overwrite an existing repository, you need **Force push permissions** to be able to execute the `git push --force` command.

{{% alert color="warning" %}}
Before executing a force push, always ensure you have a backup of your repository as this is a destructive operation.
{{% /alert %}}

Force pushing your results to the server is a separate step, in a separate script. Until this step is executed, the results of the cleanup are only stored locally.

##### Mendix Team Server

If you are using Mendix Team Server as your Git version control server, you can follow the steps below:

* Ensure you have configured a Personal Access Token to use it as described in the [Authenticating to Team Server](/refguide10/using-version-control-in-studio-pro/#authenticating) section in *Using Version Control in Studio Pro*. 
* Run the second script.
    * When prompted, enable force pushing.
    * Conduct the force push.
    * Force push will automatically be disabled again after the first push operation.

##### Other Git Platforms

When using another Git platform than Mendix Team Server, such as GitHub or Azure Devops, you can typically enable force pushing in a portal. 

{{% alert color="warning" %}}
Force pushing allows to make destructive changes to the repository, which can easily lead to unrecoverable errors. We recommend you to give these permissions to as few users as strictly necessary.
{{% /alert %}}

You can follow these steps:

* Enable force pushing for the user that will be pushing the results of the cleanup.
* Run the second script and conduct the force push.
* Disable force pushing again to prevent accidentally making destructive changes.

#### Handling Local Copies

After the results of the cleanup are pushed to the server all local clones need to be reset. This means that each developer of your team who has the project on disk and CI pipelines that have cached data need to get a fresh clone. 

For developers on your team this means they have to ensure Studio Pro can no longer find their local folders. The **sp-reset** tool, shipped together with the Cleanup tool, can be used. Alternatively, they can rename their folders of the app to *old*.

{{% alert color="warning" %}}
Deleting local folders means that any uncommitted work is lost. To retain uncommitted work, move the app folder to a location not known to Studio Pro and manually merge the changes after you downloaded a fresh clone.
{{% /alert %}}

When the local copies have been removed from a machine, Studio Pro can be used as usual to download a fresh clone of your app.

### Troubleshooting

We recommend doing the following:

* Check the **Dependencies** section of the README of the cleanup tool
* Consider re-running with verbose output
* Check the config
* Check the input provided by you in the different steps
* Check whether you have enough free disk space, at the temp location
* Check the local repo location, it should be up-to-date and there should be no uncommitted changes
* Check your git config settings, especially any setting that involves encoding or text conversions: run `git config --list --show-origin`.
* Consider moving your local repo, so that its folder has a shorter name
  
When reaching out to Mendix Support, please include:

* App/Projects ID for your app
* Log file (you can find its location in the command line output)
* Version of the tool, for example, `git-fixer v1.16.5.essentials` (you can find the version number in the command line output)

## Recommendation on Avoiding Git Issues {#recommendation}

In case you are still deciding whether to migrate from SVN to Git, or you are already on Git and want to get a recommendation on what steps to take to solve or prevent Git-related performance issues, please follow the decision trees in the image below:

{{< figure src="/attachments/refguide10/version-control/troubleshoot-version-control-issues/migration-advice-mx10.png" class="no-border" >}}

\* In case your *.git* folder is less than 2GB but you are having performance issues when cloning, please see the [Verify Full Clone Through Git CLI](/refguide10/troubleshoot-team-server-issues/#verify-full-clone-through-git-cli) section in *Troubleshooting Team Server Issues* and contact Support with the relevant information.

\** For more information on the partial clone, see [Clone Type](/refguide10/clone-type/).

\*** For more information on Git cleanup, see the [Cleanup Tool](/refguide10/troubleshoot-repository-size/#cleanup-tool) section in *Troubleshooting Repository Size*.
