---
title: "Combining Changes and Conflict Resolution"
linktitle: "Combining Changes and Conflict Resolution"
url: /refguide/merge-algorithm/
weight: 10
description: "Describes combining changes with conflict resolution flow."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
aliases:
    - /refguide/new-merge-algorithm
    - /refguide/rebase-conflict
    - /refguide/rebase
---

## Introduction

While working on your changes you may find that your local copy of the app model does not have all the changes that other team members have [committed](/refguide/commit-dialog/) to the server (the [Mendix Team Server](/developerportal/general/team-server/), or an [on-premises server](/refguide/on-premises-git/)). In Git terminology this is called being behind.

When this happens, Mendix Studio Pro offers two ways to combine your changes with changes from the server: [Rebase](#rebase) and [Merge commit](#merge). 

Both options support the following features when it comes to [resolving the conflicts](#resolve):

* Fine-grained conflict resolution – When there are conflicting changes in a document, you do not have to choose between whole documents, resolving a conflict using your change or using their change. Instead, you can resolve conflicts at the level of individual elements, such as widgets, entities, attributes, or microflow actions. All non-conflicting changes from both sides are accepted automatically.
* No conflicts on changes to lists of widgets – When two developers make changes to widgets in the same document there is no conflict, the changes are combined. However, if the changes are made too close to the same place in the document, a list order conflict is reported that reminds the developer who is merging the changes to decide on the final order of the widgets in the list.
* They can be aborted at any time. Studio Pro will continue from your latest local commit.

The differences between the two approaches are as follows:

* Rebase (default): 
    * Treats changes from the server as leading, by first retrieving the server state, and then reapplying your work to it. 
    * Results in a simple commit history.
    * Resolves conflicts when reapplying your local commits. If you have three local commits, this could trigger conflict resolution three times.
* Merge commit: 
    * Treats local and remote changes equally, and combines them in a separate merge commit.
    * Results in a more complicated commit history with extra merge commits.
    * Resolves conflicts once, regardless of the number of local and remote commits being merged.

{{% alert color="info" %}}
In general, Mendix recommends using the Rebase strategy when combining changes, especially when actively collaborating on the same branch.

In exceptional cases, for example when you have a lot of local commits where you expect conflicts, a merge commit may be the better choice. 
{{% /alert %}}

Both processes are guided by [notification controls](#notifications) showing the actual state and possible next steps.

## Scenario {#scenario}

The clearest way to explain and illustrate the differences between Rebase and Merge commit when combining changes, is to examine an example scenario. The sections [Rebase](#rebase) and [Merge commit](#merge) show how the two approaches work.

### Starting Point 

To start this scenario, let us assume that you have added the following entities to the domain model of your project:

* User
* Game

The User entity includes the string attributes **E_mail** and **Second_E_mail**.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/DomainModel/Starting_State.png"  >}} 

### Your Local Changes 

During your work you make the following changes, each one in separate commit:

* Rename *E_mail* to *Email*.

    {{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/DomainModel/First_Local_Commit.png"  >}} 

* Rename *Second_E_mail* to *Second_Email*. 

    {{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/DomainModel/Second_Local_Commit.png"  >}}   

### Another User's Changes

In the meantime, your colleague also decided to make changes to both email fields. They have renamed *E_mail* to *EmailAddress* and removed *Second_E_mail* entirely. They have then pushed their changes to the server.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/DomainModel/Remote_State.png" >}} 

### Summary 

The current situation could be represented as shown below.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_Starting_state.png" alt="Team Server with three commits (1, 2, and 4), while in Studio Pro there are also three commits (1, 3, and 5)" width="525"  >}} 

## Combining Changes {#combine-changes}

This section outlines two possible approaches to the [example scenario](#scenario): [Rebase](#rebase) and [Merge commit](#merge). 

Every time changes can be combined, for example when pulling changes from the server, you can choose the approach. You can change the default approach for your user account by adjusting the [Preferences](/refguide/preferences-dialog/#git).

### Rebase {#rebase}

Rebasing is the default way to integrate your work with the server changes. It moves your changes to the top of the changes from the server.

{{% alert color="warning" %}}
Rebase is available only for Git version 2.41.0 and above.
{{% /alert %}}

{{% alert color="info" %}}
During the rebase process, there is a slight terminology change in Studio Pro compared to merge.
{{% /alert %}}

The following sections describe a possible rebase process for the [example scenario](#scenario).

#### Rebase Started 

After starting the rebase, your two commits (`#3` and `#5`) are temporarily put aside. Studio Pro shows the latest changes which were pulled from the server, including commits `#2` and `#4`.

{{% alert color="info" %}}
Your work is now labelled *Theirs*, while the server changes are labelled *Mine*. This is opposite to the way that the work is labelled for a merge commit.
{{% /alert %}}

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_Step_1.png" width="525"  >}} 

When merge conflicts are automatically resolved, Studio Pro adds a **#Conflicts** tag to the commit message. The **#Conflicts** tag in Studio Pro serves as a smart indicator, highlighting files that encountered conflicts during the rebase process. This feature is particularly valuable as it maintains a record of auto-merged conflicts, giving developers clear insight into the merge history.
Key benefits of this approach are the following:

* Transparency – It clearly shows which files required conflict resolution.
* Efficiency – Auto-merging saves time while still flagging areas for review.
* Traceability – It maintains conflict history to support code reviews and troubleshooting.

#### Resolving the First Conflict{#resolving-first-conflict}

Git tries to apply your first commit (`#3`) to the top of the rebasing branch (*Mine*). The commit will come after commit `#4`. 

If there are no conflicts when comparing your commit (`#3`) with the latest state from the server (`#4`), Studio Pro automatically continues. A new commit is then created from your commit, shown as commit `#3` in the image below. The process then continues with the next commit (`#5`).

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_Step_2.png" width="525"  >}} 

In the example, however, there is a conflict because the **E_mail** attribute was renamed both on the server, and in your local work.

In the **Changes** pane, you can see your change in the **Theirs** column, and your colleague's work in the **Mine** column. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Conflicts/Rebase_First.png" >}} 

You must [resolve the conflict](#resolve) to proceed with the rebasing process. After resolving the conflict you can amend the current commit message. Commit `#3` is then created. 

Studio Pro can now continue with rebasing the next local commit (`#5`).

#### Resolving the Second Conflict 

While rebasing the next commit (#5), another conflict is detected. You can choose to resolve this conflict by using either *Theirs* or *Mine*, or by reverting to the original.

You can also make additional changes which are added to the same commit. For example, you can add a **Login** attribute to the **User** entity. These changes are represented as *Mine*, together with changes that were taken from the server. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Conflicts/Rebase_Mine_Change.png"  >}} 

Once the conflict is resolved and you continue the rebase, a new commit (`#5`) is created from your commit (`#5`), and you can optionally amend the commit message. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_Step_3.png" width="525"  >}} 

As this was the last local commit to reapply, the rebasing can now be completed.

#### Testing Changes 

Once the rebase process is completed, the original commits (#3 and #5) that were put aside are now removed. The final state of the branch has the commits `#1`, `#2`, `#4`, `#3`, and `#5'`, while the server still only has commits `#1`, `#2`, and `#4`.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_Step_4.png" width="525"  >}}

Your work is still on your local machine and you should test whether the combined state works as expected.

#### Pushing Changes 

After testing the merged changes, push your work to the server to set the server state to the same as your local state.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_Step_5.png" width="525"  >}}

{{% alert color="info" %}}
If a colleague has pushed other changes to the server while you were working on the merge, Studio Pro will again ask how to combine your current work (including the new `#3'` and `#5'` commits) with the latest changes on the server.
{{% /alert %}}

### Merge Commit {#merge}

Merge commit is an alternative way to integrate your work with remote changes. The combined state is committed using a separate merge commit.

The following sections describe a possible merge commit process for the [example scenario](#scenario).

#### Merge Started 

After starting the merge process, Studio Pro combines your local work (`#3` and `#5`) with the state of the server (`#2` and `#4`). 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Merge_Step_1.png" width="525" >}} 

{{% alert color="info" %}}
Your work is now labelled *Mine*, while server changes are labelled *Theirs*. This is opposite to the way that the work is labelled for a rebase.
{{% /alert %}}

You must create a merge commit that merges commits `#2` and `#4` into your work. The changes already in your local work (`#3` and `#5`) are kept.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Merge_Step_2.png" width="525"  >}} 

#### Conflict Resolution: Resolving the Two Conflicts

If conflicts arise between any local and remote commits, you must resolve them before creating the merge commit for the combined state. Unlike rebasing, which requires conflict resolution for each local commit with conflicts, here you have only one round of conflict resolution.

##### Handling the Renaming of the E_mail attribute

As the **E_mail** attribute was renamed on both the server and in your local work, you must decide which changes to retain. Alternatively, you can make yet another change to the attribute. In the **Changes** pane, you can see your change in the **Mine** column, and your colleagues' work in the **Theirs** column. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Conflicts/Merge_First.png"  >}} 

##### Handling the Removal of the Second_E_mail Attribute

Because your colleague removed the **Second_E_mail** attribute on the server, while you renamed it in your local work, there is another conflict which you must resolve.

You can also make additional changes which are added to the same commit. For example, you can add a **Login** attribute to the **User** entity.

After resolving all conflicts you can proceed with testing the app.

#### Testing and Pushing Changes 

When the combined state is tested, you can commit the current state of the app. This is a new commit (`#6`), which always shows that it has merged commits `#3` and `#5`.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Merge_Step_3.png" width="525"  >}} 

By default Studio Pro also pushes your work to the server when making a commit.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Merge_Step_4.png" width="525"  >}} 

{{% alert color="info" %}}
If a colleague has pushed other changes to the server while you were working on the merge, Studio Pro again asks how to combine your current work (including `#6`) with the latest changes on the server.
{{% /alert %}}

### Summary

You have merged your local work with the latest state from the server and resolved all conflicts. For rebasing this meant two rounds of conflict resolution, while for a merge commit you had a single round with all of the conflicts being resolved at the same time.

The history on the server now looks like this:

* After a rebase:
    {{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Rebase_End_state.png" alt="All commits in the order #1, #2, #4, #3', and #5'" width="525"  >}}
* After a merge commit:
    {{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Steps/Merge_End_state.png" alt="Commits #1, #2, #4, and #6, with commit #6 including commits #3 and #5" width="525"  >}}

Rebasing results in a simpler commit history, while a merge commit results in an additional commit that will always show as containing another commit or set of commits.

## Resolving Conflicts {#resolve}

Studio Pro provides the following ways of resolving conflicts: 

* Interactive merge
* Using whole documents

The following sections show how to resolve the conflicts when using [merge commit](#merge). You can resolve conflicts during a rebase in the same way except that *Mine* and *Theirs* are reversed.

### Using Interactive Merge

For the conflict, you can inspect the changes and decide which version to apply. Select the line that represents the conflict and choose **Resolve using Mine** or **Resolve using Theirs**.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Conflicts/Interactive_Merge.png" >}}

The document updates immediately after you click the button. If you are not satisfied with your choice, you can use **Undo** to go back and try another option. 

{{% alert color="info" %}}
You must click the document to bring it into focus before you can use the keyboard shortcuts <kbd>Ctrl</kbd> + <kbd>Z</kbd> and <kbd>Ctrl</kbd> + <kbd>Y</kbd> to undo or redo your choice.
{{% /alert %}}

Instead of using **Resolve using Mine** or **Resolve using Theirs**, you can also resolve a conflict with the **Mark as Resolved** option. This means that you do not choose either side to resolve the conflict and keep things the way they were in the original. Neither of the new text changes are applied.

Once you have chosen one of the three options to resolve the conflict, green check marks appear to indicate that this conflict has been dealt with.

Once all conflicts have been resolved, click **Accept and Exit** to finalize the results. The document is saved and the conflict for that document is gone. The result is a document that contains changes from both sides and possibly some manual edits.

At any time, you can also choose to abort conflict resolution by clicking the **Cancel** button. The conflict will remain, and you can resolve it later.

### Using Whole Documents 

You can resolve conflicted documents too. There are two causes for document conflicts:

* One person deletes a document and the other makes a change inside that document.
* Both people move a document but to different places in the app tree.

The involved document is marked as conflicted and you can see the reason in the details column of the **Changes** pane. You can resolve the conflict by one choosing one of the following options:

* Using my whole document - while merging it will resolve all conflicts in this document using your work
* Using theirs whole document - while merging it will resolve all conflicts in this document using server changes

{{% alert color="warning" %}}
Remember that Mine and Theirs are different, depending on whether you are using rebase or merge commit.
{{% /alert %}}

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/Conflicts/Interactive_Merge.png"  >}}

{{% alert color="info" %}}
If a whole folder (or module) was deleted and another person changes a document inside that folder, the folder or module is restored and also marked as conflicted. This way you know that the intention was to delete that folder but it has been restored to show you the context of the changed document.
{{% /alert %}}

## Notification Controls {#notifications}

While combining changes using either rebase or merge commit, you are shown a notification bar which displays the current status of the process and next available steps.

### Shared Controls

The following controls are displayed for both rebases and merge commits.

#### Abort

The **Abort** button is visible for almost all steps. It indicates whether you are aborting a rebase or a merge commit.

Click and confirm this button to stop the combining process and undo all of the changes up to this point. In other words, the status is reset to the point before starting this process.

This has no effect on changes committed to the server, only your local work.

#### Show Conflicts

The **Show conflicts** button is only visible when there are conflicts in your application, for example conflicting changes to domain models or microflows.

Click this button to bring the **Changes** pane into the view, as this is the place where you can resolve this type of conflict.

#### Show File Conflicts

The **Show file conflicts** button is shown when there are conflicts in files which are not directly linked to your application.

When clicked, it opens up a pop-up window with a list of all the files that are affected by the update process, with conflicted ones at the top of the list.

You can also use the [Show Changes on Disk](/refguide/version-control-menu/#show-changes) menu item instead.

### Rebase-Specific Controls

There are some buttons which are only shown when conflicts are found when doing a rebase.

#### Continue

The **Continue** button is visible when there are no more conflicts to resolve but there are still other changes to rebase. Click it to resume the rebase process.

#### Push

The **Push** button is visible after the rebase has finished successfully and your changes are ready to be pushed to the server. Click it to trigger a [push](/refguide/version-control-menu/#push) operation.

#### Examples

Some examples of the rebase notification bar are shown below.

##### Conflict Detected 

Rebase notification bar while there are still conflicts to be resolved.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/NotificationBars/Rebase_Conflicts.png" alt="Rebase notification bar showing one conflict and the Show conflicts, Show file conflicts, and Abort rebase buttons." width="525" >}}

##### Current Step Resolved

Rebase notification bar when conflicts for current step resolved.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/NotificationBars/Rebase_Continue.png" alt="Rebase notification bar showing current step is resolved and the Continue and Abort rebase buttons." width="525"  >}}

##### Rebase Concluded

Rebase notification bar when whole rebase concluded.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/NotificationBars/Rebase_Conclude.png" alt="Rebase notification bar showing rebase is complete and the Push button." width="525px"  >}}

### Merge-Specific Controls

#### Commit

The **Commit** button is visible when there are no more conflicts to resolve, and the merge process is finished.

Click it to opens up the [commit dialog](/refguide/commit-dialog/) with a predefined message indicating that this is a merge commit.

#### Examples

Some examples of the merge notification bar are shown below.

##### Conflicts Detected

Merge notification bar while in conflicts phase.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/NotificationBars/Merge_Conflicts.png" alt="Merge notification bar showing conflicts detected and the Show conflicts, Show file conflicts, and Abort merge buttons." width="525"  >}}

##### Merging Complete

Merge notification bar when merge is complete.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/merge-algorithm/NotificationBars/Merge_Conclude.png" alt="Merge notification bar showing merge is complete and the Commit and Abort merge buttons." width="525"  >}}
