---
title: "Synchronizing Commit Content"
url: /refguide/sync-commit-content/
weight: 30
---


There are several components in StudioPro where files on disk need to be generated for the App to function. such as theme cache, JavaScript actions, Java Actions. 

These files are generated based on the Documents in the app. In some cases generation of these files takes a long time (typically proportional to the app size). This is why Mendix cannot add these generated files to *.gitignore* file – it might slow down app opening and cause errors. 
Due to the above Mendix has introduced an additional step in **Prepare commit process** that ensures generated content is up to date and generation is complete before committing the changes to the repository. 

The last step on the dialog below calls synchronization commit content. The **Progress** section shows the current type of syncronization:

{{< figure src="/attachments/refguide/modeling/menus/version-control-menu/sync-commit-content/prepare-commit-process.png" alt="Prepare commit process" >}}

If any synchronization fails during this step the dialog below is displayed asking if it should continue or cancel commit process explaining the failures and statuses. The dialog lists only the synchronizers that have failed. 

{{< figure src="/attachments/refguide/modeling/menus/version-control-menu/sync-commit-content/sync-failure-dialog.png" alt="Synchronization failure dialog" >}}

