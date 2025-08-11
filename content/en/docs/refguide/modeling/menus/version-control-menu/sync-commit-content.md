---
title: "Synchronizing commit content"
url: /refguide/sync-commit-content/
weight: 30
---


There are several components in StudioPro where some files on disk need to be generated for the App to function. For example:
- Theme cache,
- JavaScript Actions, 
- Java Actions, 
- etc. 

These files are generated based on the Documents in the App. In some cases generation of these files takes long time (typically proportional to the app size). This is why we cannot add those generated files to *.gitignore* file – it might slow down App opening and cause errors in some editors. 
Due to the above we introduced an additional step in **Prepare commit process** that ensures generated content is up to date and generation is complete before Committing the changes to the repository. 

The last step on the dialog below calls synchronization commit content. The current kind of synchronization is displayed in Progress.

{{< figure src="/attachments/refguide/modeling/menus/version-control-menu/sync-commit-content/prepare-commit-process.png" alt="Prepare commit process" >}}

If any synchronization fails during this step the form below is displayed asking if it should continue or cancel commit process explaining the failures and statuses. The form lists only the synchronizers that have failed. 

{{< figure src="/attachments/refguide/modeling/menus/version-control-menu/sync-commit-content/sync-failure-dialog.png" alt="Synchronization failure dialog" >}}

