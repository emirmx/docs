---
title: "Merging and Diffing Commands"
url: /refguide/mx-command-line-tool/merge
weight: 50
description: "Describes the commands related to merging and diffing apps for the mx command-line tool."
---

## Introduction

The commands in this group enable comparing two apps and merging them.

## mx diff Command {#diff}

The `mx diff` command performs a diff of two *.mpr* files and outputs the differences to a file in JSON format.

### Usage

Use the following command pattern for `mx diff`:

`mx diff [options] BASE MINE OUTPUT`

These are the `OPTIONS`:

| Option | Shortcut | Result |
| --- | --- | --- |
| `--help` | | Shows help for the `mx diff` command and exits. |
| `--loose-version-check` | `-l` | Makes the version check loose (meaning, it auto-converts if possible before diffing). |

`BASE` is the first *.mpr* file, which is used as a base in comparison. 

`MINE` is the second *.mpr* file, which is used as the changed version in comparison. The output will contain the changes that are in this file against the base. 

{{% alert color="info" %}}
For example, if the `BASE` *.mpr* has Microflow1 and the `MINE` *.mpr* does not have it, Microflow1 will be listed as deleted in the output file. If you swap the `BASE` and `MINE` parameters and compare again, Microflow1 will be listed as added.{{% /alert %}}

`OUTPUT` is the name of the outputted JSON file.

### Examples

This is an example:

`mx diff C:\MyApp\MyApp.mpr C:\MyApp-main\MyApp.mpr c:\comparison\output.json`

### Return Codes

This table shows the return codes and their description:

| Return Code | Meaning |
| --- | --- |
| `0` | OK. |
| `2` | Conflicts were found during the diff. |
| `4` | The version of either *.mpr* file is not supported. |
| `129` | An error happened during the diff. |

## mx merge Command {#merge}

The `mx merge` command performs a three-way merge of two *.mpr* files by taking their common ancestor (base) into account.

The input is three *.mpr*  files: `BASE`, `MINE`, and `THEIRS`.

### Usage

Use the following command pattern for `mx merge`:

`mx merge [OPTIONS] BASE MINE THEIRS`

These are the `OPTIONS`:

| Option | Result |
| --- | --- |
| `--help` | Shows help for the `mx merge` command and exits. |

`BASE` is common base version of the app. If the app is version-controlled, this is the last common revision of the app (the revision that is present in the history of both branches).

`MINE` is the version to merge into. This *.mpr* contains the results of the merge.

`THEIRS` is the version to merge changes from.

The image below illustrates the meaning of the parameters:

{{< figure src="/attachments/refguide/general/mx-command-line-tool/merge.png" alt="mx merge" class="no-border" >}}

In the diagram, note the following:

* <span style="color:green">**A"**</span> is `MINE`, which is the current commit you want to merge the changes to
* <span style="color:orange">**B'**</span> is `THEIRS`, which is the last commit on a branch you want to merge changes from
* <span style="color:red">**A**</span> is `BASE`, which is the common commit where the branches diverged

In order to merge changes correctly, Studio Pro has to compare both **A"** and **B'** against **A** to see what has been changed on each branch. During the merge, the [merge algorithm](/refguide/merge-algorithm/) will try to automatically merge the changes.

This command works for any three *.mpr* files. This means you can try to merge different apps at your own risk.

{{% alert color="info" %}}
This command works differently than the normal version-controlled merges you can do in Studio Pro. While Studio Pro does a real merge of one branch into another, this command runs the merge algorithm over three *.mpr* files that do not even have to be version-controlled. {{% /alert %}}

### Conflicts

