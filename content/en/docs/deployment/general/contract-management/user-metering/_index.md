---
title: "User Metering"
url: /developerportal/deploy/user-metering/
weight: 20
description: "This document describes how user metering works."
---

## Introduction

End-user metering is the process Mendix uses to determine the number and type of users accessing applications in accordance with license agreements. Proper user classification ensures accurate reporting and optimal licensing costs for customers. <!-- *Proper user classification ensures accurate reporting, optimal licensing costs and transparency for both customers and Mendix. Customers can access Usage Report through the Control Center application on the Mendix Platform*. Link Usage Report from the Control Center doc -->

{{% alert color="info" %}}
End-user metering is currently applied to applications deployed to Mendix Cloud and Mendix Cloud Dedicated environments.
{{% /alert %}}

Mendix licenses include user-based pricing plans. With user-based pricing, customers purchase licenses based on the number of users who need access to their Mendix applications. Customers can purchase user licenses in the following categories:

* Multi-App Internal User
* Single-App Internal User
* External User

For more information, refer to the [User Types and Definitions](/developerportal/deploy/licensing-apps-outside-mxcloud/#user-types-and-definitions) section of *Licensing apps*.

### Key Features

* Automatic tracking – User consumption is automatically tracked from the moment your app is deployed to production and becomes functional, ensuring real-time tracking. 
* Monthly reporting – Usage data is collected regularly, processed monthly, and deduplicated.  <!-- Monthly reporting – Usage data is collected regularly, processed monthly, and is available in the Control Center. -->
* License type classification – Users are classified by license type as External, Multi-App Internal, or Single-App Internal. This classification yields accurate license tracking and helps optimize licensing costs. 

## How User Metering Works

User metering in the Mendix cloud operates through a four-stage automated process, and it is enabled by default for apps deployed in Mendix Cloud environments. 

### In-App User Classification

Your application logic plays an important role in creating and maintaining accurate user records. A key component of in-app user classification is maintaining a consistent user identifier. This becomes especially important when customers are using multi-app licenses.  In the Mendix multi-app license pack, the same individual may access multiple apps under a single license agreement. A unique, persistent user identifier ensures accurate tracking and prevents duplication. The application logic is responsible for classifying users as `Internal` or `External`. For more detailed information, refer to the [User Classification](/developerportal/deploy/implementing-user-metering/#user-classification). 

### Data Collection

All applications on the Mendix Cloud and Mendix Cloud Dedicated automatically send user data back to the Mendix platform. Data is collected from all environments throughout the month. However, only data from the production environment is considered for consumption numbers and billing purpose. PII information (username and other user identifiers) is hashed at source before transmission to ensure data privacy.

### Data Aggregation and Deduplication

At the end of each month, the Mendix Platform aggregates the collected data. Users are counted and rolled up to the app portfolio level. This process is detailed in the [User Aggregation and Deduplication](/developerportal/deploy/user-metering/#how-user-aggregation-and-deduplication-work) section below.

### User Classification and Reporting

Users are thereafter automatically classified in the following user licensing buckets based on your user licenses:

1. External Users
2. Single-App Internal Users
3. Multi-App Internal Users (default)

End-of-month usage reports are generated at the beginning of each month and are made available via the Control Center dashboard. The reports are generally available on the 1st of each month and reflect the previous month's usage.

## How Deduplication and User Classification Work

The user classification and deduplication process determines which user pack is consumed when a user accesses one or more of your applications. The process evaluates users in a sequence so that each user is counted according to the correct license pack without duplication. The classification follows the steps below:

### User Identification (Deduplication)

Users are deduplicated based on common identifier values. A customer who has different identifier values in the aggregated data cannot be recognized as same user, and will be counted as multiple users. The deduplication mechanism evaluates two user attributes. When different values persist, Mendix treats them as different users. For more information, refer to [Guidelines for Unique User Identification (Deduplication)](/developerportal/deploy/implementing-user-metering/#guidelines-for-unique-user-identification-deduplication).

### Classifying External Users

The first step is to determine whether a user is an external user:

* If the customer has a valid External User Pack subscription, and
* The user is explicitly marked as `External` within the application.

Then the User is classified as an External user.

Once classified, the user is licensed under the External User Pack and excluded from further classification steps. For more information, see the [User classification](/developerportal/deploy/implementing-user-metering/#user-classification) section of *Implementing Metering*.

All remaining users are classified as `Internal` Users and further classified as described in the sections below.

### Classifying Single-App Internal Users

After `External` users are classified, the classification process further classifies the single-app internal users.

If the application is associated with a Single-App Internal User Pack, the user of the app will be classified as a single-app internal user. This user will be counted against the single-app internal user pack for that application.
 <!-- *For more details on how to assign single-app user packs to your apps, refer to the Assigning Single-App Internal User Packs section of the Control Center.* Link from the Control Center doc -->

 {{% alert color="info" %}}
An internal user accessing multiple apps, one of which is covered under a Single-App Internal User Pack, will be counted as a single-app internal user for that app and will be also be counted separately for any other apps they use. 
{{% /alert %}}

### Classifying Multi-App Internal Users

After external users and single-app internal users have been identified, any remaining internal users are classified as multi-app internal users.
These users are licensed under the multi-app internal user pack, and no further action is required from your side.

## Read More

* [Licensing Apps](developerportal/deploy/licensing-apps-outside-mxcloud/)
