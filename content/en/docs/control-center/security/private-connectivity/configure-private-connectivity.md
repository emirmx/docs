---
title: "Configuring and Using Private Connectivity"
linktitle: "Configuring and Using Private Connectivity"
url: /control-center/configure-private-connectivity/
description: "Describes the configuration steps of Private Connectivity in the Mendix Control Center."
weight: 1
---
## Introduction
To connect from an application on Mendix Cloud to a resource on your internal network, you will have to go though several steps:
* Add a network - This is done on the Mendix platform
* Add an agent- This is done on the Mendix platform
* Install an agent- This is done on your internal infrastructure
* Run the agent- This is done on your internal infrastructure
* Expose resources - This is done on the Mendix platform
* Enable resources- This is done on the Mendix platform
* Request a connection- This is done on the Mendix platform
* Approve the connection- This is done on the Mendix platform
* Configure DNS (optional)- This is done on the Mendix platform

## Networks {#private-connectivity-networks}
Mendix Cloud Connect networks are the bridge between Mendix Cloud and your own infrastructure. You will need at least one network to be able to connect from an application on Mendix Cloud to your own infrastructure. You can create multiple networks, for example to isolate your production traffic from non-production traffic.

On the **Networks** tab of the [Private Connectivity](https://privateconnectivity.mendix.com) page in Control Center, you can see all the networks of your company. The page shows the following information for each network:
* Network - The name of the network.
* External Agents - The number of agents installed in your internal infrastructure, that are connected to the network.
* Environments - The number of application environments on Mendix Cloud that have at least one connection using the network.

### Adding a Network {#private-connectivity-networks-add}
To add a new network for your company, follow these steps:
1. Open the [Private Connectivity](https://privateconnectivity.mendix.com) page in Control Center.
2.
	* If you don't have a network yet, click **Create a Network** to launch the network wizard.
	* If you already have a network, click on **Add Network** on the **Networks** tab.
3. On the **Instructions** step of the wizard, you will find a short summray of the steps required to connect from an application on Mendix Cloud to a resource on your own network.
4. On the **Create Network** step of the wizard, provide a name for your new network. Make sure the name is descriptive and recognizable. Click **Create**.
5. You will need at least one agent for every network. On the **Add Agent** tab of the wizard, provide a name for the new agent for your new network. Make sure the name is descriptive and recognizable. Click **Add**.

Your network and agent are now added. You can continue with [installing the agent](#private-connectivity-agents-install) in your own infrastructure and [configuring the DNS](#private-connectivity-networks-dns) for your network.

### Viewing and Editing Networks {#private-connectivity-networks-details}
To view and edit an existing network, follow these steps:
1. On the **Networks** tab, find the network that you want to view the details or edit, click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) and select **Details**. 

The details of that network will be shown. This includes
* Network Name - The name you gave to the network. You can edit this. Make sure the name is descriptive and recognizable.
* Network ID - The internal ID of your network. You can copy this, for example to provide it in a support ticket if you have issues with the network.
* External Agents - A list of all external agents, running on your own internal infrastructure, that have access to the network. The status of each agent is shown as well.
* DNS Details - A list of dommains for which you have [configured DNS](#private-connectivity-networks-dns).
* Environment Details - A list of applications environments that are using the network to connect to a resource.
* Show Logs - This will show [the flow logs](https://tailscale.com/kb/1219/network-flow-logs) for your network. This can help you troubleshoot issues with connectivity on your NEtwork.

Click **Save** to save changes you made.

### Configuring DNS for your Network {#private-connectivity-networks-dns}
If your Mendix application is connecting to external resources, you probably want to do this using host names. DNS (Domain Name System) servers (nameservers) translate a host name, like `www.mendix.com` to an IP address, like `192.168.1.1`. If the DNS record with that translation is on a public nameserver, this is not a problem, as Mendix applications can access those by default. But if you want to connect to a private host name, like `mydatabase.myinternalnetwork.net`, where the DNS record for this host name is stored on a private nameserver, your Mendix application will not be able to resolve the host name to an IP address, making the host inaccessible for the Mendix application.
With Mendix Cloud Private Connectivity, you can configure your network to use restricted nameservers for specific domains. Using a restricted nameserver is also known as split DNS. If you configure an internal nameserver for a domain, for example `myinternalnetwork.net`, any DNS request for host names within that domain, for example `mydatabase.myinternalnetwork.net` will be forwarded to the configured nameserver. There, the host name will be resolved to an IP address. This will allow you to use internal host names to connect to resources on your internal infrastructure, without having to add the DNS records for those internal resources on a public DNS server.

To configure spli DNS for a new domain on your network, follow these steps:
1. Click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) for a network and select **Add DNS**.
2. On the **Edit DNS** dialog, click **Add New Domain**.
3. Provide the following information:
* Domain - Provide the domain for which the nameservers should be used, for example `myinternalnetwork.net`.
* Nameservers - Provide the IP address of the nameserver to use to resolve DNS queries for the provided domain. You can add multiple nameserver IP addresses for high-availability.

Click **Save** to save changes.

To remove split DNS for a domain on your network, follow these steps:
1. Click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) for a network and select **Add DNS**.
2. On the **Edit DNS** dialog, find the domain you want to add a nameserver for and click **Delete Domain** and then confirm that you want to delete it.

To add a nameserver for a domain you added to your network before, follow these steps:
1. Click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) for a network and select **Add DNS**.
2. On the **Edit DNS** dialog, find the domain you want to add a nameserver for and click **Add New Nameserver**.
3. Provide the following information:
* Nameservers - Provide the IP address of the nameserver to use to resolve DNS queries for the provided domain. You can add multiple nameserver IP addresses for high-availability.

Click **Save** to save changes.

To remove a nameserver for a domain you added to your network before, follow these steps:
1. Click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) for a network and select **Add DNS**.
2. On the **Edit DNS** dialog, find the domain you want to delete a nameserver for, click **Delete Nameserver** for the nameserver you want to delete and then confirm that you want to delete it.

