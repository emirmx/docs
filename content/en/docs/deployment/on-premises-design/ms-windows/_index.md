---
title: "Microsoft Windows"
url: /developerportal/deploy/deploy-mendix-on-microsoft-windows/
description: "How to install and configure Mendix on a system running Microsoft Windows"
weight: 50
aliases:
    - /deployment/on-premises/deploy-mendix-on-microsoft-windows.html
    - /deployment/on-premises/deploy-mendix-on-microsoft-windows
#To update these screenshots, you can log in with credentials detailed in How to Update Screenshots Using Team Apps.
---

## Introduction

This document describes the installation and configuration of Mendix software on a system running Microsoft (MS) Windows. It covers:

* Installing the Mendix Service Console

* Deploying a Mendix app

* Configuring the MS Internet Information Services (IIS) server

## Prerequisites {#Prerequisites}

To set up an environment to run Mendix applications, you will need to install the Mendix software. You must also create a separate user (service) account for each Mendix application you plan to run.

{{< figure src="/attachments/deployment/on-premises-design/ms-windows/ms-windows-setup.png" >}}

Before starting this how-to, make sure you have the following prerequisites:

* MS Windows Server 2012 or higher
    * The Mendix Service Console will run and deploy a Mendix app on the [minimum hardware requirements for MS Windows Server 2012](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134246(v=ws.11)#system-requirements). However, you may need to increase the specifications depending on the functionality of your app. Although not directly comparable, see the [Cloud Resource Packs](/developerportal/deploy/mendix-cloud-deploy/#resource-pack) used when deploying to Mendix Cloud for comparative information.
* .NET Framework 4.7.2 or higher
* IIS 8 or higher with the following service roles enabled:

    * IIS Management console
    * Default Document
    * Static content

* MS Application Request Routing (ARR) installed (for more information, see [Application Request Routing](https://www.iis.net/downloads/microsoft/application-request-routing))
* MS IIS URL Rewrite installed (for more information, see [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite))
* Java Runtime, version depending on your Mendix Server Distribution. See [System Requirements](/refguide/system-requirements/#java) for more information. 
* The Mendix Deployment Archive (MDA) of your Mendix project
* The Mendix server distribution corresponding to your Mendix Studio Pro version (see the [Mendix Marketplace](https://marketplace.mendix.com/link/studiopro/))
* A database with sufficient security rights

    * Suitable database servers are MariaDB, MS SQL Server, MySQL, Oracle Database and PostgreSQL. See [System Requirements](/refguide/system-requirements/#databases) for more information

* A local or domain user with the *“log on as a service”* local security policy set

## Installing the Mendix Service Console {#service-console}

To download and install the Mendix Service Console, follow these steps:

1. Download the latest version of the [Mendix Service Console](https://marketplace.mendix.com/link/component/223425) module from the Marketplace.
2. Install the Mendix Service Console by following the installation wizard.

3. Start the Mendix Service Console after completing the installation. The first time you launch the application, you will see a dialog box (it will always be shown if no valid location is configured for the apps and server files):

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/service_console_first_run.png" >}}

4. Click **Yes**. The **Preferences** dialog box will be shown:

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/18580730.png" >}}

5. In the **Preferences** dialog box, enter the **Location of apps and server files**. This location is used for storing your app files and Mendix server files. Mendix recommends using a directory:

    * that is NOT on the system partition
    * where you can easily control the security rights

    The app directory consists of four sub-directories:

    * Backup – stores any database changes due to model upgrades
    * Log – stores all of the application log files
    * Project – contains all of your application files; within this directory you will find the directory data/files that contain all of your uploaded files
    * Service – contains files for configuring the Windows Services

    In addition, there will be a file called `Settings.yaml` that contains your application configuration.

## Deploying a Mendix App

To deploy a Mendix app using the Mendix Service Console, follow these steps:

1. Start the Mendix Service Console.
2. Click **Add app** to add a new app. A wizard will appear for configuring the new app.
3. Configure the **Service Settings** as follows:

    * **Service name** – this name must be unique within all existing Windows services
    * **Display name** – the description of the app which is visible as a tooltip for the app in the left bar of the Mendix Service Console or as a column in the list of Windows services
    * **Description** – a description of the application that will be visible in the Mendix Service Console
    * **Startup type** – select whether you want the app to be started automatically when the server starts, started with a delay, started manually, or disabled altogether
    * **User name** and **Password** – the app will always run under the user account given here, and the service will be installed with this user account configured (for more information, see [Prerequisites](#Prerequisites))

4. Click **Next >**.

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/18580728.png" >}}

5. On the **Project Files** screen, click **Select app…**.

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/service_console_selectapp.png" >}}

6. Now select the **MDA** file that was [created in Mendix Studio Pro](/refguide/create-deployment-package-dialog/) and contains your application logic. After the installation of your MDA file, you will see which Mendix server (Mendix Runtime) version is needed.

7. Configure the **Database Settings**:

    * **Type** – the database server type
    * **Host** – the IP address or host name of the database server
    * **Name** – the database name
    * **User name** and **Password** – the database user name and password

8. Click **Next >**.

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/18580726.png" >}}

9. On the **Common Configuration** screen, keep the default settings. These settings should only be changed if this is needed for your application setup.

10. Click **Finish** and start the application.

## Configuring the Microsoft Internet Information Services Server{#configure-msiis}

To configure the MS IIS server, follow the steps in the sections below.

### Activating a Proxy in ARR

In order to use the proxy functionality within ARR, you need to enable this feature within IIS. To activate a proxy in ARR, follow these steps:

1. Start the IIS Manager.
2. Select the **Server** in the **Connections** pane.
3. Open the **Application Request Routing** feature.
4. Click **Server Proxy Settings** in the **Actions** pane on the right side of the screen.
5. Select **Enable proxy** and click **Apply** in the **Actions** pane.

### Creating a Website

To create a website, follow these steps:

1. Open the IIS Manager.
2. In the **Connections** pane, click the **Sites** node in the tree. If **Default Website** or any other website is present under **Sites**, check if it is being used.
3. Right-click **Sites** and select **Add Web Site**.
4. In the **Add Web Site** dialog box, enter a friendly name for your web site in the **Web site name** field.
5. In the **Physical path** field, enter the physical path of your application-project-web folder (for example, *D:\Mendix\Apps\Application\Project\Web*).
6. Select the **Protocol** for the website from the **Type** list.
7. The default value in the IP address box field is **All Unassigned**. If you need to specify a static IP address for the website, enter the address in the **IP address** box.
8. Enter a port number in the **Port** field.
9. If any other website is already running on this IIS server, enter the desired hostname for this website in the **Host name** field.
10. Click **OK**.

### Add HTTPS binding

1. Make sure the certificate you want to use for the website has been added to the Windows Certificate Store.
2. Right-click the website you have just created and select **Edit Bindings...**.
3. Click **Add...**.
4. In the **Type** field, select **https**.
5. In the **Host name** field, enter the hostname you want to use for this website.
6. If the certificate you are going to use is an SNI certificate, check the **Require Server Name Indication** box.
7. Select the certificate for the website either in the dropdown box or through the **Select...** dialog.
8. Click **OK**.

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/iis_add_https_binding.png" >}}

### Configuring the MIME Types

To configure the MIME types, follow these steps:

1. Open the IIS Manager and navigate to the website you want to manage.
2. In the **Features View**, double-click **MIME Types**.
3. In the **Actions** pane, click **Add**.
4. In the **Add MIME Type** dialog box, add this file type:

    * **File name extension**: *.mxf*
    * **MIME type**: *text/xml*

5. Depending on the IIS version, the MIME type for JSON can be present by default or not. Check if *.json* is already in the list and if not, add another MIME type:

    * **File name extension**: *.json*
    * **MIME type**: *application/json*

6. Click **OK**.

### Configuring the URL Rewrite

{{% alert color="info" %}}
These instructions use port 8080, which is the default port. Please use the port for which your Mendix App is configured.
{{% /alert %}}

#### Reverse Proxy Inbound Rules{#reverse-proxy-rules}

You need to add a number of rules to configure the following request handlers.

Rule | Name | Pattern | Rewrite URL
:--- | :--- | :--- | :---
1 | xas | `^(xas/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
2 | ws | `^(ws/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
3 | ws-doc | `^(ws-doc/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
4 | file | `^(file)(.*)` | `http://localhost:8080/{R:1}{R:2}`
5 | link | `^(link/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
6 | rest | `^(rest/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
7 | rest-doc | `^(rest-doc/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
8 | debugger | `^(debugger/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
9 | oauth | `^(oauth/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
10 | p | `^(p/)(.*)` | `http://localhost:8080/{R:1}{R:2}`
11 | manifest | `^(manifest.webmanifest)(.*)` | `http://localhost:8080/{R:1}{R:2}`

{{% alert color="info" %}}
Some patterns include a trailing slash, `/`, when they need to match an exact path. For example, the pattern `ws-doc/` will match `/ws-doc/mydoc/1234`, but it will not match similar prefixes like `/ws-documentation/`.

Additionally, while the example path (`/ws-doc/mydoc/1234`) includes a leading slash because browser URLs always start with one, IIS rewrite patterns do not include this slash. This is because the web server removes the leading slash before processing the URL path for matching.
{{% /alert %}}

Follow the instructions below and replace *[Name]* with the name of the rule in the table above, *[Pattern]* with the regular expression pattern, and *[Rewrite URL]* with the Rewrite URL.

1. Open the IIS Manager and navigate to the website you want to manage.
2. In the **Features View**, double-click **URL Rewrite**.
3. In the **Actions** pane on the right side of the screen, click **Add rule(s)…** to add a new rewrite rule.
4. In the **Inbound Rules** section, double-click *Blank rule*.
5. In the **Name** field, enter *[Name]* from the table above.
6. In the **Match URL** section, set **Requested URL** to *Matches the Pattern*.
7. Set **Using** to *Regular Expressions*.
8. In the **Pattern** field, enter `[Pattern]`.
9. In the **Action** section, set **Action type** to *Rewrite*.
10. In the **Rewrite URL** field, enter `[Rewrite URL]` (in the rules above, this is always `http://localhost:8080/{R:1}{R:2}`).
11. Ensure the **Append query string** checkbox is set to *true* (checked).
12. Click **Apply**.
13. Click **Back to Rules**.
14. Repeat from step 3 to add all the required rules.

You can also add additional request handlers in the same way. However you must ensure that they come *after* the rule *add x-forwarded-proto header*, described below.

#### Rule *add x-forwarded-proto header*

This is required to ensure that you can access the Swagger documentation of your published REST services. 

{{% alert color="info" %}}
This has to be the first rule; it is described after the rewrite rules to ensure that it is moved to the top and that additional rules are not placed above it accidentally.
{{% /alert %}}

1. Click **View Server Variables**.
2. Check if server variable **HTTP_X_FORWARDED_PROTO** is listed. If it is, skip to step 7.
3. In the **Action** page, click **Add** to add the server variable.
4. Enter the **Server variable name** *HTTP_X_FORWARDED_PROTO*.
5. Click **OK**.
6. Click **Back to Rules**.
7. Click **Add rule(s)…**.
8. Click **Blank Rule**.
9. Set the **Name** to *add x-forwarded-proto header*.
10. In the **Match URL** section, set **Requested URL** to *Matches the Pattern*.
11. Set **Using** to *Regular Expressions*.
12. Set the **Pattern** to `.*`.
13. Set **Ignore Case** to *true* (checked).
14. In the **Server Variables** section, click **Add**.
15. Select Server variable name **HTTP_X_FORWARDED_PROTO**.
16. Set **Value** to *https*.
17. Click **OK**.
18. In the **Action** section, select **None**.
19. Set **Stop processing of subsequent rules** to *false* (unchecked).
20. Click **Apply** in the **Action** pane to save the rule.
21. Click **Back to Rules**.
22. Select the newly created *add x-forwarded-proto header* rule and use the **Move Up** button in the Action pane to move the rule to the top of the list.

#### Redirect HTTP to HTTPS (optional)

If HTTPS was configured at step 5.3 it is recommended to redirect all unencrypted HTTP traffic to HTTPS. To configure this, follow these steps:

1. Click **Add rule(s)…**.
2. Click **Blank Rule**.
3. Set the **Name** to *Redirect to HTTPS*.
4. In the **Match URL** section, set **Requested URL** to *Matches the Pattern*.
5. Set **Using** to *Regular Expressions*.
6. Set the **pattern** to `(.*)`.
7. Set **Ignore Case** to *true* (checked).
8. In the **Conditions** section, click **Add...**.

    1. In the **Condition input** field, enter `{HTTPS}`.
    2. Set **Check if input string** to *Matches the Pattern*.
    3. In the **Pattern** field, enter: `off`.
    4. Set **Ignore case** to *true* (checked).
    5. Click **OK**.

9. In the **Action** section, set **Action type** to *Redirect*.
10. In the **Redirect URL** field, enter `https://{HTTP_HOST}/{R:1}`.
11. Set **Append query string** to *true* (checked).
12. Set **Redirect type** to *Permanent (301)*.
13. Click **Apply** in the **Actions** pane to save the rule.
14. Click **Back to Rules**.
15. Select the newly created *Redirect to HTTPS* rule and use the **Move Up** button in the Action pane to move the rule to the top of the list, even above the previously created *add x-forwarded-proto header* rule.

### Disabling the Client Cache

1. In the **Features View**, double-click **HTTP Response Headers**.
2. In the **Actions** pane, click **Set Common Headers...**.
3. Set **Expire Web content** to *true* (checked).
4. Make sure the *Immediately* radio button is selected.
5. Click **OK**.

    {{< figure src="/attachments/deployment/on-premises-design/ms-windows/iis_response_headers.png" >}}

Afterwards, the contents of the *web.config* file will be similar to the following example:

**web.config**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                <rule name="add x-forwarded-proto header">
                    <match url=".*" />
                    <conditions logicalGrouping="MatchAll" trackAllCaptures="false" />  <serverVariables>
                        <set name="HTTP_X_FORWARDED_PROTO" value="https" />
                    </serverVariables>
                    <action type="None" />
                </rule>
                <rule name="xas" stopProcessing="true">
                    <match url="^(xas/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="ws" stopProcessing="true">
                    <match url="^(ws/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="ws-doc" stopProcessing="true">
                    <match url="^(ws-doc/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="file" stopProcessing="true">
                    <match url="^(file)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="link" stopProcessing="true">
                    <match url="^(link/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="rest" stopProcessing="true">
                    <match url="^(rest/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="rest-doc" stopProcessing="true">
                    <match url="^(rest-doc/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="debugger" stopProcessing="true">
                    <match url="^(debugger/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="oauth" stopProcessing="true">
                    <match url="^(oauth/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
                <rule name="p" stopProcessing="true">
                    <match url="^(p/)(.*)" />
                    <action type="Rewrite" url="http://localhost:8080/{R:1}{R:2}" />
                </rule>
            </rules>
        </rewrite>
        <staticContent>
            <mimeMap fileExtension=".mxf" mimeType="text/xml" />
            <clientCache cacheControlMode="DisableCache" />
        </staticContent>
    </system.webServer>
</configuration>
```

## Preserving the Host Header{#preserve-header}

To make sure the correct application root URL is used within your web services, you must make sure the host header contains the original host header from the client request. To make sure the host header is preserved, follow either of these steps.

1. Via IIS Manager:

    1. Select the **Server** in the **Connections** pane.
    2. Double-click the **Configuration editor** feature.
    3. In the **Section** drop-down menu, select *system.webServer/proxy*.
    4. Set the **preserveHostHeader** option to *True*.
    5. In the **Actions** pane, click **Apply**.

2. Via command prompt:

    1. Click **Start**, and then click **All Programs**.
    2. Click **Accessories**, and then click **Command Prompt**.
    3. Execute the following command from the command prompt:

        ```batch
        cd %windir%\system32\inetsrv
        ```

    4. Enter:

        ```batch
        appcmd.exe set config -section:system.webServer/proxy /preserveHostHeader:"True" /commit:apphost
        ```

## Troubleshooting

### IIS

When configuring IIS it can seem like you have done everything right but it just doesn't seem to work. A guide to troubleshooting IIS is available here: [Troubleshooting IIS](/developerportal/deploy/troubleshooting-iis/).

### Service Console Shows Wrong Service Status

If you are using [Powershell cmdlets](/developerportal/deploy/automate-mendix-deployment-on-microsoft-windows/#powershell) to change the status of your service (for example, using `Start-MxApp`) the service status will not update automatically in the Service Console GUI. The correct status will be shown when you restart the Service Console.

## Read More

* [On-Premises](/developerportal/deploy/on-premises-design/)
