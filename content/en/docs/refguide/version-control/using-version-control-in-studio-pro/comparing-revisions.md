---
title: "Comparing Revisions"
url: /refguide/comparing-revisions/
weight: 65
description: "How to compare a historical revision to your current state to understand what changed."
---

## Introduction

You can compare any historical revision of your version-controlled app to your current working state via the **Comparison** pane. It can help you understand what has changed since a specific commit, see the impact of your recent edits, or review what is affected if you reverted to a past revision.

The comparison shows differences between:

* **Older revision** – The historical revision you selected from the **History** pane
* **Newer revision** – Your current working state, including all uncommitted changes

As the comparison always includes your current uncommitted changes, you can use this feature to see how your recent edits differ from any point in your app's history.

{{% alert color="warning" %}}
The key limitation of the **Comparison** pane is that you can only compare a selected historical revision to your current working state. It is not possible to compare two arbitrary past revisions to each other.
{{% /alert %}}

For more information on the **Comparison** pane and its detailed overview, see [Comparison Pane](/refguide/comparison-pane/).

## Common Scenarios

### What Changed Since a Specific Commit?

You can review all changes made since a specific commit, which helps you understand the scope of work done since the selected commit, made by you or other team members.

### What Would Reverting to This Revision Undo?

Before reverting to an older revision, you can compare your current changes to a revision you are considering reverting to and review what changes will be lost. This helps you make an informed decision about whether reverting is safe or if it would undo important work.

### Will My Recent Changes Affect a Specific Area?

To check if your recent uncommitted changes affected a specific area of your app:

1. Open the **History** dialog and select your most recent commit (HEAD).
2. Right-click and select **Compare to current state**.
3. Look for the documents or elements you are concerned about.

As the comparison includes uncommitted changes, you can see the full impact of your current working session.

## Tips and Tricks

* **Use Expand all and Collapse all** in Level 3 to quickly show or hide all nested property paths. This is especially useful when comparing complex elements with many properties.
* **Right-click any cell** in the grids and select **Copy** to copy the cell value to your clipboard. This is useful for documenting changes or sharing information with your team.
* **Press <kbd>Enter</kbd> to drill down** and <kbd>Backspace</kbd> to go back when navigating the comparison. This is faster than using the mouse to click buttons.
* **Understand version conversions** – When comparing older revisions, remember that model conversions may introduce minor differences in how properties are displayed. These do not affect your stored revisions.

## Read More

* [Comparison Pane](/refguide/comparison-pane/)
* [History](/refguide/history-dialog/)
* [Changes Pane](/refguide/changes-pane/)
* [Version Control](/refguide/version-control/)
* [Using Version Control History](/refguide/peer-review/)
