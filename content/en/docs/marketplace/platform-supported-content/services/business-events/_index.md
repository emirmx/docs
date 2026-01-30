---
title: "Mendix Business Events"
url: /appstore/services/business-events/
description: "Describes the Mendix Business Events service, which is available in the Mendix Marketplace."
aliases:
    - /appstore/modules/business-events/
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Business events are like a mailing list to share event notifications between apps. The key difference between business events and traditional communication between apps, like REST or web services, is that there is no direct communication between the different apps.

With [Mendix Business Events](https://marketplace.mendix.com/link/component/202649), applications can signal when something important happens and can independently subscribe to these events if they want to be informed.

To deliver these events reliably between your applications, an event broker is required. For apps running Mendix Cloud on licensed nodes, you will need to purchase a license for the [Mendix Event Broker](/appstore/services/event-broker/).

{{% alert color="info" %}}
Business events are supported in Studio Pro [9.18](/releasenotes/studio-pro/9.18/) and above and currently can only be deployed to the [Mendix Cloud](/developerportal/deploy/mendix-cloud-deploy/).{{% /alert %}}

### Typical Use Cases

Business events help you automate the resulting actions when something happens in your organization. They can be useful in a variety of situations, such as: 

* Uploading a payment receipt in one app, while another app processes the outgoing payment in the company's ledger
* Making an appointment with a service provider in an appointment app, then needing it to be added to the scheduling app of the service provider
* Customers placing an order in a web shop, and other apps need to take follow-up actions like scheduling shipments, sending an invoice, and reordering inventory stock

### Prerequisites

To use Mendix Business Events, you will need the following:

* The [Mendix Business Events](https://marketplace.mendix.com/link/component/202649) service from the Mendix Marketplace
* Studio Pro [9.24](/releasenotes/studio-pro/9.24/) and above
* An event broker; this can be a licensed [Mendix Event Broker](#mendix-event-broker) for apps running in Mendix Cloud or the [local testing](#local-testing) broker (see [Deployment](#deployment))
* [Docker](https://www.docker.com/) for local deployment

## Licensing {#licensing}

The Mendix Business Events service itself does not require a license, but it depends on an event broker to deploy to production environments. You can purchase a [Mendix Event Broker License](/appstore/services/event-broker/#event-broker-license) for a broker to be set up for you. See the [Mendix Event Broker](https://marketplace.mendix.com/link/component/202907) platform service page for more details. You can also run business events on [your own Kafka cluster](#byok).