---
title: "Configuring External Secret Management with AWS Secret Manager"
url: /private-mendix-platform/configure-aws-secret-manager/
description: "Documents the configuration of AWS Secret Manager for the Private Mendix Platform."
weight: 40
---

## Introduction

The Private Mendix Platform offers enhanced security and flexibility for credential management by supporting [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) as an external secret management solution, alongside the traditional database storage option. In the legacy database storage approach, the credentials are encrypted and stored directly in the Private Mendix Platform database. With AWS Secrets Manager, credentials are instead stored in AWS Secrets Manager and accessed securely via IAM roles for improved security, centralized management, and compliance with enterprise security policies. This document describes how you can configure AWS Secrets Manager integration for your Private Mendix Platform project.

## Prerequisites

Before configuring AWS Secrets Manager integration, prepare the following:

* An AWS account with appropriate permissions to create and manage secrets
* IAM permissions to create roles and policies for AWS Secrets Manager access
* Access to the PPrivate Mendix Platform project admin panel with administrative privileges
* Basic knowledge of AWS services, IAM roles, and Kubernetes (if using EKS deployment)
* An existing EKS cluster (if your PMP deployment runs on Kubernetes)