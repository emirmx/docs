---
title: "Network Timeout"
url: /refguide/mobile/building-efficient-mobile-apps/network-timeout/
weight: 15
description: "Configure network request timeouts in your Mendix mobile apps to handle slow network conditions."
aliases:
    - /refguide/mobile/network-timeout/
---

## Introduction

Network request timeout is a crucial configuration parameter for mobile applications that helps handle slow or unreliable network conditions. Setting appropriate timeout values ensures:

* Better app responsiveness
* Enhanced user experience
* Proper error handling during network issues

## Default Timeout

The default timeout value for network requests in Mendix mobile applications is 60000 milliseconds (60 seconds).

## Excluded Network Requests

The following operations are excluded from the configured timeout settings:

* **Data Synchronization Operations** 
    * All `synchronize_objects` calls

* **Microflow Runtime Operations**
    * All `runtimeOperation` calls

{{% alert type="info" %}}
These exclusions ensure that complex operations like data synchronization and server-side processing can complete without interference from the general timeout settings.
{{% /alert %}}
