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
2. **GIT CLI interaction with the Team Server:** Is the Git Command Line Interface (CLI) able to work effectively with the Team Server?
3. **Studio Pro connectivity:** Are you encountering issues within Studio Pro, provided that the first two steps have not resulted in issues?

#### Validating General Connectivity

##### CURL over HTTP
The first step is to validate whether CURL is able to reach the Team Server over HTTP.
* **Why:** Git uses the CURL library to do network operations over HTTP or HTTPS. If CURL does not work the network infrastructure is interfering. 
* **How:** Store the output of the following command on the command line: ```curl -v http://git.api.mendix.com```
* **Validate the output:** Beneath you will see a expected response. Important lines:
    * **Line 6:** We see the request connected to git.api.mendix.com This means the request hit our server and was not interrupted.
    * **Line 13:** We see the request returned a Permanent redirect. This is expected as we are hitting the HTTP URL, which should redirect to the HTTPS variant.
* **Actions to take if something is wrong:** Request your internal IT department to look into this and get back to Mendix once the CURL request returns an expected request. 
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
* **How:** Store the output of the following command on the command line: ```curl -v https://git.api.mendix.com```
* **Validate the output:** Beneath you will see a expected response. Important lines:
    * **Line 2:** We see the host/domain is resolved
    * **Line 6:** We see the request connected to https://git.api.mendix.com on port 443 This means the request hit our server and was not interrupted
    * **Line 13:** We see the request returned a Permanent redirect. This is expected as we are hitting the HTTP URL, which should redirect to the HTTPS variant.
    * **Line 23:** Even though this line says 400 Bad Request, this is expected as the teams server is no HTTP server. This line DOES tell us the request made it to the server and a response was given. 
* **Actions to take if something is wrong:** Request your internal IT department to look into this and get back to Mendix once the CURL request returns an expected request. 
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
* **How:** 
    * Check whether you are behind a VPN or proxy. Reach out to your internal IT department if you're not sure. Alternatively you can try a website to detect a VPN.
    * Perform the following line on `cmd` and store the output: ```netsh.exe winhttp show proxy```
* **Validate the output:** 
    * A system with no proxy should show the following:
    ```
    C:\Users\UserName>netsh.exe winhttp show proxy
    Current WinHTTP proxy settings:
    Direct access (no proxy server).
    ```
