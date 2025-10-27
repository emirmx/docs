---
title: "Private Connectivity"
linktitle: "Private Connectivity"
url: /control-center/private-connectivity/
description: "Describes the Private Connectivity section in the Mendix Control Center."
weight: 30
---

## Introduction
On the **Private Connectivity** page, you view and manage your company's Private Connectivity assets: networks, agents, resources and connections.

## Use Cases
Many applications running on Mendix Cloud have integrations with external resources, such as database, services and other applications. Some of these resources are public, accessible via the public internet. Other are running on your internal networks, either on-premises or on cloud infrastructure (PaaS/SaaS). You can connect to these resources over the public internet as well, [securing](/developerportal/deploy/securing-outgoing-connections-from-your-application/) them with HTTPS, a reverse proxy and client certificates. You will have to expose your internal resource to public internet in this scenario. Not all of our customers want to or are able to do this. This could be due to security, compliance or legacy reasons.

Mendix Cloud Private Connectivity can help you with connecting your applications on Mendix Cloud to your internal resources (on-premises and in the cloud) securely and privately. That means that the connection will not go over the public internet, but through a private tunnel.
Mendix Cloud Private Connectivity results in a private tunnel between your applications on Mendix Cloud and your own infrastructure. This can be an on-premises datacenter or infrastructure in the cloud, for example on AWS, Azure or GCP. You can also connect multiple networks running on different infrastructure. Through the tunnel, you can connect from your applications on Mendix Cloud to resources running on your own infrastructure. Mendix Cloud Private Connectivity only supports outgoing connections, that is, connections that are initiated from your applications on Mendix Cloud towards resources on your own infrastructure. You won't be able to connect to your applications on Mendix Cloud from an external client over the private tunnel.

Using Private Connectivity, you can retrieve data from an Microsoft SQL database on Azure into your Mendix application. You could also connect to a Kafka broker running on your own AWS account. Or connect to an SAP system running on your on-premises data center. All securely and privately, without exposing these internal resources to the public internet.

## Tailscale {#private-connectivity-tailscale}
Mendix partners with [Tailscale](https://tailscale.com) to offer Private Connectivity. Tailscale is a recognized leader in secure networking. Tailscale provides a secure, private mesh network solution built on the high-performance and modern cryptography of the WireGuard® protocol. Mendix will create all the assets required to create a private connection for our customers on the Tailscale platform. Neither Tailscale nor Mendix can access the data that is sent over the Tailscale network. All traffic is [encrypted](https://tailscale.com/kb/1504/encryption) end-to-end, with separate keys and public key infrastructure for each network.

## Architecture {#private-connectivity-architecture}
Mendix has an enterprise account with Tailscale. Within our Tailscale account, we will create _networks_ for each of our customers. Each network is dedicated to one customer. You can have multiple networks, for example to isolate their production traffic from your non-production traffic. 
Next, you must install _agents_ in your own infrastructure. These agents are connectivity tools that initiate an outgoing connection to the network created for you. Agents require authentication keys that are managed on the Mendix platform. Agents can only connect to the network their authentication key is linked to. Agents can be installed directly into the network hosting the resources you want to connect to. Or they can be installed in a separate network from where they have access to the resources. An agent can connect to only one network, but you can install multiple agents that connect to the same network. For example, you can install an agent in your on-premises data center and another agent in your AWS account, so that your applications on Mendix Cloud can connect to resources on both infrastructures.
Once agents are installed, you must expose _resources_ through your agents. These resources are subnets of network. They will be available through the agent (not the public internet). Resources exposed via agents must always first be enabled on the Mendix platform, before they can be connected to from your applications on Mendix Cloud. This gives you full control over what resources are accessible.
When resources are exposed and enabled, you can add _connections_. Connections link a specific application environment to a specific resource. Connections must be requested and only if they are approved, an application on Mendix Cloud can connect to the resource on the other side of the connection. This four-eye principle allows for governance over your connections, giving you full control over what application environment can access what resource. Approved connections can be disabled at any time, retracting the access from the application environment to the resource. You can add multiple connections for each application environment, giving them access to resources on your on on-premises data center as well as resources on your AWS account, for example.
Mendix will install a Tailscale agent in the application container of each application environment with one or more approved connections. As the Tailscale agent is running inside the application container, only that application can access your network and approved connections, ensuring other applications don't have access.

## Frequently Asked Questions {#private-connectivity-faq}

### Does Mendix or Tailscale have access to my data?
No. All data going over the Tailscale network is [encrypted](https://tailscale.com/kb/1504/encryption) end-to-end, with separate keys and public key infrastructure for each network. Neither Tailscale nor Mendix can access the data that is sent over the Tailscale network. 

### Is Tailscale SOC2-compliant?
Yes, [Tailscale has completed a SOC 2 Type II certification](https://tailscale.com/security).

### Do I need to sign up for my own Tailscale account?
No, you don't need to sign up for a Tailscale account yourself. All assets required for Mendix Cloud Private Connectivity will be created within Mendix' Tailscale account. This is similar how we create all resources required to run your applications on Mendix Cloud in Mendix' AWS account.

### Can I connect my existing Tailscale networks?
At this time, it is not possible to connect to an existing Tailnet if you're existing Tailscale customer.

## Resources

For information on how to configure and use Mendix Private Connectivity, refer to [Configuring and Using Private Connectivity](/control-center/configure-private-connectivity/).