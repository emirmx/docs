---
title: "Private Connectivity Best Practices"
url: /control-center/private-connectivity/best-practices/
description: "Provides best practices for configuring and using Private Connectivity in Mendix Control Center."
weight: 2
beta: true
---

{{% alert color="warning" %}}
Private Connectivity is currently in Public Beta, and will be out of Public Beta on May 31, 2026. After that date, the service will no longer be available, and Mendix will start preparing it for General Availability (GA). Check the [Feature Release Calendar](/releasenotes/feature-release-calendar/) for details on the GA release date.
{{% /alert %}}

## Introduction

This page provides best practices for configuring and using Private Connectivity networks, agents, and resources. Following these guidelines helps ensure secure, efficient, and maintainable connections between your Mendix apps and internal infrastructure.

## Authentication Key Security 

Creating an agent involves creating an authentication key. Agents registered with that authentication key join the agent's network. If you have a production network, only use the generated authentication key for agents placed in your production network. Apply the same principle for development networks.

## When to Create Networks 

Create a new network only when you have a clear need for isolated networks. Examples include:

* Environment isolation – Separate production traffic from non-production traffic.
* Organizational separation – Different business units or regional offices, such as separate networks for APAC and EMEA teams.

The crucial aspect in each network is agent placement. If you create a network for development use only, place agents only in the development environment of your organization's network.

## Working with Agents 

Agents determine what parts of your infrastructure are accessible. While creating separate networks can help organize your apps on the Mendix side, agents and their placement ultimately determine what infrastructure is accessible.

### Where to Place Agents 

An agent forwards traffic to the advertised subnets. Place it where it can access those addresses:

* AWS – Place the agent in your VPC.
* Azure – Place the agent in your VNet.
* On-premises – Place the agent where it has network access to the target resources.

### Infrastructure Requirements 

Install agents on infrastructure that has direct network reachability to the resources you want to advertise. Typically, a virtual machine (VM) or other host inside the relevant VPC or VNet is the best fit.

Tailscale supports both Linux and Windows. For the best results and performance, use Linux due to its [kernel integration](https://tailscale.com/docs/reference/kernel-vs-userspace-routers). 

For recommendations on operating system tweaks and machine sizing for the main cloud providers, refer to [Tailscale's performance best practices](https://tailscale.com/docs/reference/best-practices/performance#operating-system-recommendations).

Suitable infrastructure for agents includes EC2 instances, VMs, and containers with privileged access on EKS or AKS.

### How Many Agents Do You Need? 

It is more efficient to use one agent with multiple advertised subnets. You can advertise as wide a CIDR range as you need, or as many subnets as necessary on one agent. Using one agent requires less effort to install and is cheaper in both time and maintenance cost.

However, you need to consider these trade-offs:

* If you use one agent to expose multiple resources and it suffers an outage or requires maintenance, you may affect more Mendix apps than desired.
* Consider grouping agents by function, advertising only the routes necessary for a particular Mendix app, and creating separate agents for other apps.

### High Availability 

Subnet routers support high availability. To set this up, create two or more agents that advertise the same routes. For example, if you set up two routers that both advertise `10.0.0.0/16`, the first agent acts as the primary and the second as the failover.

This setup provides a simple active/passive failover, not load balancing. When Tailscale detects that your primary subnet router is offline, routing immediately switches to the failover device.

For more information, refer to [Subnet Router High Availability](https://tailscale.com/docs/how-to/set-up-high-availability#subnet-router-high-availability).

## Using Containers

Containerized platforms are common, and you may want to keep your agent deployment consistent with the rest of your infrastructure. Tailscale can run in containers, but there are some important considerations.

### State Persistence 

When a Tailscale container starts, it generates a node key to identify itself and register with the Tailscale control plane. If that key is not persisted, every container restart generates a new key and attempts to register as a new device. Since authentication keys created via the Mendix wizard are single-use, this causes authentication failures. You must persist the state of the container so it remembers itself after each restart.

Use these parameters to avoid authentication failure issues:

* Use the `TS_AUTHKEY` parameter to supply the authentication key to your container. For more information, refer to [TS_AUTHKEY](https://tailscale.com/docs/features/containers/docker/docker-params#ts_authkey).
* Use the `TS_AUTH_ONCE` parameter to ensure the container only attempts authentication once. This helps avoid *API key not found* errors on container restarts. For more information, refer to [TS_AUTH_ONCE](https://tailscale.com/docs/features/containers/docker/docker-params#ts_auth_once).
* Use the `TS_STATE_DIR` parameter to declare a space to save the state, then mount it as a volume. For more information, refer to [TS_STATE_DIR](https://tailscale.com/docs/features/containers/docker/docker-params#ts_state_dir).
* On Kubernetes, use the `TS_KUBE_SECRET` parameter to store the state in a secret. The value is the name you want to give that secret. If it is not set, it defaults to `tailscale`. For more information, refer to [TS_KUBE_SECRET](https://tailscale.com/docs/features/containers/docker/docker-params#ts_kube_secret).

### Privilege Requirements 

Tailscale typically runs in kernel (TUN) mode. If your environment permits privileged containers or host networking, that is the better choice for a subnet router. If it does not, you can run the subnet router in userspace mode, but this is better suited for lighter-duty use and is not the first recommendation for higher-throughput production routing.

For example, Azure Container Apps do not have privileged container access, so you need to enable userspace mode. For more information, refer to the following pages in the Tailscale documentation:

* [Docker containers](https://tailscale.com/docs/features/containers/docker)
* [Userspace networking mode](https://tailscale.com/docs/concepts/userspace-networking#start-tailscale-in-userspace-networking-mode)

## Advertised Routes

Mendix uses Tailscale subnet routers to advertise routes to your network. This gives you full control over where the agent forwards traffic within your network.

### Routes to Advertise

The routes you advertise depend on what your Mendix Cloud app needs to access and what you want to share:

* Single resource – If your app only needs to reach one specific resource, advertise that single address via a `/32` subnet mask (for example, `192.168.1.10/32`).
* App subnet – If you host all apps in one specific subnet, use the subnet CIDR (for example, `192.168.1.0/24`).
* Entire network – If you want to share the entire network to avoid repeatedly opening new routes, use the entire VPC or VNet CIDR (for example, `192.168.0.0/16`).
