---
title: "Troubleshooting Team Server Issues"
url: /refguide/troubleshoot-team-server-issues/
linktitle: "Team Server Issues"
weight: 30
description: "Presents a list of problems and fixes for Team Server issues."
---

## Introduction

Mendix Studio Pro needs to connect to the Team Server, where all your apps are stored. Team Server is a version control server. Git is the default version control system for the Team Server. This document describes permissions and settings required to connect to the Team Server, as well as troubleshooting steps in case of connectivity issues.

## Basic Configuration: Team Server App Network Settings

Being unable to download the Team Server app can indicate that the security configuration of your company network is blocking access to `https://home.mendix.com` and the Team Server itself that is located at `https://git.api.mendix.com/`.

Mendix Studio Pro uses the HTTPS (TCP) to communicate with the Team Server. To access the Team Server from Studio Pro, the network at your location needs the following settings:

* The HTTPS port (TCP 443) needs to be open
* The HTTP port (TCP 80) needs to be open

Mendix Studio Pro connects to `https://teamserver.sprintr.com/` and with the domains shown in the diagram below over HTTPS on port 443. These domains should be added to the firewall white list:

{{< figure src="/attachments/refguide/version-control/troubleshoot-version-control-issues/networkaccessmendixplatform.png" alt="Domains home.mendix.com, cloud.mendix.com, and git.api.mendix.com need to be accessible on port 443 from your network" class="no-border" >}}

You can look up the IP address `https://git.api.mendix.com/`.

{{% alert color="warning" %}}
Mendix reserves the right to change the IP address at any time and without notification to the customer. This could happen if Mendix moves to a different infrastructure, for example.
{{% /alert %}}

{{% alert color="info" %}}
Contact your network administrator and give them this information to allow them to configure your network (for example, firewall and proxy settings) correctly.
{{% /alert %}}

## Advanced

### Symptoms

Customers experiencing Git-related problems often report the following issues:

* Git operations (clone, commit, pull, push) are unusually slow or never complete.
* Network timeout errors when connecting to git.api.mendix.com. 
* SSL connection failures during Git operations. 
* Proxy error
* Sprintr connector errors while trying to create a project or interact with the TeamServer.

#### Known Errors and Exceptions

**Timeout Error:**

```Failed to connect to git.api.mendix.com port 443 after 262515 ms: Timed out```

**SSL Connection Error:**

```The SSL connection could not be established```

**Sprintr Connector Exception:**

```
Mendix.Modeler.Sprintr.SprintrConnectorException: Error while creating sprintr project.
---> Mendix.Modeler.VersionControl.TeamServer.TeamServerRepositoryServiceException: An error occurred while sending the request.
---> System.Net.Http.HttpRequestException: An error occurred while sending the request.
---> System.Net.Http.WinHttpException (80072EFF, 12030): Error 12030 calling WINHTTP_CALLBACK_STATUS_REQUEST_ERROR, 'The connection with the server was terminated abnormally'.
```

**SocketException and Early EOF:**

```LibGit2Sharp.LibGit2SharpException: early EOF```

**Proxy error:**

```Failed with status 407```

### Diagnosing the Issues

To determine which area has an issue there are 3 diagnosing steps, which are explained in more detail in the next sections.

1. **General connectivity:** Are you able to reach the Team Server, what is the internet speed, is there a VPN/Proxy?
2. **GIT CLI interaction with the Team Server:** Is the Git Commandline Interface (CLI) able to work effectively with the Team Server?
3. **Studio Pro connectivity:** Are you encountering issues within Studio Pro, provided that the first two steps have not resulted in issues?

#### Validating General Connectivity

##### CURL over HTTP
The first step is to validate whether CURL is able to reach the Team Server over HTTP.
* **Why:** Git uses the CURL library to do network operations over HTTP or HTTPS. If CURL does not work the network infrastructure is interfering. 
* **How:** Ask the customer to provide the output of the following command on the command line: ```curl -v http://git.api.mendix.com```
* **Validate the output:** Beneath you will see a expected response. Important lines:
    * **Line 6:** We see the request connected to git.api.mendix.com This means the request hit our server and was not interrupted.
    * **Line 13:** We see the request returned a Permanent redirect. This is expected as we are hitting the  http URL, which should redirect to the https variant.
* **Actions to take if something is wrong:** The customer should ask their internal IT department to look into this and get back to Mendix once the CURL request returns an expected request. 
    ```
    curl -v git.api.mendix.com
    * Host git.api.mendix.com:80 was resolved.
    * IPv6: (none)
    * IPv4: 63.32.204.124, 34.246.251.192
    *   Trying 63.32.204.124:80...
    * Connected to git.api.mendix.com (63.32.204.124) port 80
    > GET / HTTP/1.1
    > Host: git.api.mendix.com
    > User-Agent: curl/8.8.0
    > Accept: */*
    >
    * Request completely sent off
    < HTTP/1.1 308 Permanent Redirect
    < Date: Wed, 02 Oct 2024 09:34:54 GMT
    < Content-Type: text/html
    < Content-Length: 164
    < Connection: keep-alive
    < Location: https://git.api.mendix.com
    < X-Content-Type-Options: nosniff
    <
    <html>
    <head><title>308 Permanent Redirect</title></head>
    <body>
    <center><h1>308 Permanent Redirect</h1></center>
    <hr><center>nginx</center>
    </body>
    </html>
    * Connection #0 to host git.api.mendix.com left intact
    ```

