---
title: "Scoring Criteria Tab"
linktitle: "Scoring Criteria Tab"
url: /control-center/scoring-criteria-tab/
description: "Describes the Scoring Criteria tab on the Software Composition page of the Mendix Control Center."
weight: 3
---

## Introduction

The **Scoring Criteria** tab allows you to adjust the conditions and severity for each type of finding. A finding is defined as a vulnerability identified in the components of an app. The settings on this tab determine how each such vulnerability is determined for apps, environments, and components.

{{< figure src="/attachments/control-center/security/software-composition/scoring_criteria.png" >}}

## Tab Details

This is the information available on the **Scoring Criteria** tab:

* **Finding Type** — The type of vulnerability that components can have, along with its definition. This can be **Outdated** or **Deprecated**.
* **Severity** — The severity level of a finding. These are color-coded for easy identification throughout Software Composition, and can be:

    * **Critical**
    * **High**
    * **Medium**
    * **Low**

  For outdated components, you can adjust all severity levels.    
  For deprecated components, you can choose which severity level to assign.

* **Conditions** — Configure the minimum number of days that the condition must be true for the finding to become applicable.    
  For example, you can set a value of 100 for the **Critical** severity level if you want components that have been outdated for 100 days to be marked as **Critical**.
* **Status** — Toggle the selector on or off depending on which levels of severity you want to see.    
  For example, if you are only interested in **Critical** findings, you can toggle off all the rest.