* **Actions to take if something is wrong:**
    * If you are behind a proxy according:  
        * Retry without VPN or proxy and see if that solves the issue. Reach out to your internal IT department if you're not sure how to test this.
        * Setup the proxy in Git according to [Troubleshooting Version Control](/refguide/troubleshoot-version-control-issues/#proxy-servers-are-not-supported).
        A proper example command would look like this: ```git config --global http.proxy https://username:mypassword@someproxyurl.com:123123```
        Take note; if the username or password contains a `:` or a `@` it might not work.
    * In case ```netsh.exe winhttp show proxy``` gives an output that implies there is no proxy active, keep this information in mind. If the Git CLI **is** able to work with Git, but Studio Pro **is not**, then you can ask try ```netsh.exe winhttp reset proxy```, to reset your local proxy setting.

##### Validate network speed

* **Why:** Speed gives us a tool to calculate the expected time a clone should roughly take.
* **How:** Use a tool to test your network speed, such as [speedtest.net](https://www.speedtest.net/) and write down the download and upload speed.
* **Validate the output:** The speeds are in bits, meaning the value has to be divided by 8. if the speed is shown as 80Mbitps, the actual speed is 10MByte per second. Meaning a 80MB repo will take at least 8 seconds to download.

#### GIT CLI Interaction with the Team Server

In the previous chapter, we checked: 
* The ability of CURL to access the Team Server through HTTP and HTTPS;
* Proxy/VPN situation;
* Internet speed.

With this information, we will try to get Git working on the command line.

**Why are we doing this:**
Studio Pro uses the Git Command Line Interface (CLI) under the hood. However, Studio Pro does a little bit more. To pinpoint the exact area causing the issue, it is important to first attempt to reproduce the issue without Studio Pro.

{{% alert color="info" %}}
If you are encountering issues when using Git CLI, this is typically an issue your internal IT department should help with.
{{% /alert %}}

##### Setup Before Troubleshooting

You need to setup a [Personal Access Token (PAT)](https://docs.mendix.com/community-tools/mendix-profile/user-settings/#pat) to work with Git on the command line. The following permissions should be configured while creating the PAT:
```
Model Repository
mx:modelrepository:repo:write
Write access to Team Server Git repositories and Team Server API
mx:modelrepository:repo:read
Read access to Team Server Git repositories and Team Server API
mx:modelrepository:write
Full access to Team Server Git repositories and Team Server API
Model Server
mx:modelrepository:write 
```

{{% alert color="info" %}}
The troubleshooting commands should be executed in `cmd` or `command prompt` in Windows. Executing these through `Powershell` will not work for all commands.
{{% /alert %}}

##### Verify Git Config
* **Why:** Various config lines in the config could influence the connection, such as the SSL config or proxy settings.
* **How:** Perform the following command in `cmd` and store the output for contacting Support: `git config --list --show-origin`
* **Validate the output:** Please be on the lookout for values `http.proxy` containing incorrect values.
* **Actions to take if something is wrong:** Update the config file where needed: `git config --global [key] "[value]"`.

##### Verify Full Clone Through Git CLI:
* **Why:** Validate if a Git clone works as intended outside Studio Pro.
* **How:** Replace `[PROJECT_ID]` with your project ID and perform the following command in `cmd`:
```
set GIT_TRACE_PACKET=1
set GIT_TRACE=1
set GIT_CURL_VERBOSE=1
git clone https://git.api.mendix.com/[PROJECT_ID].git 
```
* **Validate the output:** 
If this succeeded, please continue with troubleshooting the [Studio Pro connectivity](#studio-pro-connectivity).
* **Actions to take if something is wrong:** Save the output and try the next step, a shallow clone.


##### Verify Shallow Clone Through Git CLI:
* **Why:** If a full clone fails it could be due to the size of the repository. A shallow clone only retrieves all files for 1 commit, reducing the amount of data needed to transfer by magnitudes.
* **How:** Replace `[PROJECT_ID]` with your project ID and perform the following command in `cmd`:
```
set GIT_TRACE_PACKET=1
set GIT_TRACE=1
set GIT_CURL_VERBOSE=1
git clone --depth 1 https://git.api.mendix.com/[PROJECT_ID].git 
```
* **Validate the output:** 
If this succeeded there is a connectivity issue causing a full clone to fail. Contact your internal IT department to discuss the situation.
* **Actions to take if something is wrong:** Save the output and try the next step, a shallow clone without using HTTPS.

##### Verify Shallow Clone Through Git CLI Without HTTPS:
* **Why:** If a shallow clone fails it could still be that the SSL handling is failing. This could be due to a proxy, ZScaler or a VPN.
* **How:** Replace `[PROJECT_ID]` with your project ID and perform the following command in `cmd`:
```
set GIT_TRACE_PACKET=1
set GIT_TRACE=1
set GIT_CURL_VERBOSE=1
git -c http.sslVerify=false clone --depth 1 https://git.api.mendix.com/[PROJECT_ID].git 
```
* **Validate the output:**
If this succeeded, there is an issue with SSL handling. Please take a look at your git config 
    * If you are using a proxy: setting up a `http.proxy` in the git config might address this issue.
    * If you are seeing a warning on SSL on windows: setting the `http.sslbackend` to `schannel` by running the following command might improve your experience. `git config --global http.sslbackend schannel`
    * If you are using a VPN: try again without VPN
    * If Zscaler or any other network filtering/security tool is active: check whether SSL inspection is enabled. Try disabling SSL inspection or adding an exception for git.api.mendix.com.
    *
* **Actions to take if something is wrong:** Save the output and reach out to Mendix Support with all information that you've gathered to discuss the situation.

#### Studio Pro Connectivity {#studio-pro-connectivity}
{{% alert color="info" %}}
When reaching this step in the diagnosis, you should have already seen a Git clone working outside Studio Pro. If this is not the case, please first read through the previous chapters.
{{% /alert %}}

From this point onward the actions taken in Studio Pro should be done with debug level logs. To achieve this start Studio Pro the commandline running the following command adjusting the version to match your project: ```C:\Program Files\Mendix\9.24.4.11007\modeler>studiopro.exe --log-level=debug```

At the end all files from the log folder should be shared with Mendix in a Support ticket. Go to Help-> Open Log File directory to easily access the logfolder.

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

## Read More

* [Version Control FAQ](/refguide/version-control-faq/)