##### CURL over HTTPS
The second step is to validate whether CURL is able to reach the Team Server over HTTPS.
* **Why:** Git uses the CURL library to do network operations over HTTP or HTTPS. If CURL does not work the network infrastructure is interfering. 
* **How:** Ask the customer to provide the output of the following command on the command line: ```curl -v https://git.api.mendix.com```
* **Validate the output:** Beneath you will see a expected response. Important lines:
    * **Line 2:** We see the host/domain is resolved
    * **Line 6:** We see the request connected to https://git.api.mendix.com on port 443 This means the request hit our server and was not interrupted
    * **Line 13:** We see the request returned a Permanent redirect. This is expected as we are hitting the  http URL, which should redirect to the https variant.
    * **Line 23:** Even though this line says 400 Bad Request, this is expected as the teams server is no HTTP server. This line DOES tell us the request made it to the server and a response was given. 
* **Actions to take if something is wrong:** The customer should ask their internal IT department to look into this and get back to Mendix once the CURL request returns an expected request. 
    ```
    curl -v https://git.api.mendix.com
    * Host git.api.mendix.com:443 was resolved.
    * IPv6: (none)
    * IPv4: 63.32.204.124, 34.246.251.192
    *   Trying 63.32.204.124:443...
    * Connected to git.api.mendix.com (63.32.204.124) port 443
    * schannel: disabled automatic use of client certificate
    * ALPN: curl offers http/1.1
    * ALPN: server accepted http/1.1
    * using HTTP/1.x
    > GET / HTTP/1.1
    > Host: git.api.mendix.com
    > User-Agent: curl/8.8.0
    > Accept: */*
    >
    * Request completely sent off
    * schannel: remote party requests renegotiation
    * schannel: renegotiating SSL/TLS connection
    * schannel: SSL/TLS connection renegotiated
    * schannel: remote party requests renegotiation
    * schannel: renegotiating SSL/TLS connection
    * schannel: SSL/TLS connection renegotiated
    < HTTP/1.1 400 Bad Request
    < Date: Wed, 02 Oct 2024 09:34:44 GMT
    < Content-Length: 64
    < Connection: keep-alive
    < Strict-Transport-Security: max-age=15724800; includeSubDomains
    < X-Content-Type-Options: nosniff
    <
    Please provide valid input to execute this request. Invalid url.* Connection #0 to host git.api.mendix.com left intact
    ```

##### VPN, Proxy or Zscaler
The next step is to verify whether you are behind a VPN, Proxy or Zscaler.

* **Why:** Git uses the CURL library to do network operations over HTTP or HTTPS. If CURL does not work the network infrastructure may be interfering.  




#### GIT CLI Interaction with the Team Server

#### Studio Pro Connectivity
{{% alert color="info" %}}
When reaching this step in the diagnosis, you should have already seen a Git clone working outside Studio Pro. If this is not the case, please first read through the previous chapters.
{{% /alert %}}

From this point onward the actions taken in Studio Pro should be done with debug level logs. [This page](https://support.mendix.com/hc/en-us/articles/9634287106204-How-to-enable-extra-logs-e-g-Debug-Logs-in-Studio-Pro) describes how to enable that.

At the end all files from the log folder should be shared with Mendix in a Support ticket.

**Why are we doing this:** Given that the commands through Git CLI succeeded, we need to find out what is the problem with Studio Pro.

##### If a Full Clone Through Git CLI Succeeded

Please attempt a regular clone in Studio Pro. In case this fails, run ```netsh.exe winhttp reset proxy``` on the commandline and try again. 
If it still fails, gather all files from the Log directory and provide them to Support.

##### If a Shallow Clone Through Git CLI Succeeded

Please attempt a Partial Clone in Studio Pro, by changing the [Clone Type](/refguide/clone-type) and doing a fresh clone. In case this fails, run ```netsh.exe winhttp reset proxy``` on the commandline and try again. 
If it still fails, gather all files from the Log directory and provide them to Support.

##### If a Clone Through Git CLI Without SSL Succeeded

Please temporarily disable Git sslVerify, by running ```config --global http.sslVerify false```, and attempt a fresh clone in Studio Pro. 

Re-enable Git sslVerify by running ```git config --global --unset http.sslVerify```.

Gather all files from the Log directory and provide them to Support. Please mention the result of cloning with Git sslVerify disabled.



## TO DO: Clean up the text:
* Git CLI should always be written in this casing
* CURL should always be uppercase, outside the code blocks
* HTTP and HTTPS should also be uppercased, outside code blocks and URLs



## Read More

* [Version Control FAQ](/refguide/version-control-faq/)
