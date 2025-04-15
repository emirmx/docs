---
title: "SALT License"
url: /deployment/salt
weight: 95
description: Describes how to deploy a Mendix application with a SALT license.
---
# Introduction

When purchasing Mendix, some customers receive a SALT (Siemens Advanced Licensing Technology) license. This guide provides instructions on how to deploy a Mendix application using a SALT license.

# Prerequisites

* After purchasing, you will receive an email containing the SALT license file.
* SALT licenses are compatible with Mendix version 10.24 and above.
* SALT licenses cannot be used in the Mendix Public Cloud.

# License Server

To use a SALT license, you need to install the Siemens License Server in the environment where your Mendix applications are deployed. Ensure that all Mendix applications can access the license server. The license server can be downloaded from Siemens Support.
For detailed instructions on installing and configuring the license server, please refer to the Siemens knowledge base

# License Provisioning

Licenses are provisioned to your Mendix applications via the license server.

# Application Configuration

After deploying the license server, configure each Mendix application that uses a SALT license by setting the runtime parameter License.SaltLicenseLocation to port@host, where port is the port number chosen during the license server installation, and host is the hostname or IP address of the license server. Instructions on how to set runtime parameters can be found here.
