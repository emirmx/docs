A custom icon has been [requested](https://mendix.atlassian.net/wiki/spaces/MXICONS/pages/3741155509)

## Entry points
- History → Select revision → Compare revision
![Indication of where the Compare revisions button should go on the history panel](HistoryCompareButton.png)
- History → Right click on a revision
    - Compare to previous
    - Compare to parent
    - Compare to current state
    - Compare to… (This one opens the dialogue as specified below)
- MxDock menu → Version Control → Compare Revisions…
![Indication of where the Compare revisions item should go in the MxDock menu](MenuCompareRevisions.png)
- Comparison pane → Select revisions
- Comparison alert → Select revisions

## Selecting revisions
![Example Compare revisions dialogue](SelectComparison.png)
- If the entry point was through history, the selected revision will be set as the ‘Older Revision’ and the corresponding branch line will be set as ‘Branch line’. Newer revision will be on ‘Current revision + uncommited changes’. 
![Compare revisions dialogue when entering through history](FromHistory.png)
- If the entry point was through MxDock menu or the ‘Select revisions’ button and no comparison is active, then the branch line is set to the branch line they are currently working in, the Older revision will be set to (none). Newer revision will be on ‘Current revision + uncommited changes’.
![Compare revisions dialogue when entering through MxDock](FromMxDock.png)
- If the entry point was through MxDock menu or the ‘Select revisions’ button and a comparison is currently active, the settings of the current comparison are shown
- When clicking on the ‘Select’ button for Branch line, the following popup is shown:
![Branch line selector](SelectBranch.png)
    - The search function searches both the Name and Author Last Commit
    - The headers of the columns can be clicked to sort that column in ascending/descending order
    - The date/time of the last commit is shown using local date/time notation
    - Author is shown using full name, on hover author information is shown in a tooltip as `Firstname Lastname <email@adres.com>`.
    - On hover of any cell in the Name, or Last Commit column will show the contents of that cell in a tooltip
- After selecting a different branch line, both revisions are set to (none)
- Clicking the ‘Select’ button for Revision in Older revision or in Newer revision → Compare to → Select revision, will call the existing ‘Select Revision' dialogue
![Select Revision dialogue](SelectRevision.png)
    - When selecting a revision for ‘Older revision’ while a revision has already been selected for ‘Newer revision’, all revisions that are newer than the ‘Newer revision’ are disabled
    - When selecting a revision for ‘Newer revision’ while a revision has already been selected for ‘Older revision’, all revisions that are older than the ‘Older revision’ are disabled
- The arrow buttons next to the ‘Select’ button, will set the currently selected revision as the revision on the other side. So, if the one in the ‘Older revision’ area is used, the ‘Newer revision’ will be set to ‘Select revision’ with the revision that was set as the ‘Older revision’ and the ‘Older revision’ will be set to none and vise versa.
- Clicking the ‘Compare’ button will start the comparison

## Refresh behavior
| Newer                | When                            | Behavior                                                                                   |
|----------------------|---------------------------------|--------------------------------------------------------------------------------------------|
| Current state        | Saving changes in current state | Activate ‘refresh’ button, which when clicks updates the comparison to include new changes |
| Current state        | Making new commit               | Update comparison to compare to new commit                                                 |
| HEAD (latest commit) | Making new commit               | Keep same comparison, update labels if needed                                              |
| Anything else        | Making new commit               | N/A                                                                                        |

## Alert
![Alert banner for top of StP](Alert.png)
- Once a comparison has started an alert is shown at the top of the modeler, directly under the MxDock to indicated what revisions are being compared
- Text should be `Comparing ‘Older’ revision: 2023/4/13 1:45 PM - f90c8509 to ‘Newer’ revision: 2023/4/21 11:28 AM - 8e9c2792`. So `Comparing ‘Older’ revision:` + Date/time of Older revision according to local formatting + ` - ` + commit hash of Older revision + ` to ‘Newer’ revision: ` + Date/time of Newer revision according to local formatting + ` - ` + commit hash of Newer revision
- If ‘Current revision + uncommited changes’ was selected while setting up the comparison, the text should be: `Comparing ‘Older’ revision: 2023/4/13 1:45 PM - f90c8509 to ‘Newer’ revision: 2023/4/21 11:28 AM - 8e9c2792 + uncommitted changes`

## Comparison pane
![Example of the comparison pane](ComparisonPane.png)
- For every cell in a grid it should be possible to right-click → Copy
- When a comparison is started the comparison pane becomes visible
    - If it hasn’t been opened before it is shown in the bottom pane
    - If it has been opened before, but since been closed, it will open in whichever location it was last used
    - If it is already open, it is brought into focus
- The tab of the comparison pane has a badge which indicates how many documents are different between the revisions
![Comparison pane tab appearance](Tabs.png)

### Level 1
- Level 1 is the version of the Comparison pane that shows a list of the documents that are different between the two selected revisions
- If the app does not have version control. All buttons should be disabled, the badge in the tab should disappear and a message is shown.
![Comparison pane for an unversioned app](Unversioned.png)
- Show a spinner during processes that are likely to take longer than 2 seconds (e.g. starting a comparison)
![Comparison pane with spinner](Spinner.png)

#### Level 1 → Task bar
![Task bar for level 1](TaskBarL1.png)
- **Back button**: This button is always disabled when at level 1, this is to prevent all the buttons moving to the right when navigating to level 2/3
- **Go to button**: This opens the selected document and navigates to the corresponding level 2/3 (This is also the behavior for double clicking on a row)
    - If a document has been deleted:
        - In [phase 1](#phase-1): Open the document if available in current version, if not, disable the button with a tooltip on hover that says “This document doesn’t exist in your local project.”
        - In [phase 2](#phase-2): Show the old version of the document
    - If the change is about a module (adding, changing, deleting)
        - Focus on it in the app explorer if possible
        - If the module is not available in the current project, disable the button with the tooltip on hover that says “This document doesn’t exist in your local project.”
    - If the change is the project itself, focus on the project node in the app explorer
- **Select revisions button**: This button opens the ‘Select revisions’ dialogue as specified above
- **Stop comparison button**: This button stops the comparison:
    - All open documents that are showing a split-screen comparison are closed.
    - The alert at that top of the IDE disappears. 
    - The comparison pane goes to it’s ‘blank’ state
![Comparison pane when there is no comparison](NoComparison.png)
        - The hyperlink text has the same behavior as the ‘Select revisions’ button
        - ‘Go to’ and ‘Stop comparison’ buttons are deactivated
        - Show purely visual changes toggle: If the toggle is active, documents that only have purely visual changes are shown. If the toggle is not active, these documents are hidden.

#### Level 1 → Grid
![Grid for level 1](GridL1.png)
- Column width is adjustable
- Spacing of the icon & text in the Status and Item column should be consistent with how they are shown in the commit details in the history pane.
- The headers of the columns can be clicked to sort that column in ascending/descending order
- Full value should be shown in a tooltip when hovering over a cell

### Level 2/3
![Comparison pane while viewing level 2 and 3](ComparisonPaneL2.png)
- Levels 2 and 3 are what is shown in the comparison pane after having double clicked on a document

#### Level 2/3 → Task bar
![Task bar for level 2 and 3](TaskBarL2.png)
- **Back button**: Navigate back to Level 1
- **Go to button**: Focus on the element that has been selected in Level 2 in the document and display the corresponding changed properties in level 3
    - If an element is deleted, go to button will remain active so it can be used to navigate to the correct document (e.g. when viewing a different tab)
- **Select revisions & Stop comparison buttons**: Same as level 1
- **Expand all button**: Expands all grey rows on level 3
- **Collapse all button**: Collapses all grey rows on level 3
- **Show purely visual changes toggle**: If active, show purely visual changes in level 2, if not, hide them

#### Level 2 → Grid
![Grid for level 2](GridL2.png)
- The level 2 grid shows all changes to the elements of the current document (except for visual changes if the toggle is inactive)
- Full value should be shown in a tooltip when hovering over a cell
- Spacing of the icon & text in the Status and Item column should be consistent with how they are shown in the commit details in the history pane.

#### Level 3 → Grid
![Grid for level 3](GridL3.png)
- Level 3 shows the values that have changed within the element that has been selected in level 2
- There are three columns: Property, which shows all labels, Older which shows the values of the revision selected in ‘Older revision’ and Newer which shows the values of the revision selected in ‘Newer revision’
- Paths of the values that have been changed should be consolidated into a tree view
    - The grey rows indicate levels of the paths that don’t directly represent a value
    - The order of the rows should reflect the order (top to bottom, left to right) that the corresponding components are displayed in their dialogues

## Divider/Splitter
![Splitter used between level 2 and 3](Splitter.png)
- There is a draggable splitter between level 2 and level 3
- There should be no padding between the splitter and the elements next to it

## Showing document
- When the Go to button or double click is used for a document in the Comparison pane, it should be opened

### Phase 1
- The document, if available, should be opened as it currently available in the project
- If the document is not available, level 2/3 is shown, but nothing else

### Phase 2
![Tabs in phase 2](Phase2.png)
- Both the versions of the document are opened
- Each version will have blue text in the tab to indicate which corresponds to the Older/Newer revision
- On opening, the Older version will be to the left of the Newer version in the tabs bar

### Phase 3
- Visual diffing! What it will look like is TBD