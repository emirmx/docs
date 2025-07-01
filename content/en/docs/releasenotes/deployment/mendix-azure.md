---
title: "Mendix on Azure Release Notes"
linktitle: "Mendix on Azure"
url: /releasenotes/developer-portal/mendix-on-azure/
description: "Release notes for Mendix on Azure"
weight: 25
---

{{% alert color="info" %}} This feature is currently available to participating customers. For more information, contact your Customer Success Manager. {{% /alert %}}

These release notes cover changes to deployment to [Mendix on Azure](/developerportal/deploy/mendix-on-azure/). There are separate release notes for other deployment targets; for more information, see the [Deployment](/releasenotes/developer-portal/deployment/) release notes page.

For information on the current status of Mendix deployment, see [Mendix Status](https://status.mendix.com/).

### Release date: July 3, 2025
* As of today, Mendix on Azure users can now create and restore environment backups through Private Cloud Portal. Detailed information can be found in the backups [backups](/developerportal/deploy/mendix-on-azure/backups) documentation.
* Moving forward, [Cloud tokens](/control-center/cloud-tokens/) are required for cluster initialization and environment creation in Mendix on Azure except when a trial is active. A check is added under Preflight check which validates if user have sufficient valid cloud tokens.
* The Mendix on Azure portal is now available in Japanese and Korean languages, enhancing user experience for native speakers. Language preferences can be adjusted in the Work environment tab under Preferences.
* We've fixed a portal issue where error messages were incorrectly displayed despite successful resource provisioning.
* We've eliminated the requirement to visit the Private Cloud portal before accessing the Mendix on Azure portal for first-time users.
* We've made improvements to the handling of cluster deployment retries.
* A new option has been added to the Cluster initialize/Edit steps to enable Managed Grafana with Private access.
* We also added a new option under Preflight check which validates that the Azure account used to initialize the cluster has an owner role assigned on the target subscription.

### Release date: May 29, 2025

* We have strengthened the preflight check process to deliver a better user experience.

### Release date: April 24, 2025

* You can now update the **Additional Options** even after the clusters have been initialized.
* We have enhanced our preflight check process to deliver a better user experience.
* A back button has been added to the **Review and Initialize** screen, allowing users to return to the Provision screen.
* A new dropdown filter has been introduced on the **Cluster Overview** page to filter clusters by status.
* Users will now see a message indicating the deployment progress of provisioned resources within the cluster.
* A Creator column has been added to the **Support Overview** page to display who created each support ticket.
* We have resolved an issue by disabling the Zendesk ticket link for users who did not create the corresponding support ticket.

### Release date: March 20, 2025

* We have introduced a Custom Tags option in the Initialization flow.
* We have resolved an issue where a deleted cluster manager could still access the cluster in the Mendix on Azure portal after being removed from the Private Cloud portal for a specific cluster.
* The Postgress Compute SKU and Postgress Storage Performance Tier for IOPS can now be configured in the Initialization flow.

### Release date: March 3, 2025

The initial release of Mendix on Azure provides a simplified, integrated way to deploy Mendix applications to a Microsoft Azure environment. For more information about the available features, see [Mendix on Azure](/developerportal/deploy/mendix-on-azure/).
