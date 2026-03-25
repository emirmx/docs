---
title: "Portable App Distribution"
url: /developerportal/deploy/portable-app-distribution/
weight: 15
description: "Describes the Portable App Distribution deployment method for Mendix apps."
---

## Introduction

Portable App Distribution provides a flexible and straightforward method for isolated server-based deployments. By enabling customers to generate a bundled artifact packaged as a .zip file and run it directly, Portable App Distribution simplifies the deployment process.

## Benefits

Portable App Distribution offers the following benefits:

* Simplified deployment - Portable apps eliminate complex installation procedures, making it easier and faster to get software up and running across different machines. This reduces setup time and potential configuration errors.
* Enhanced consistency - By bundling all dependencies, portable apps ensure a consistent operating environment for the application, regardless of the underlying system configuration.
* Improved mobility and flexibility - Teams can easily move applications between workstations, virtual machines, or even cloud instances without the need for reinstallation, fostering greater agility in project work.
* Reduced system impact - Portable apps often run in isolated environments, which can help prevent conflicts with other installed software and maintain system stability.
* Streamlined updates - Managing updates can be more straightforward, as new versions of a portable application can often be deployed by simply replacing the package.
* Layered configuration - Portable App Distribution supports defining base configurations that can be extended with environment-specific or deployment-type-specific entries (for example, distinct configurations for development, testing, or production environments).

## Licensing

You can test Portable App Distribution on a [Free App](/developerportal/deploy/mendix-cloud-deploy/#free-app). For more information about Free Apps and their limitations, as well as licensing apps outside of the Mendix Cloud, see [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/).

To license a Mendix app on the Portable App Distribution, add it to your configuration. For more information, see [Obtaining a Mendix License](/developerportal/deploy/licensing-apps-outside-mxcloud/#get-license).

## Prerequisites

The Portable App Distribution functionality is available for Mendix Studio Pro version 11.9, 11.6.x MTS, or above.

You must also ensure that you have the supported version of [Java Runtime Environment](/refguide/system-requirements/#java).