---
title: "User Metering"
url: /developerportal/deploy/user-metering/
weight: 20
description: "This document describes how user metering works."
---

## Introduction

End-user metering is the process Mendix uses to determine the number and type of users accessing applications in accordance with license agreements. Proper user classification ensures accurate reporting, optimal licensing costs, and transparency for both customers and Mendix. Customers can access usage reports through the Control Center application on the Mendix Platform.

{{% alert color="info" %}}
End-user metering is currently available for applications deployed to Mendix Cloud and Mendix Cloud dedicated environments.
{{% /alert %}}

Mendix licenses include user-based pricing plans. With user-based pricing, customers purchase licenses based on the number of users who need access to their Mendix applications. Customers can purchase user licenses in the following categories:

* Multi-App Internal User
* Single-App Internal User
* External User

For more information, refer to the [User Types and Definitions](/developerportal/deploy/licensing-apps-outside-mxcloud/#user-types-and-definitions) section of the *Licensing apps*.

### Key Features

* Automatic tracking – User consumption is automatically tracked from the moment your app is deployed to production and becomes functional, ensuring real-time tracking. 
* Monthly reporting – Usage data is collected regularly, processed monthly, and is available in the Control Center.
* Application-level visibility – you get the detailed insights into named user counts for each application, helping you to identify optimization opportunities
* License type classification – Users are classified by license type as External, Multi-App Internal, or Single-App Internal. This classification gives you accurate license tracking and helps optimize licensing costs. 
* Historical data – User metering provides you with access to usage data for all months since user metering was enabled.

## How User Metering Works

User metering in the Mendix cloud operates through a three-stage automated process, and it is enabled by default for apps deployed in Mendix cloud environments. 

### Data Collection

Once users are classified as `internal` or `external`, all applications on the Mendix cloud and Mendix cloud dedicated automatically send user data back to the Mendix platform. The data is collected throughout the month only from the production environment. PII information (username and other user identifiers) is hashed at source before transmission to ensure data privacy.

### Data Aggregation and Deduplication

At the end of each month, the Mendix Platform aggregates the collected data. Users are counted and rolled up to the app portfolio level. This process is detailed in the [User Aggregation and Deduplication]() section below.

### User Classification and Reporting

Users are thereafter automatically classified in the following sequence:

1. External Users
2. Single-App Users
3. Multi-App Internal Users (default)

End-of-month usage reports are generated at the beginning of every month and made available via the Control Center dashboard. Reports are generally available on the 1st of each month for the previous month's usage.

## How User Aggregation and Deduplication Work

The user aggregation and deduplication process determines which user pack is consumed when a user accesses one or more of your applications. The process evaluates users in a sequence so that each user is counted according to the correct license pack without duplication. The classification follows the steps below:

### Classifying External Users

The first step is to determine whether a user is an external user:

* If the customer has a valid External User Pack subscription, and
* The user is explicitly marked as "External" within the application

Then the User is classified as an External User.

Once classified, the user is licensed under the External User Pack and excluded from further classification steps. For more information, see the [User classification]() section of *Implementing Metering*.

All remaining users are classified as internal Users and further classified as described in the sections below.

### Classifying Single-App Internal Users

After external users are classified, the classification process further classifies the single-app internal users.

If the application is associated with a Single-App Internal User Pack, the user of the app will be classified as a single-app internal user. This user will be counted against the single-app internal user pack for that application.

For more details on how to assign single-app user packs to your apps, refer to the [Assigning Single-App Internal User Packs](to be linked) section of the Control Center.

### Classifying Multi-App Internal Users

After external users and single-app internal users have been identified, any remaining internal users are classified as multi-app internal users.
These users are licensed under the multi-app internal user pack, and no further action is required from your side.
