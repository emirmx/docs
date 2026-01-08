---
title: "Setting up Teamcenter Configuration using Teamcenter Connector 2512.0.0 or higher"
url: /appstore/modules/siemens-plm/teamcenter-connector/setup-guide/
weight: -1
description: "Describes the steps to configure the connection to your Teamcenter instance using Teamcenter Connector 2512.0.0 or higher."
---

## Dependencies
* Make sure all dependencies are installed.
* Download the Teamcenter Connector from the Mendix Marketplace.
* Assign the TcConnector Module role Administrator to the user role that will configure the connection to the Teamcenter instance.
* Assign the TcConnector Module role User to the user role that will authenticate to Teamcenter.
* Add NAV_AdminHomePage to the Navigation so that the admin can configure the Teamcenter Connection.


## Connect your Mendix application to Teamcenter
### Configure your Mendix application
* Make sure all dependencies are installed.
* Download the Teamcenter Connector from the Mendix Marketplace.
* Assign the TcConnector Module role `Administrator` to the user role that will configure the connection to the Teamcenter instance.
* Assign the TcConnector Module role `User` to the user role that will authenticate to Teamcenter.
* Add `NAV_AdminHomePage` to the Navigation so that the admin can configure the Teamcenter Connection.

{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/navigation.png">}}

### Set up the connection
* Login to your Mendix application as the `Admin` user role.
* Use the `NAV_AdminHomePage` button to navigate to the overview page of all Teamcenter Configurations.

{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-new-configuration.png">}}
* Click on the **+ New configuration** button to create a new configuration.

All configurations require the following fields:
* **Configuration Name**
  * The configuration name is a unique identifier for the configuration that can be freely chosen.
  This can be used in a subsequent microflow to identify which environment to connect to.
* **Teamcenter Host URL**
  * The Teamcenter Host URL typically ends with `/tc`.
* **Teamcenter FMS URL**
  * The URL of your Teamcenter File Management Server.
* **Authentication Method**
  * The method of authenticating to the Teamcenter instance. 
Use:
    * `Credentials`, if you use a username and password to login.
    * `Teamcenter SSO`, if you use SSO on an On-premises environment.
    * `Teamcenter X SSO`, if you have a Teamcenter X environment.
* **Active**
  * If this is set to True, this configuration is used to authenticate the user.

If your Teamcenter instance is using SSO additional steps are required, see [Teamcenter SSO](#teamcenter-sso) and [Teamcenter X SSO](#teamcenter-x-sso). Follow these steps before continuing.

Click on the **Save** button.

### Test the connection
Depending on the connection type, you can test your access to Teamcenter from the Mendix application using the following approaches.

* If you want to use SSO with user provisioning for anonymous users
  * Use the Teamcenter SSO button from the login page as setup in the user provisioning section.
{{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-sso-button.png">}}
* If you want to use SSO with user provisioning for logged in users
  * Add `NAV_UserLogin` to the navigation
    * If you want to connect to multiple instances of Teamcenter at the same time, use `NAV_UserLoginMultipleActive` instead.
  * Login to the Mendix Application as a Mendix User
  * Use the `NAV_userLogin` button to login to Teamcenter.

  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/navigation-login-user.png">}}

* If you want to use credentials-based login
  * Add `NAV_UserLogin` to the navigation
    * If you want to connect to multiple instances of Teamcenter at the same time, use `NAV_UserLoginMultipleActive` Instead.
  * Login to the Mendix Application as a User
  * Use the `NAV_userLogin` button to login to Teamcenter.

  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/navigation-login-user.png">}}

## Authentication specific configuration steps
### Teamcenter SSO
Teamcenter SSO is the setup of Teamcenter where Single Sign On is set up for a Teamcenter instance that is not a Teamcenter X environment.

To set up the connection, your application needs to be registered with the identity provider.

To see how to register an application or find the information needed to configure the settings, see [How to register your application for Teamcenter SSO](#register-your-application-for-teamcenter-sso).

The following settings need to be provided to make a successful connection. 
* `SSO Login Server URL`
  * The endpoint where your authentication request is sent. It acts as the main entry point for users trying to log in using SSO. This URL is typically associated with the Identity Provider (IdP) and is responsible for handling login requests and directing users through the authentication process.
* `SSO Identity server`
  * The URL of the Identity Server where the Mendix application should be registered.
* `Teamcenter Application Id`
  * The existing Teamcenter Application ID obtained from the Teamcenter Security Services Identity Service configuration.
* `Mendix Application Id`
  * The `APPLICATIONID` of the registration for Mendix application at the Identity Server.

When using SSO, you need to make sure that Mendix accounts match Teamcenter users. Information on how to set this up can be found in section [User Provisioning for SSO](#user-provisioning-for-sso).

### Teamcenter X SSO
If the instance you are connecting to is a Teamcenter X instance, select Teamcenter X SSO. 

To connect to a Teamcenter X instance:
* Your Mendix application needs to be registered to Teamcenter X SSO.
* You must obtain the required fields from your CApS (Cloud Application Services) representative.
* You will need to provision users in Mendix.

This section explains all these steps.

Please work with your CApS representative to register your application to Teamcenter X SSO.

The following settings need to be configured to make a successful connection:
* `Teamcenter X Client ID`
  * The Application ID of your Teamcenter X instance
* `Mendix Client ID`
  * The Application ID of your Mendix application
* `Mendix Client Secret`
  * The Secret of your Mendix application
* `Token Exchange Client ID`
  * The Application ID of the Token Exchange server
* `Token Exchange Client Secret`
  * The Secret of the Token Exchange server
* `Well-known Configuration URL`
  * This URL should have a format like `https://your-teamcenter-x.com/.well-known/openid-configuration`

When using SSO, you need to make sure that Mendix accounts match Teamcenter users. Information on how to set this up can be found in section [User Provisioning for SSO](#user-provisioning-for-sso).

### User provisioning for SSO
When using SSO, it is essential that Mendix accounts are uniquely assigned to Teamcenter users. This section describes the process of assigning a Mendix user to a Teamcenter login. You either create your own or use one of the examples provided in the Teamcenter Connector. 
The examples are:
1. `EXAMPLE_UserProvisioningAnonymous`
This example microflow can be used with the Teamcenter SSO button on the `login.html` page (for instructions see [Add SSO login button to your login page](#add-sso-login-button-to-your-login-page)).
It assigns a Mendix Account with a username that matches the Teamcenter Login username with user role User. 
This usage is not allowed to be triggered by a logged in user.
2. `EXAMPLE_UserProvisioningNamed`
This example microflow is meant to be used by a Logged in user and keeps the same Mendix user logged in. It does not validate whether the  Mendix login matches the Teamcenter Login.
This usage is not allowed from the Login page of the application.

To enable either of those or implement your own, go to the `CUSTOM_UserProvisioning` microflow and add your preferred user provisioning microflow.


### Add SSO login button to your login page
To change the login page of your application, see [here](/howto/front-end/customize-styling-new/#custom-web). 
We provide an example that allows you to overwrite the `login.html` with one that contains the SSO login button. It can be found in your `resources\TeamcenterConnector`  folder. When copying over the content, also copy over the `xceleratorlogo.png` as the login button uses this image.  The button on this html page redirects to `/rest/tcsso/v1/login`.

### Register your application for Teamcenter SSO
To login to your Mendix application using Teamcenter SSO, you need to register your application in Teamcenter Security Services in Teamcenter Deployment Center. 

Note that your Teamcenter instance already needs to be configured to use SSO.

This guide provides abbreviated instructions on how to register your Mendix application. Configuring the settings requires assistance from your Teamcenter security expert. Please see the configuration on Siemens Support Center for Teamcenter Security Services. 

To register your Mendix application, perform the following steps.
* Go to Teamcenter Deployment Center.
* Select the correct environment.
* Go to tab **4. Components**.
* Select **Teamcenter Security Services (TcSS)**.
* Scroll down to the **SSO Application Ids** and note the **Teamcenter SSO Application ID**.
  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-sso-application-id.png">}}

* Scroll down to the **TcSS Login URL Settings** and note the **Login Service URL**.
  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-login-service-url.png">}}
* Scroll down to the **Teamcenter Application Registry**
* Add a new line for each environment of your application, generally at least `localhost` for development and `production` for deployment
  * Localhost example:
    * `APPLICATIONID: localhost`
    * `APPLICATIONROOTURL: http://localhost:8080`
    * `LDAP_USERNAME: uid`
  * Production example:
    * `APPLICATIONID: prod`
    * `APPLICATIONROOTURL: http://prod.example.com`
    * `LDAP_USERNAME: uid`
  * Teamcenter Extension example:
    * `APPLICATIONID: tce`
    * `APPLICATIONROOTURL: http://localhost:12345`
    * `LDAP_USERNAME: uid`
* See the screenshot below how your Teamcenter Application Registry may look like after adding the Mendix application.
  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-application-registry.png">}}

* Scroll down to the **TcSS Identity Service URL Settings** and note the **Identity Service URL**.
  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-identity-service-url.png">}}

Now you are ready to setup your configuration in Mendix. If you encounter any issues with the SSO registration of your application, please contact your Teamcenter security expert.

### Register your application for Teamcenter X SSO
Please work with your CApS representative to register your application to Teamcenter X SSO. You need your CApS representative to provide you with all the relevant information to configure your Teamcenter X SSO setting in Mendix.

## Troubleshooting
### Troubleshooting SSO
Setting up SSO can be a complex procedure. 
This section provides some guidance on what to check when you are unable to login via SSO.
1. Please check your Teamcenter configuration to ensure all fields are set correctly.
2. Set up SSO on your local machine first before working on a deployed version.  
To do so, you need to register for local development ([Teamcenter SSO](#register-your-application-for-teamcenter-sso) or [Teamcenter X SSO](#register-your-application-for-teamcenter-x-sso)).
If your setup works locally but not on your hosted environment, do follow the steps below again. If that does not solve the issue, see [here](#troubleshooting-sso-on-hosted-environments) for specific deployment issues.
3. Open the application in an incognito browser to make sure you are not logged in to a different instance, and you can see all intermediate steps.
4. Follow the steps in [Test the Connection](#test-the-connection).

**Question 1**
Can you see the login page of the identity provider?

* Option 1: Yes. Good then Continue to login and go to Question 2.
* Option 2: No, I see a `Page not found` message &rarr; Check whether you need a VPN and whether the `SSO Login URL` or `Well-known Configuration URL` are correct.
* Option 3: No, I see a `Something went wrong` page &rarr; Open the development tools of your browser and see whether there is an error in the console that explains what went wrong.

**Question 2**
What does your error (page) look like?
* Option 1: A blank screen or a `Page not found`.
  * Check the URL in your browser and match it against your applications URL. It should match your Application URL + `rest/tcsso/v1/callback`
* Option 2: This page
  {{< figure src="/attachments/appstore/platform-supported-content/modules/siemens/teamcenter-connector/configuration/teamcenter-error-page.png">}}
  * Check the logs in the Console of Mendix Studio Pro and adjust configuration where needed.
* Option 3: A Message: `Unable to Login to Teamcenter
The User was created but was unable to login to Teamcenter.`
  * Check the logs in the Console of Mendix Studio Pro and adjust configuration where needed.

### Troubleshooting SSO on hosted environments
Make sure your SSO setup on your local machine works first before working on a deployed version. If the local set up works, but it does not work on your hosted environment, the section below provides guidance on what to check when you are unable to login via SSO.

#### Unable to reach a page after login
The SSO setup of the Teamcenter Connector uses deep links to access the Mendix application. We use the following paths:
* `/rest`
* `/{url_prefix}`, where the default value for `{url_prefix}` is	`/p`.

If the url prefix is changed from `/p`, make sure `CONST_Deeplink_Url_Prefix` matches this url prefix.

The hosted environment needs to allow for these incoming connections. This is done via reverse proxy rules. To see how this can be set up, see [here](/developerportal/deploy/deploy-mendix-on-microsoft-windows/#reverse-proxy-rules).