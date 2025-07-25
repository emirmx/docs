---
title: "Scoring Criteria Tab"
linktitle: "Scoring Criteria Tab"
url: /control-center/scoring-criteria-tab/
description: "Describes the Scoring Criteria tab on the Software Composition page of the Mendix Control Center."
weight: 3
---

## Introduction

The **Scoring Criteria** tab allows you to adjust the conditions and severity for each type of finding.     
A finding represents a vulnerability identified in the components of an app.     
Scoring criteria reflect your company's risk preference.

The settings on this tab determine how each such vulnerability is calculated for apps, environments, and components.

{{< figure src="/attachments/control-center/security/software-composition/scoring_criteria.png" >}}

The default values are strict, but you can adjust them to reflect the practice of your company.

## Finding Types

The types of findings that you can adjust for are **Outdated** and **Deprecated**.

### Outdated

A finding is generated when a component becomes outdated, meaning when a new runtime compatible version is published on the Mendix Marketplace.    

For example, if you set a **Low** severity to 30 days, and if a new version of the component is published to the Marketplace, then the team must update this component to the new version within 30 days. Otherwise, it will have a low severity finding. 

There are two ways to adjust the scoring criteria:

* Change the condition — You can increase the number of days so the teams have more time to update their apps.
* Turn off the finding — You can turn off lower severity levels to allow teams to focus on higher severity findings.

### Deprecated

A finding is generated when a component has been labeled as deprecated in the Mendix Marketplace.

You can choose the severity level for deprecated components. For example, if your company views deprecated components as a critical risk, then set the **Critical** severity for this finding.

## Tab Details

This is the information available on the **Scoring Criteria** tab:

* **Finding Type** — The type of vulnerability that components can have, along with its definition. This can be **Outdated** or **Deprecated**.
* **Severity** — The severity level of a finding. These are color-coded for easy identification throughout **Software Composition**, and can be:

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