### Deleting Networks {#private-connectivity-networks-delete}
To delete a network you have created before, follow these steps:
1. On the **Networks** tab, find the network that you want to delete, click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}), select **Delete** and then confirm that you want to delete it.

Deleting a network will delete all agents connected to that network, revoke the agents' authentication keys, remove all resources exposed through the agents and all connections to those resources. Approved connections will be broken immediately. Deleting a network will not uninstall the connected agents from your own infrastructure!

See below for instructions on [uninstalling an agent](#private-connectivity-agents-uninstall). 

## Agents {#private-connectivity-agents}
To connect your own infrastructure to your Mendix Cloud Connect network(s), you need Mendix Cloud Connect agents. You will need at least one agent to be able to connect from an application on Mendix Cloud to your own infrastructure. You can connect multiple agents to each network.

On the **Agents** tab of the [Private Connectivity](https://privateconnectivity.mendix.com) page in Control Center, you can see all the agents of your company. The page shows the following information for each agent:
* Agent - The name of the agent.
* Network -- The network the agent is connected to.
* Resources - The number of resources exposed through the agent.
* Status (last Seen) - Shows the status of the agent
    * Connected - The agent is currently connected to the network.
	* Date and time - The last time the agent was connected to the network. The agent is not connected at this time.

### Adding an Agent {#private-connectivity-agents-add}
You can only add agents if you have at least one network. See [Adding a Network]() to add a network.

To add a new agent to an existing network, follow these steps:
1. Launch the agent wizard by choosing one of the following options:
    * Select a network to which you want to add an agent by clicking the ***More Options** ({{< icon name="three-dots-menu-horizontal" >}}) and selecting **Add Agent** on the **Networks** tab.
    * Click **Add Agent** on gents** tab and click **Add Agent**.
2. On the **Add Agent** step of the wizard, select the network you want to add the agent to.
3. Provide a name for the agent. Make sure the name is descriptive and recognizable.
4. Select the infrastructure type for your agent.
5. Click **Create**.

Your agent is now added. You can continue with [installing the agent](#private-connectivity-agents-install) in your own infrastructure.

### Viewing and Editing Agents {#private-connectivity-agents-details}
To view and edit an existing agent, follow these steps:
1. On the **Agents** tab, find the network that you want to view the details or edit, click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) and select **Details**. 

The details of that agent will be shown. This includes:
* Agent Name - The name you gave to the agent. You can edit this. Make sure the name is descriptive and recognizable.
* Agent ID - The internal ID of your agent. You can copy this, for example to provide it in a support ticket if you have issues with the agent.
* Agent Key - The authentication key of your agent. You can copy this authentication key to configure it when [starting an agent](#private-connectivity-agents-run). This key should be treated as confidential.
* Network -- The network the agent is connected to.
* Status (last Seen) - Shows the status of the agent
    * Connected - The agent is currently connected to the network.
	* Date and time - The last time the agent was connected to the network. The agent is not connected at this time.
* Resource Details - A list of the resources exposed via the agent.
* DERP Details - Information on the preferred Tailscale [Designated Encrypted Relay for Packets (DERP) server](https://tailscale.com/kb/1232/derp-servers).

Click **Save** to save changes you made.

### Deleting an Agent {#private-connectivity-agents-delete}
To delete an existing agent, follow these steps:
1. On the **Agents** tab, find the agent that you want to delete, click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}), select **Delete** and then confirm that you want to delete it.

Deleting an agent will automatically revoke its authentication key, remove all resources exposed through the agent and all connections to those resources. Approved connections will be broken immediately.
Deleting an agent will not uninstall the agent from your own infrastructure! See below for instructions on [uninstalling an agent](#private-connectivity-agents-uninstall). 

### Installing an Agent in your own infrastructure {#private-connectivity-agents-install}
To establish a connection between your network and your infrastructure, you must install an agent on this infrastructure. Installing an agent is typically done by your internal IT, Infra or Network team.

#### Installing an Agent on a Windows Server {#private-connectivity-agents-install-windows}
To install an agent on a Windows server, follow these steps:
1. Open the [Tailscale Download page](https://tailscale.com/download/windows).
2. Select **Windows** and click **Download Tailscale for Windows**
This will download the Tailscale installer for Windows. Run the installer to install the agent. Then continue with [starting the agent](#private-connectivity-agents-run).

#### Installing an Agent on a Linux Server {#private-connectivity-agents-install-linux}
To install an agent on a Linux server, follow these steps:
1. Run the following script on the server you want to install the agent:
```bash Linux
curl -fsSL https://tailscale.com/install.sh | sh
```
Alternatively, open the [Tailscale Download page for Linux](https://tailscale.com/download/linux) for instructions on manually installing the Tailscale agent on your specific Linux distribution.

After installing the agent, continue with [starting the agent](#private-connectivity-agents-run).

### Starting an Agent in your own infrastructure {#private-connectivity-agents-run}
When you have installed your agent, you can start the agent and connect to your network. For this, you will need an authentication key. This authentication key is created when [an agent is added](#private-connectivity-agents-add). You can find the authentication key for your agent on [the agent details page](#private-connectivity-agents-details).

To start an agent and to connect it to your network, run the following script on the machine where the agent is installed, where you replace `AUTH_KEY` with the authentication key of your agent:
```shell Windows
tailscale up --auth-key=<AUTH_KEY>
```
```bash Linux
tailscale up --auth-key=<AUTH_KEY>
```

### Uninstalling an Agent in your own infrastructure {#private-connectivity-agents-uninstall}
After [deleting an agent](#private-connectivity-agents-delete), you can uninstall it from your infrastructure. Uninstalling an agent is typically done by your internal IT, Infra or Network team.

#### Uninstalling an Agent on a Windows server {#private-connectivity-agents-uninstall-windows}
Tailscale for Windows can be uninstalled like any Windows app, by using the Windows Control Panel. Go to **Settings > Apps**, find **Tailscale**, and press the **Uninstall** button.

If you'd like to _completely_ delete Tailscale, destroying any state or local information, you can also remove the files at the following paths:
```
C:\ProgramData\Tailscale
C:\Users\%USERNAME%\AppData\Local\Tailscale
C:\Windows\System32\config\systemprofile\AppData\Local\Tailscale
```

The path under `System32` was only used in older versions of the Tailscale client and may not be present on your system.

#### Uninstalling an Agent on a Linux server {#private-connectivity-agents-uninstall-linux}
Uninstall Tailscale by using the uninstall command of the package manager you used to install the binary in the first place:

For all **Ubuntu and Debian** versions, uninstall using `apt-get`:
```bash Linux
sudo apt-get remove tailscale
```

For **CentOS 7 and Amazon Linux 2**, uninstall using `yum`:
```bash Linux
sudo yum remove tailscale
```

For **openSUSE Leap 15.1, 15.2, and openSUSE Tumbleweed**, uninstall using `zypper`:
```bash Linux
sudo zypper rm tailscale
```

For **CentOS 8, CentOS Stream 9, RHEL 8, and Fedora**, uninstall using `dnf`:
```bash Linux
sudo dnf remove tailscale
```

If you'd like to _completely_ delete Tailscale, destroying any state or local information, you can also remove the file at:
```
/var/lib/tailscale/tailscaled.state
```

## Resources {#private-connectivity-resources}
Resources are services, such as databases or applications, on your own infrastructure that are exposed via your agents and accessible via your networks. Applications on Mendix Cloud can be connected to these resources.

On the **Resources** tab of the [Private Connectivity](https://privateconnectivity.mendix.com) page in Control Center, you can see all the exposed resources of your company. The page shows the following information for each resource:
* Resource - The name of the resource.
* Agent - The name of the agent that exposes the resource.
* Network - The name of the network that the agent exposing the resource is connected to.
* Status - The status of the resource. This is one of the following:
	* Enabled - Users can request connections to the resource.
	* Disabled - Users can't request connections to the resource.
*Environments - The number of application environments on Mendix Cloud that have an approved connection to the resource.

### Viewing and Editing Resources {#private-connectivity-resources-details}
1. On the **Resources** tab, find the resource that you want to view the details or edit, click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) and select **Details**. 
The details of that resource will be shown. This includes
* Resource Name - The name you gave to the resource. You can edit this. Make sure the name is descriptive and recognizable.
* Resource ID - The internal ID of your resource. You can copy this, for example to provide it in a support ticket if you have issues with the resource.
* Resource Type - The type of the resource, this is one of the following values:
* Route - The resource is an exposed subnet route.
* Route - The exposed IP range (only if the resource type is _Route_)
* Agent - The name of the agent that exposes the resource.
* Network - The name of the agent that the agent exposing the resource is connected to.
* Status - The status of the resource. This is on of the following:
	* Enabled - Users can request connections to the resource.
	* Disabled - Users can't request connections to the resource.
* Environment Details - A list of applications environments that have an approved connection to the resource.

Click **Save** to save changes you made.

### Exposing Resources {#private-connectivity-resources-expose}
Before you can connect to resources running on your own infrastructure, you have to expose these resources through an agent. This requires you to install an agent on the machine running the resource, or on a machine that has access to the resource.

Mendix Cloud Private Connectivity currently supports exposing physical [subnet routes](https://tailscale.com/kb/1019/subnets) to your network, via an agent. You can expose a single IP range, for example `192.0.2.0/24` or multiple IP ranges separated by a semicolon, for example `192.0.2.0/24,198.51.100.0/24`.

#### Exposing Subnet Routes on a Windows server {#private-connectivity-resources-expose-routes-windows}
To expose subnet routes for an agent that is already running, run the following script on the machine where the agent is installed, where you replace `IP_RANGE` with the IP range(s) you want to expose:
```shell Windows
tailscale set --advertise-routes=<IP_RANGE>
```

You can also configure the exposed subnet routes when starting the agent. In that case, use the following script, replacing `AUTH_KEY` with the authentication key of your agent and `IP_RANGE` with the IP range(s) you want to expose:
```shell Windows
tailscale up --auth-key=<AUTH_KEY> --advertise-routes=<IP_RANGE>
```

#### Exposing Subnet Routes on a Linux server {#private-connectivity-resources-expose-routes-linux}
To expose subnet routes for an agent on a Linux server, follow these steps:
1. [Enable IP forwarding](https://tailscale.com/kb/1019/subnets?tab=linux#enable-ip-forwarding)
2. Run the following script on the machine where the agent is installed, where you replace `IP_RANGE` with the IP range(s) you want to expose:
```shell Linux
sudo tailscale set --advertise-routes=<IP_RANGE>
```

You can also configure the exposed subnet routes when starting the agent. In that case, use the following script, replacing `AUTH_KEY` with the authentication key of your agent and `IP_RANGE` with the IP range(s) you want to expose:
```shell Linux
sudo tailscale up --auth-key=<AUTH_KEY> --advertise-routes=<IP_RANGE>
```

### Enabling and disabling Resources {#private-connectivity-resources-enable-disable}
Once resources are [exposed](#private-connectivity-resources-expose), they must be enabled by a Mendix Admin, before users can request connections to the resource.

To enable a resource, follow these steps:
1. On the **Resources** tab, find the resource you want to enable and click **Enable**.

To disable a resource, follow these steps:
1. On the **Resources** tab, find the resource you want to enable and click **Disable**.

## Connections {#private-connectivity-connections}
Mendix Cloud Connect connections allow applications on Mendix Cloud to connect to Mendix Cloud Connect resources over Mendix Cloud Connect networks. A connection has to be requested and approved, before an application on Mendix Cloud can connect to the resource. An application on Mendix Cloud can have multiple connections to multiple resources.

On the **Connections** tab of the [Private Connectivity](https://privateconnectivity.mendix.com) page in Control Center, you can see all the connections of your company. The page shows the following information for each connection:
* App - The name of the app for the connection
* Environment - The name of the Environment for the connection
* Network -- The network for the connection.
* Resource - The name of the resource for the connection.
* Status - Shows the status of the connection. This is one of the following:
    * Pending - The connection was requested, but not yet approved. The app environment can't connect to the resource using this connection.
	* Approved - The connection is approved. The app environment can connect to the resource using this connection.
	* Rejected - The connection is rejected. The app environment can't connect to the resource using this connection.

### Viewing Connection Details {#private-connectivity-connections-details}
To view an existing connection, follow these steps:
1. On the **Connections** tab, find the connection that you want to view the details or edit, click **More Options** ({{< icon name="three-dots-menu-horizontal" >}}) and select **Details**. 

The details of that connection will be shown. This includes:
* Request Details - The details of the connections request:
	* Name - The name of the user that requested this connection.
	* Status - The status of the connection request. This is one of the following:
		* Pending - The connection was requested, but not yet approved. The app environment can't connect to the resource using this connection.
		* Approved - The connection is approved. The app environment can connect to the resource using this connection.
		* Rejected - The connection is rejected. The app environment can't connect to the resource using this connection.
	* Date - The data and time the connection was requested.
* App - The name of the app for the connection
* Environment - The name of the Environment for the connection
* Network -- The network for the connection.
* Resource - The name of the resource for the connection.
* Resource ID - The internal ID of your resource. You can copy this, for example to provide it in a support ticket if you have issues with the resource.
* Agent - The name of the agent for the connection.
* Agent ID - The internal ID of the agent. You can copy this, for example to provide it in a support ticket if you have issues with the agent.
* Network - The name of the network for the connection.
* Network ID - The internal ID of the network. You can copy this, for example to provide it in a support ticket if you have issues with the Network.

Click **Save** to save changes you made.

## Approving and Rejecting Connections {#private-connectivity-connections}
Once a connection is requested, it must be approved before the app enviroment can connect to the resource.

To approve a connection, follow these steps:
1. On the **Connections** tab, find the connection you want to enable and click **Approve**.

If this is the first connection that is approved for an app environment, you must [redeploy](/developerportal/deploy/mendix-cloud-deploy/deploying-an-app/) the environment to be able to use the connection!

To disable a connection, follow these steps:
1. On the **Connections** tab, find the connection you want to enable and click **Reject**.

## Activities
On the Activity tab, you can view a log of activities performed on your Private Connectivity assets.