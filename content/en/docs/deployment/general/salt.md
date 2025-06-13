---
title: "Siemens Advanced License Technology"
url: /developerportal/deploy/salt/
weight: 90
description: "This guide explains how to use Siemens Advaned License Technology (SALT) with Mendix"
#To update these screenshots, you can log in with credentials detailed in How to Update Screenshots Using Team Apps.
---

{{% alert color="info" %}}
SALT-based licenses are issued only to selected customers. If you received an email explicitly referencing a SALT license and directing you to this page, follow this guide. Otherwise, refer to the [guide for non-SALT Mendix licenses](/developerportal/deploy/licensing-apps-outside-mxcloud/).
{{% /alert %}}

## Introduction

Siemens Advanced License Technology (SALT) is a Siemens service used to validate software licenses. This guide outlines the steps to deploy a Mendix application using a SALT-based license.

## Prerequisites

Upon purchase, you will receive an email containing your SALT-based license file. Please note the following limitations when using a SALT-based license:

* Version compatibility - SALT licenses are supported only for Mendix version 10.24 and newer.
* Version binding - Each SALT license is bound to a specific major version of Mendix and cannot be used in newer major versions.
* Cloud restrictions - SALT licenses are not supported for Mendix Cloud environments.

## Siemens License Server

To validate SALT-based licenses, the Siemens License Server is required. This server must be installed within the same environment where your Mendix applications are deployed. All Mendix applications utilizing a SALT license must be able to access the license server.

The license server must also have access to your SALT license file, which is used to validate the Mendix application licenses at runtime.

### Downloading

You can download the Siemens License Server (SLS) from the [Siemens Support Portal](https://support.sw.siemens.com/en-US/product/1586485382).

### Installation

Please review [this video](https://support.sw.siemens.com/en-US/knowledge-base/MG616411) to learn how to install the Siemens License Server.

During the installation, you will be prompted to provide your license file. You can download the license file by following this [guide from Siemens](https://support.sw.siemens.com/en-US/product/1586485382/knowledge-base/MG612613).

## Mendix Application Configuration

Each Mendix application that uses a SALT license must be configured with the following [runtime setting](/refguide/custom-settings/):

```
License.SaltLicenseLocation = port@host
```

* `port`: The port number specified during the license server installation.
* `host`: The hostname or IP address of the machine running the license server.

After configuring the runtime setting and starting the Mendix application, it will attempt to connect to the Siemens License Server to validate the SALT license.
