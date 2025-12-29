---
title: "Contract Management"
url: /developerportal/deploy/contract-management/
weight: 20
description: "Understand your Mendix subscription lifecycle, contract statuses, renewal timelines, and critical actions to ensure continuous service and prevent data loss."
---

## Introduction

The Mendix Contract Management system ensures a seamless and predictable Mendix experience by keeping you fully informed of contract statuses and maintaining data portability throughout your Mendix subscription lifecycle.

This document provides detailed information about the status of your Mendix application contracts, critical timelines, and the actions required to guarantee continuous access to your Mendix applications and services.

## Mendix Subscription Lifecycle

Your Mendix subscription is categorized into three main statuses: Active, Expiring, and Expired, based on your contract's expiry date.

| Account Status | Timeline | Impact |
|---------------|----------|--------------------|
| **Active (Green)** | Until 30 days before contract expiry | Your account is fully active, with all services available and application runtimes functioning normally. This is the optimal state for uninterrupted Mendix usage. |
| **Expiring (Orange)** | From 30 days before contract expiry until contract end date | All services continue to function normally. You receive email reminders, notifications, and platform banner alerts about your upcoming renewal. |
| **Expired (Red)** | From the day after contract expiry up to 60 days | Your account and applications are downgraded to **Unlicensed** mode, revoking platform features such as **Control Center**. You can still deploy and manually restart apps, but they will automatically shut down after 2-4 hours and are limited to 6 concurrent users.|

## Contract Timeline and Milestones

The Mendix Platform provides proactive communication to simplify the renewal process. The following timeline outlines key timelines and required actions:

### Before Contract Expiry

These milestones provide ample opportunity to discuss renewal options and take necessary steps to ensure contract continuity with no access or data loss.

#### 90 Days Before Contract Expiry

* **Email notification** – You receive email providing information about your upcoming contract renewal
* **Action required** – Contact your (Partner) Account Team to discuss renewal options and next steps. Early engagement allows for a smoother process

#### 30 Days Before Contract Expiry

* **Status change** – Your Mendix account transitions to Expiring (Orange) status
* **Platform notifications** – Users see renewal countdown banners on the platform
* **Email reminders** – You receive reminder emails if renewal requirements have not been met
* **Administrator notifications** – Mendix Administrators and Technical Contacts receive offboarding reminders and data download instructions

{{% alert color="info" %}}
While Mendix services continue to function normally, this is the critical period to initiate renewal discussions.
{{% /alert %}}

#### 15 Days Before Contract Expiry

* **Impact warning emails** – You receive automated emails detailing the consequences of non-renewal
* **Tailored instructions** – Mendix Administrators and Technical Contacts receive offboarding instructions

#### 7 Days Before Contract Expiry

* **Final impact emails** – You receive final detailed explanations about the impact of non-renewal
* **Data offboarding instructions** – Administrators and Technical Contacts receive tailored instructions for offboarding application data

{{% alert color="warning" %}}
This is your final opportunity to renew your contract and avoid service disruption.
{{% /alert %}}

### Post Contract Expiry

If your contract is not renewed by the expiry date, your account transitions to Expired status with the following consequences:

#### 1 Day After Contract Expiry

* **Platform banner** – All users see a banner clearly stating that the contract has expired
* **Restricted access** – Account access is downgraded to **Unlicensed** mode
* **Feature limitations** – Access to platform features like **Control Center** is revoked
* **Deployment restrictions** – You can no longer deploy new versions of your applications on Mendix Cloud
* **Deployment restrictions** – Application data is available for download for the next 30 days

#### 30 Days After Contract Expiry

* **Access revoked**:
    * Customer applications are no longer accessible
    * No end user access

#### 60 Days After Contract Expiry

Your Mendix Cloud application data is permanently deleted.

{{% alert color="warning" %}}
Application data deletion is irreversible. Make sure to download all necessary application data before this deadline if you do not intend to renew your contract.
{{% /alert %}}

### Quick Reference Timeline

| **Days Before Expiry** | **Key Actions** | **Status** |
|------------------------|-----------------|------------|
| **90 days** | Contact Account Team for renewal | Active |
| **30 days** | Finalize renewal discussions | Expiring |
| **15 days** | Download data if not renewing | Expiring |
| **7 days** | Last chance to renew | Expiring |
| **0 days (Expired)** | Contract expires | Expired |
| **+1 day** | Limited access, no deployments | Expired |
| **+30 days** | Apps inaccessible to end users | Expired |
| **+60 days** | Data permanently deleted | Expired |

## Steps to Guarantee Continuous Service

To prevent any disruptions to your Mendix services and avoid data loss, follow these essential steps:

* **Engage your account team early** – Proactively contact your (Partner) Account Team to discuss renewal options and finalize agreements well in advance of your contract expiry date
* **Complete the contract renewal process** – Ensure that the renewal process is fully completed and confirmed before your current contract expires
* **Review and upgrade regularly** – Review Mendix Terms regularly and plan to upgrade annually to remain aligned with End-of-Life standards and benefit from the latest platform enhancements and security updates

## Read More

* [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/)