If there are conflicts during the merge, resolve them by opening the app in Studio Pro and selecting **Version Control** > [Merge Changes Here](/refguide/version-control-menu/#merge-changes-here). 

The reason for this is that conflict resolution is a complex process that has two requirements:

* The app has to be version-controlled
* Your Git repository has to be in the merge state (Studio Pro does this when you click **Merge Changes Here**)

This merge state is needed for Studio Pro to know what your current branch is and which branch you are trying to merge into it. This way, when you are trying to resolve the conflict using the `THEIRS` document, Studio Pro can download the document from the branch and put it into your current app. 

So, if you run this command from the command line specifying the three *.mpr* files but the result has conflicts, you will not be able to resolve the conflicts in the `MINE` app using the `THEIRS` documents by just opening the app in Studio Pro. Instead, you need to configure Git to use `mx merge` as a [merge driver](#merge-git-driver) for the *.mpr* files and trigger the merge from the Git command line (so the repository is put in the merge state for Studio Pro to be able to pick it up after the command is complete).

{{% alert color="warning" %}}
`mx merge` as a [merge driver](#merge-git-driver) is suitable only for [MPRv1 Format](/refguide/troubleshoot-repository-size/#mpr-format)
{{% /alert %}}

### Examples

Here is an example:

`mx merge C:\MyApp\MyApp.mpr C:\MyApp-main\MyApp.mpr C:\MyApp-FeatureBranch\MyApp.mpr`

### Return Codes

This table shows the return codes and their description:

| Return Code | Description |
| --- | --- |
| `0` | The merge is successful and there are no conflicts. *MINE.mpr* contains the result of the merge. |
| `2` | Conflicts are detected. Open *MINE.mpr* in Studio Pro to resolve them. |
| `4` | The version is unsupported. |
| `129` | There is an exception, an error occurred during the merge. Error details are printed in the command line output. |

## mx merge as Git Merge Driver {#merge-git-driver}

{{% alert color="warning" %}}
`mx merge` as a [merge driver](#merge-git-driver) is suitable only for [MPRv1 Format](/refguide/troubleshoot-repository-size/#mpr-format)
{{% /alert %}}

{{% alert color="info" %}}
Studio Pro configures the merge driver for an app with the name **studiopro** when opening it in Studio Pro.
{{% /alert %}}

This section outlines the necessary configuration to enable the [mx merge](#merge) command as a merge driver in Git. With this configuration, you can merge one branch into another using third-party version control tools and the Git command line.

Normally, when you are merging branches with Git, it compares the file changes in both branches. If a certain file has been changed in both branches, this triggers a conflict. If conflicting files are text files, Git attempts to resolve it automatically (very often successfully). 

However, if the conflicting files are Mendix apps, the conflict occurs in two .mpr files. Both the files and the conflict itself are more complex, which is why Studio Pro is needed to resolve them. 

For such cases, Git provides an option to delegate conflict resolution for specific file types to an external tool. The `mx merge` command is designed to work with this mechanism, allowing Git to attempt merging the *.mpr* files as Studio Pro would. If conflicts remain, you can open Studio Pro and resolve them manually.

### config File {#merge-config}

Add the lines below to the *config* file located in the *.git* folder of your app on disk.

At the end of the file, add a `[merge "custom"]` block like this:

```ini
[merge "custom"]
    name = custom merge driver for MPR files
    driver = [MX.EXE_PATH] merge %O %A %B
```

Replace `[MX.EXE_PATH]` with a full path to your *mx.exe* file in the Unix format (for example, `'/C/Program Files/Mendix/11.0.0.8753/modeler/mx.exe'`).

{{% alert color="info" %}}
The *.git* folder is a hidden folder in your computer file management system. You can view it when hidden items are visible.
{{% /alert %}}

### attributes File

Create an `attributes` file in the *info* folder of the *.git* directory of your app. Add the following line to instruct Git to use the `[merge "custom"]` driver from the [config](#merge-config) section of this document for merging *.mpr* files.

```ini
*.mpr merge=custom
```

### Verification

To confirm this works, use Studio Pro to create a blank version-controlled app and do the following:

1. Create a branch called *branch* and download it.
2. Change the caption of a home page to *Branch*.
3. Add a microflow named *branch*.
4. Commit and push your changes.
5. Switch back to the **Main** branch.
6. Change the caption of the home page to *Main*.
7. Add a microflow named *main*.
8. Commit and push your changes.
9. Open the Git command line in your app's **Main** branch directory and run `git merge origin/branch`.

If you configured everything correctly, the command line output should look like this:

```code
$ git merge origin/branch
Checking MPR Versions.
Complete.
Converting MPRs
Complete.
Merging MPRs.
Conflicts found during merging. Please resolve them by opening the project in Studio Pro.
Complete.
Auto-merging MyBlankApp.mpr
CONFLICT (content): Merge conflict in MyBlankApp.mpr
Automatic merge failed; fix conflicts and then commit the result.
```

Now, if you open you app on the **Main** branch, you should see the following:

* Both the **branch** and **main** microflows (this is a non-conflicting change, so `mx merge` sorted this out automatically, just like Studio Pro would do)
* A conflict on the **Home_Web** page concerning the renaming of home page caption (this is a conflicting change, as you changed the same caption to different values on both branches, so you can resolve this manually)

{{% alert color="info" %}}
When you get a different output, the custom merge drive is not configured correctly. Abort the merge using the command `$git merge --abort` and close the Git command line tool before making changes to the configuration. Changes made to the configuration *config* and *.gitattributes* files are picked up by reopening the Git command line tool.
{{% /alert %}}

## mx git-merge Command {#git-merge}

The `mx git-merge` command performs a three-way merge of two *.mpr* files by  considering their common ancestor (base).
The command is suitable for both [MPRv2 Format](/refguide/troubleshoot-repository-size/#mpr-format) and the [MPRv1 Format](/refguide/troubleshoot-repository-size/#mpr-format).  

{{% alert color="warning" %}}
The command should be used as a **merge driver** and not as a standalone command like [mx merge](#merge).
{{% /alert %}}

The input is three *.mpr* files: `BASE`, `MINE`, `THEIRS`, and three labels: `BASE_COMMIT`, `MINE_COMMIT` and `THEIRS_COMMIT` which are their respective revisions.

### Usage

{{% alert color="info" %}}
Studio Pro configures the merge driver for an app with the name **studiopro** when  opening it in Studio Pro.
{{% /alert %}}

This section outlines the necessary configuration to enable the `mx git-merge` command as a merge driver in Git. With this configuration, you can merge one branch into another using third-party version control tools and the Git command line.

Normally, when you are merging branches with Git, it compares the file changes in both branches. If a certain file has been changed in both branches, this triggers a conflict. If conflicting files are text files, Git attempts to resolve it automatically (very often successfully). 

However, if the conflicting files are Mendix apps, the conflict occurs in two *.mpr* files. Both the files and the conflict itself are more complex, which is why Studio Pro is needed to resolve them.

For such cases, Git provides an option to delegate conflict resolution for specific file types to an external tool. The `mx git-merge` command is designed to work with this mechanism, allowing Git to attempt merging the *.mpr* files as Studio Pro would. If conflicts remain, you can open Studio Pro and resolve them manually.

{{% alert color="warning" %}}
Currently `mx git-merge` supports merging MPRv2 with MPRv2 and MPRv1 with MPRv1. Merging MPRv2 with MPRv1 or MPRv1 with MPRv2 should be done in Studio Pro.
{{% /alert %}} 

### config File {#git-merge-config}

Add the lines below to the *config* file located in the *.git* folder of your app.

At the end of the file, add a `[merge "custommpr"]` block like in the example below:

```ini
[merge "custommpr"]
    name = custom merge driver for MPR files
    driver = [MX.EXE_PATH] git-merge %O %A %B %S %X %Y
```

and a `[merge "custommprcontent"]` block like this

```ini
[merge "custommprcontent"]
    name = custom merge driver for mxunit files
    driver = true
```

Replace `[MX.EXE_PATH]` with a full path to your *mx.exe* file in the Unix format (for example, `'/C/Program Files/Mendix/10.21.0.63213/modeler/mx.exe'`).

{{% alert color="info" %}}
The *.git* folder is a hidden folder in your computer file management system. You can view it when hidden items are visible.
{{% /alert %}}

With the `custommprcontent` merge driver you specify that all *.mxunit* files will be preserved from the default git merge driver. As all conflict are resolved inside the mx tool, there is no need to solve any conflicts in *.mxunit* separately -- mx tool does it together with resolving the mpr file conflict. The only side effect is that modified and deleted files remain unstaged. They should be staged manually using the git add command or within Studio Pro when committing the merge result.

### attributes File

Create the `attributes` file in *info* folder of *.git* directory of your app. Add the following line to instruct Git to use `[merge "custommpr"]` and `[merge "custommprcontent"]` drivers from the [config](#git-merge-config) section of this document for merging *.mpr* and *.mxunit* files.

```ini
*.mpr     merge=custommpr
*.mxunit  merge=custommprcontent
```
