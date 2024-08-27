---
title: "Getting Started with Mendix on Azure"
url: /developerportal/deploy/mendix-on-azure/quickstart/
description: "Documents the pre-implementation tasks for Mendix on Azure."
weight: 20
---

## Introduction

This document provides a comprehensive guide for 

### Prerequisites

Before starting the installation and implementation process, make sure that you have all the necessary prerequisites:

* 

Create managed application, select Standard plan

Link your Azure account

Azure clusters shown in mx portal

Create a managed azure application

In the managed application, the resource group is created

Resource group has all the resources that must be initialized

Go back to Mx on Azure portal by clicking a button in Azure

New cluster visible under initializable clusters

Initialize cluster, first a preflight check to see if resources can be registered in the cluster; virtual images used to host mendix app, so the check verifies if the required type of virtual image is available in the cluster; list of providers visible in portal, Nidhi also will provide additional specs

Select the service tier, Free is the default but any can be chosen, higher tier=higher cost

Click Initialize, in about 15 minutes the resources are provided

Creates a resource group in the managed app

Advanced Options

Namespace and cluster automatically created in private cloud portal and you can create environments as usual

you can't delete the cluster, only from the azure portal

Clyde to share doc about limitations