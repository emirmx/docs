---
title: "Comparing Revisions"
url: /refguide/comparing-revisions/
weight: 25
description: "How to compare a historical revision to your current state to understand what changed."
---

## Introduction

The Comparison Pane allows you to compare any historical revision of your version-controlled app to your current working state. This helps you understand what has changed since a specific commit, see the impact of your recent edits, or review what would be affected if you reverted to a past revision.

The comparison shows three levels of detail: which documents changed, which elements within those documents changed, and which property values differ between the revision and your current state.

**Key limitation:** You can only compare a selected historical revision to your current working state. It is not possible to compare two arbitrary past revisions to each other.

This feature is similar to the [Changes Pane](/refguide/changes-pane/), but while the Changes Pane shows your local modifications since your last commit, the Comparison Pane shows differences between any historical revision and your current state.

## Starting a Comparison

To compare a historical revision to your current state:

1. Open the History pane by clicking **View** > **History**.
2. Right-click the revision you want to compare.
3. Select **Compare to current state**.

The [Comparison Pane](/refguide/comparison-pane/) opens and displays all differences between the selected revision and your current working state, including any uncommitted changes you have made.

## Version Compatibility

When you compare revisions created in older versions of Studio Pro, the older revision is automatically converted to the current Studio Pro version format. This conversion allows you to compare across versions, but the displayed older revision may not be 100% identical to the original due to model conversion differences. For more information, see the [Version Compatibility](/refguide/comparison-pane/#version-compatibility) section in *Comparison Pane*.

## What Gets Compared

The comparison shows differences between:

* **Older revision** – The historical revision you selected from the History pane
* **Newer revision** – Your current working state, including all uncommitted changes

Because the comparison always includes your current uncommitted changes, you can use this feature to see how your recent edits differ from any point in your app's history.

## Understanding the Comparison Results

The Comparison Pane shows results at three levels:

### Document-Level Differences

At Level 1, you see a list of all documents that differ between the two revisions. Each document has a status:

* **Added** (green circle) – The document exists in your current state but did not exist in the selected revision
* **Modified** (yellow circle) – The document exists in both versions but has changes
* **Deleted** (red circle with minus) – The document existed in the selected revision but no longer exists in your current state

### Element-Level Differences

At Level 2, you see which elements within a document have changed. For example, in a page, you might see which widgets were added, modified, or deleted. In a domain model, you might see which entities or associations changed.

### Property-Level Differences

At Level 3, you see the specific property values that differ for a selected element. The grid shows three columns:

* **Property** – The name of the property (shown as a tree structure for nested properties)
* **Older** – The value in the historical revision
* **Newer** – The value in your current state

Grey rows in the property tree represent intermediate path levels without a direct value. The properties are ordered to match how they appear in the corresponding dialog boxes.

## Navigating the Comparison

Use these methods to navigate through the comparison:

* **Double-click** a row to drill down to the next level
* **Press <kbd>Enter</kbd>** on a selected row to drill down
* **Click the Go to button** to drill down or focus on an element in the document
* **Click the Back button** or **press <kbd>Backspace</kbd>** to return to the previous level
* **Use the Expand all button** to expand all property tree nodes in Level 3
* **Use the Collapse all button** to collapse all property tree nodes in Level 3

## Opening Documents

When you click **Go to** or double-click a document in the comparison, the document opens as it currently exists in your project. This allows you to see the document in context and make further edits if needed.

If the document no longer exists in your current state (it was deleted), Level 2 and Level 3 are still displayed so you can review what was in the document, but nothing opens in the editor.

## Refreshing a Comparison

When you are viewing a comparison and you save changes to your app, a **Refresh** button appears in the Comparison Pane. Click **Refresh** to update the comparison to include your latest saved changes. This allows you to iteratively make changes and see how they affect the comparison without restarting the comparison from scratch.

## Stopping a Comparison

Click the **Stop comparison** button in the Comparison Pane to close the comparison. The pane returns to a blank state and is ready for you to start a new comparison. Documents that were opened as part of the comparison remain open in your editor.

## Common Scenarios

### What Changed Since This Commit?

To see all changes made since a specific commit:

1. Open the History pane and find the commit in question.
2. Right-click the commit and select **Compare to current state**.
3. Review the document, element, and property-level differences.

This helps you understand the scope of work done since that commit, whether by you or other team members.

### What Would Reverting to This Revision Undo?

Before reverting to an older revision, you can preview what would change:

1. Open the History pane and find the revision you are considering reverting to.
2. Right-click the revision and select **Compare to current state**.
3. Review the differences to understand what would be undone by the revert.

This helps you make an informed decision about whether reverting is safe or if it would undo important work.

### Did My Recent Changes Affect This Area?

To check if your recent uncommitted changes affected a specific area of your app:

1. Open the History pane and select your most recent commit (HEAD).
2. Right-click and select **Compare to current state**.
3. Look for the documents or elements you are concerned about.

Because the comparison includes uncommitted changes, you can see the full impact of your current working session.

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
