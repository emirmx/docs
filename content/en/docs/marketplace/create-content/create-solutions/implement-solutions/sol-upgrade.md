---
title: "Upgrading Solutions"
url: /appstore/creating-content/sol-upgrade
linktitle: "Upgrading Solutions"
weight: 10
description: "Describes how to upgrade a properly set up solution with a new version provided by the ISV."
---

## Introduction

Upgrading a solution is the process of merging changes provided by the ISV into a new version of an adaptable solution.

## Prerequisites

To upgrade a Mendix solution, make sure the following prerequisites are met:

* Make sure you are using the correct Studio Pro version.    
  A solution can only be set up with the Studio Pro version that exactly matches the version used to create it. For example, if Studio Pro 10.0 was used to create the solution, only that version can be used to set up the solution.
* Your version control system is Git.
* Your app is currently on the **main** branch line.
* The **solution-releases** branch line exists.
* There are no uncommitted changes or unresolved conflicts in your app.
* There is only one MPR file in the solution package.
* The name of your app's MPR file is the same as the name of the MPR file in the solution package.
* The solution you are upgrading to is the same solution that was used to set up your adaptable solution.

{{% alert color="info" %}}You can consider skipping versions when upgrading. For example, if you set up your solution with v1 and the ISV then released v2 and v3, it is not necessary for you to upgrade versions one by one. You can go directly from v1 to v3 if there were no data migration changes in v2.{{% /alert %}}

## Upgrading Process

To upgrade a solution, follow these steps:

1. Open Studio Pro and click **File** > **Upgrade Solution**.

    {{< figure src="/attachments/appstore/create-content/implement-solutions/solution-upgrade.png" alt="Upgrade Solution" class="no-border" >}}

    {{% alert color="info" %}}In Studio Pro 9 and below, this option must be enabled by setting a feature flag. Since Studio Pro 10, it is available for general use, and no longer hidden behind a flag.{{% /alert %}}

2. Select the solution package file (*.mxsolution*) provided by the ISV and click **OK**.
3. Once the solution upgrade is completed, a new commit to the **solution-releases** branch line is created. This commit contains the unchanged new version of the solution, as provided by the ISV. You cannot make any changes in this branch, as that would render the solution incompatible with upgrades, or lead to unpredictable errors during upgrades.

### Read More

* [Setting Up Solutions](/appstore/creating-content/sol-set-up/) 
