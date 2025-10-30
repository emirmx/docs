---
title: "Feedback"
url: /developerportal/app-insights/feedback/
weight: 5
description: "Mendix supports [feedback management](/developerportal/app-insights/feedback/) by enabling [Feedback](/appstore/modules/mendix-feedback/) module in any application. App users can easily provide feedback through the feedback button integrated within the applications. This feedback is efficiently processed by the app development team using the Mendix Feedback UI. With the enhanced Mendix Feedback module, your app users gain the ability to capture pixel-perfect screenshots and annotate them, enabling them to clearly convey their specific points and preferences to you."
aliases:
    - /developerportal/collaborate/feedback/
#To update these screenshots, you can log in with credentials detailed in How to Update Screenshots Using Team Apps.
---

## Introduction

If your app uses the [Mendix Feedback](/appstore/modules/mendix-feedback/) module, users can report feedback to your team using the feedback widget in your app, and all their feedback goes to the **Feedback** page. **Feedback** enables your team to view all the feedback. It also allows team members with the right permissions to organize feedback and add feedback manually. 

You can access the **Feedback** page by selecting your app in [Apps](https://sprintr.home.mendix.com/), then clicking **Feedback** in the navigation pane. 

The **Feedback** page contains two tabs: [Inbox](#inbox) and [Archive](#archive).

### What Happens to a User After They Submit Feedback? {#what-happens-to-user}

#### What Happens to Your Team

After a user reports feedback in the app using the feedback widget, the feedback goes automatically to the [Inbox](#inbox) on the **Feedback** page, and the members in your team who are a Scrum Master or have the same permissions as the default permissions of a Scrum Master get a [notification](/global-navigation/#notifications). If you no longer want to receive feedback notifications for a specific app, you can [stop watching](/developerportal/#my-apps) the app. 

On top of getting a notification in the Mendix Portal, Scrum Masters of your team also receive an email with a link to the details of the new feedback.

#### What Happens to the User

After a user reports feedback in the app using the feedback widget, they get an email with a link to their submitted feedback. When the user opens the link, a page opens and shows their feedback details.

##### What a User Can See on the Page

* Name of the feedback they submitted 
* Description of the feedback they wrote 
* Screenshot (if they uploaded a screenshot for the feedback)
* Current status of the feedback
* Submission date
* Last activity on the feedback 

##### What a User Cannot See on the Page

* Metadata
* Summary of the feedback that your team wrote
* To which team member the feedback is assigned 
* Tags that are assigned to the feedback
* Other feedback that are associated with this feedback
* Attachments that you team uploaded for this feedback
* Team comments on this feedback

This page also has a message feature for the user to communicate with your team directly. Users can upload images in the messages. When the user posts a message, the assigned team member receives an email. When there is a reply from your team, the user gets an email notification.

{{% alert color="warning" %}}Do not share the link to feedback details with someone unless it is necessary. Anyone who has this link can access the feedback details, including the title and the description of the feedback, as well as the screenshot if there is one.{{% /alert %}}

If the user does not want to receive notifications anymore, they can clear the checkbox for the notification on the page. 

{{< figure src="/attachments/developerportal/app-insights/feedback/user-notification.png" class="no-border" >}}

## Inbox {#inbox}

On the **Inbox** tab, everyone from your team can view all the unarchived feedback sent from your app by users.  

{{< figure src="/attachments/developerportal/app-insights/feedback/feedback-items.png" alt="inbox tab" >}}

In the list on the **Inbox** tab, you can find the following information for each feedback:

* **ID** – the unique number that is assigned to the feedback
* **Subject** – the title of the feedback given by the reporter
* **Submitter** – the email address of the reporter
* **Tags** – tags that your team has assigned to the feedback
* **Status** – the status that your team has set to the feedback
* **Group** – the group that the feedback belongs to
* **Stories** – the story that the feedback is linked to
* **Priority** – the priority that the feedback has
* **Date** – the date when the feedback was submitted

For more information about each item, refer to the [Feedback Details](#feedback-details) section.

To export all feedback into one CSV file, click **Export**.

To search by **Subject**, **Description**, **User Email** and **Tags**, type the keywords in the search bar.

Clicking the filter bar opens the **Filter by** side panel. In this side panel, you can filter feedback using the **Environment**, **Assignee**, **Priority**, **Status**, and **Submission date**. The filters are saved until the page is refreshed.

### Organizing Feedback in Inbox

If your [role](/developerportal/general/app-roles/) has the **Can edit ‘Stories, Documents and Feedback'** permission, you can also organize feedback.

You can add new feedback manually yourself. 

To manually add feedback:

* Click **Create new feedback** if there is not any feedback yet in the **Inbox** tab.
* Click **Add feedback** if there is already feedback in the **Inbox** tab.

Then, fill in the **Subject**, **Description**, and **Submitter Email** fields, and optionally **Upload Screenshots** in the **New Feedback** dialog box.

If you select feedback, you can see the following items: 

{{< figure src="/attachments/developerportal/app-insights/feedback/bulk-actions-bar.png" alt="bulk-actions-bar" >}}

* **Group Items** – Clicking this button will show you two options:
  * **Create New Group** – You can create a new group for the selected feedback.
  * **Add to Group** – You can add the selected feedback to an existing group.

* **Move** – Clicking this moves the selected feedback to any other app of which you are a member. This action will remove the link to the group if the feedback is part of a feedback group.

* **Create Stories** – If your app is connected to [Epics](/developerportal/project-management/epics/) or [Jira](/developerportal/project-management/jira-connector/), clicking this button converts the selected feedback to stories on the designated board in Epics or Jira.

* Status drop-down list – You can select a status from the drop-down list and then click **Apply** to change the status of the selected feedback.

* **Archive** – Clicking this sets the selected feedback as archived.

* **Delete** – Clicking this deletes the selected feedback permanently. The result is irreversible.

## Groups {#groups}

On the **Groups** tab, everyone in your team can view all the feedback groups in your app.

In the list on the **Groups** tab, you can find the following information for each group:

* **Group Name** – the name of the group
* **Status** – the status of the group
* **Priority** – the priority of the group
* **Assignee** – the assignee of the group
* **Number of Items** – the number of feedback that the group has

You can use the search bar in the list. You can search by group name, assignee, status and priority.

You can add a new group from this page by clicking **Add Group** button below the list

## Archive {#archive}

On the **Archive** tab, everyone in your team can view all the archived feedback.

In the list on the **Archive** tab, you can find the following information for each feedback:

* **ID** – the unique number that is assigned to the feedback
* **Subject** – the title of the feedback given by the reporter
* **Submitter** – the email address of the reporter
* **Tags** – tags that your team has assigned to the feedback
* **Status** – the status that your team has set to the feedback
* **Date** – the date when the feedback was submitted
* **Archive Date** – the date when the feedback was archived

To export all the archived feedback into one CSV file, click **Export**.

To search by **Subject** and **ID**, type the keywords in the search bar.

Clicking the filter bar opens the **Filter by** side panel. In this side panel, you can filter the feedback using the **Environment**, **Closed date**, **Submission date**, and **Assignees**. The filters are saved until the page is refreshed.

## Feedback Details {#feedback-details}

On the [Inbox](#inbox) and the [Archive](#archive) tabs, you can open feedback details by clicking the feedback in the list:

* If you click the **ID** of the feedback, the feedback details open in a new page. 
* If you click a place other than the **ID** of the feedback, the feedback details open as a side panel. 

The following items are available: 

* Buttons ({{% icon name="view-off" %}} {{% icon name="view" %}} {{% icon name="hyperlink" %}} {{% icon name="three-dots-menu-horizontal" %}}) in the upper-right corner
    * {{% icon name="view-off" %}} – Click this button to start receiving [notifications](#notifications) for this feedback. You also automatically start receiving notifications for this feedback whenever you respond to the reporter of the feedback or leave a comment for your team in the **Communications** section within the feedback.
    * {{% icon name="view" %}} – Click this button to stop receiving notifications for this feedback.
    * **Copy Link** ({{% icon name="hyperlink" %}}) – Click this button to copy the URL to the feedback. You can send the URL to other people.
    * **More Options** ({{% icon name="three-dots-menu-horizontal" %}}) – Click this button to open a menu with the following items:
        * **Move** – Click this button to move the feedback to any other app of which you are a member. This action will remove the link to the group if the feedback is part of a feedback group.
        * **Create Story** – If your app is connected to [Epics](/developerportal/project-management/epics/) or [Jira](/developerportal/project-management/jira-connector/), clicking this button converts the feedback to a story on the designated board in Epics or Jira. Once the story is created, clicking this button brings you to the story on the board right away.    
          You can also create feedback-based stories using Maia. For details, refer to the [Creating Stories with Maia](#create-with-maia) section.
        * **Delete** – Click this button to remove the feedback permanently from **Feedback**. The result is irreversible.

* **Assigned To** – This shows to whom the feedback is assigned. When you assign the feedback to someone, they get an email notification.

* **Priority** – This shows the priority that the feedback has.

* **Status** – This is the status of the feedback. There are two default statuses: **New** and **Closed**. Whenever feedback arrives in the app, it is set to **New**. When you close feedback, it is set to **Closed** and moved to the **Archived** page.    
   You can add custom statuses by clicking **Manage Statuses** ({{% icon name="cog" %}}) and adding the new statuses in the dialog box. Custom statuses can only be used for feedback that are not archived. You can filter your feedback based on the status.    
    You can edit the default status name by clicking **Manage Statuses** ({{% icon name="cog" %}}), then clicking the status name.

* **Tags** – These are the tags assigned to the feedback. You can select a tag from the drop-down list or create new tags by typing the tag name inside the text box and then clicking **create [the tag name]**. A tag should contain maximum 50 characters. You can assign multiple tags.

    {{< figure src="/attachments/developerportal/app-insights/feedback/tags.png" class="no-border" >}}

    {{% alert color="info" %}}For more information about how to manage tags, refer to the [Managing Tags](#manage-tags) section.{{% /alert %}}

* **Summary** – This is the summary of the feedback. Only your team members can see this summary.

* **User Feedback** – This shows the original feedback title, feedback description, and screenshot submitted by the reporter.

* **Metadata** – This shows a list of metadata of the feedback.

    * **Active Userrole** – This is the user role the reporter has in your app
    * **Active form** – This is the page from which the reporter sent the feedback
    * **Screen Size** – This is the dimensions of the reporter’s screen in pixels
    * **Browser** – This is the version of the browser the reporter was using
    * **Application location** – This is the URL of the page from which the reporter sent the feedback
    * **Submitter name** – This shows the name of the reporter
    * **Submitter email** – This shows the email of the reporter
    * **Submission date** – This shows the date when the feedback was submitted

* **Story** – This displays any stories that are linked to that feedback.    
    To unlink a story from a feedback, click its corresponding {{% icon name="unlink" %}} **unlink** button.    
    If there are no linked stories, you have the following options:
    
    * **Add to Story** – Add the feedback to an existing story.
    * **Create Story** – Create a new story based on the feedback.

* **Attachments** – This contains attachments to this feedback. To upload an attachment, click **Add** ({{% icon name="add" %}}) and select the file. The following formats are supported: .*peg*, *.png*, and *.gif*. Each attachment can be a maximum of 5 MB in size. Once an attachment is uploaded, only your team members can view, download, and delete the attachment.

* **Communication** – This section is made up of the following tabs:

    * **Team Comments** — Displays all the comments left by your team for this feedback. You can leave comments here and add attachments to each comment.     
      Comments submitted to this tab are only visible to your team members.
    * **Contact Submitter** — Allows you to read messages from the reporter and reply to them. Whenever the reporter sends a message, it is shown in this section. Once you reply to a message from the reporter, they receive an email.   
      For more information, see the [What Happens to a User After They Submit Feedback](#what-happens-to-user) section.

### Creating Stories with Maia {#create-with-maia}

You can use Maia to create a story based on one or more feedback. To do that, follow these steps:

1. Enable Maia in the app [Settings](/developerportal/general-settings/#maia-settings) page.
2. Select a feedback from the list.
3. Click **Create Story**. The **Create Story** window is displayed.
4. Click {{% icon name="sparkles" %}}**Create with Maia**.     
   If Maia is enabled, the **Create Story with Maia** window is displayed.    
   If Maia is not enabled, you are given the option to activate it on the spot, from the **Enable Maia** window.
5. Check the suggested story description.    
   If you do not agree with the suggestion, click **Regenerate Response** to obtain a different suggestion.
   If you agree with the suggestion, click **Apply to Story**.
6. Check Maia's suggestions for the **Title**, **Type**, **Sprint**, and **Epic** fields, and edit them if necessary.
7. Click **Create Story**. 

A banner at the bottom of the window shows you that the story has been created. You can click **Open** to access the story on the **Planning** page.

You can select several feedback to create a story based on all of them.

### Managing Tags {#manage-tags}

From the [feedback details](#feedback-details) page, you can manage tags by clicking **Settings** ({{% icon name="cog" %}}) next to the **Tags** text box.

  {{< figure src="/attachments/developerportal/app-insights/feedback/tag-management-settings.png" alt="tag management setting" >}}

  To create a new tag, follow these steps:

  1. Click **Add Tag**. A text box appears, with a colored circle indicating the color of the new tag.
  2. In the text box, enter the name of the new tag.
  3. If you want to change the color of the new tag, click the colored circle and select a different color.
  4. Click **Save** ({{% icon name="checkmark-circle" %}}) to save the new tag.

  To edit an existing tag, do as follows:

  1. To change the tag name, click the name of the tag. After the name becomes editable. Edit the name in the text box, and then click outside the text box to save the change.
  2. To change the tag color, click the colored circle and select a different color.

  To delete an existing tag, do as follows:

  1. Hover over the row where the tag is listed. A **Delete** button ({{% icon name="trash-can" %}}) appears at the end of the row.
  2. Click **Delete**. The **Confirmation** dialog box opens.
  3. Click **Delete Tag**.

## Group Details {#group-details}

On the [Groups](#groups) tab, you can open group details by clicking the group in the list:

The following items are available: 

* Buttons 
    * **Ungroup** Click this button to ungroup all feedback in the group. This action doesn't delete the group .
    * **Delete Group** Click this button to delete the group and all feedback in this group. This action is destructive and once you delete a group with feedback, you delete both of them.

* **Group Name** – This is the name of the group. You can edit the name by click on the name. It will show a input panel where you can edit the name
  
* **Description** – This is the description of the group.

* **Tags** – These are the tags assigned to the group. You can select a tag from the drop-down list or create new tags by typing the tag name inside the text box and then clicking **create [the tag name]**. A tag should contain maximum 50 characters. You can assign multiple tags.

    {{< figure src="/attachments/developerportal/app-insights/feedback/tags.png" class="no-border" >}}

    {{% alert color="info" %}}For more information about how to manage tags, refer to the [Managing Tags](#manage-tags) section.{{% /alert %}}

* **Status** – This is the status of the group. There are two default statuses: **New** and **Closed**. Whenever you create a new group in the app, it is set to **New**. 
   You can add custom statuses by clicking **Manage Statuses** ({{% icon name="cog" %}}) and adding the new statuses in the dialog box. Custom statuses can only be used for feedback that are not archived or for groups. You can filter your feedback based on the status.    
    You can edit the default status name by clicking **Manage Statuses** ({{% icon name="cog" %}}), then clicking the status name.

* **Assigned To** – This shows to whom the group is assigned. When you assign a group to someone, they get an email notification.

* **Priority** – This shows the priority that the group has.

* **Reply To All** – You can reply to the submitters of all the feedback in the group. The same message will be sent to all of the feedback.

* **Grouped Feedback** – This shows a list of the feedback that are linked to the group. If you click on an item in the list it will show you the feedback details in side panel.

  * **ID** – the unique number that is assigned to the feedback. Clicking the ID will open another page with the feedback details
  * **Subject** – the title of the feedback given by the reporter
  * **Action** – This column contains actions just for the feedback in the row.
    * **Reply to Submitter** - You can reply to the submitter of the feedback.
    * **Ungroup feedback** - You can ungroup the feedback from the group
    * **Group Team Comments** — Displays all the comments left by your team for this group. You can leave comments here and add attachments to each comment.     
      Comments submitted here are only visible to your team members.

## Receiving Notifications {#notifications}

You can receive a notification when new feedback lands in **Inbox** or when a reporter replies to their feedback. To receive notifications, follow these steps:

1. Watch the app on the [Settings](/developerportal/general-settings/) page after you open it in [Apps](https://sprintr.home.mendix.com/).
2. Go to **Feedback** again.
3. Click the **Settings** {{% icon name="cog" %}} icon in the upper-right corner of the page.
4. Turn on the toggle to receive notifications.
5. Choose how often you want to receive notifications by selecting a **Frequency**:    

    * **Instantly** (default) 
    * **Daily email update**
    * **Weekly email update**

### Receiving Notifications for an Individual Feedback {#notifications-individual-feedback}

You can receive notifications for an individual feedback. To set this up, click the {{% icon name="view-off" %}} icon in the upper-right corner of the **Feedback** tab after you open the feedback.     
You also automatically start receiving notifications for an individual feedback once you respond to the reporter of the feedback or leave a comment for your team in the **Communications** section of the feedback details window.
