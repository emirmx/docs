---
title: "Upgrading from Mendix Studio Pro 10 to 11"
url: /refguide/upgrading-from-10-to-11/
linktitle: "Upgrading from Studio Pro 10 to 11"
weight: 30
description: "Provides details on upgrading your app from Studio Pro 10 to Studio Pro 11, including sections on converting your app and deprecated features."
---

## Introduction

Mendix Studio Pro 11 is a [major version](/releasenotes/studio-pro/lts-mts/#major-version) release that provides powerful tools for building your apps and brings a host of improvements and fixes. For the full list of changes, see the [Studio Pro 11 release notes](/releasenotes/studio-pro/11.0/).

### Upgrading from Older version to Studio Pro

If your app is on a Studio Pro version below 10, you must upgrade in order of version. 

Moving from Mendix Studio Pro 8 to 9
Upgrading from Mendix Studio Pro 9 to 10
If your app is running on Mendix Cloud, you can check what version the app is currently on by referring to the Control Center dashboard. Alternatively, contact your Customer Success Manager to find out how to check the Mendix version of your app.

### Steps Required for Upgrade

We recommend to follow the following steps during your upgrade:

1. Back up your app.
1. Upgrade to the latest patch of Studio Pro 10.24.
1. Upgrade Widgets, Modules, Marketplace Components, Templates, Connectors.
1. Fix deprecations and test your app.
1. Upgrade app to Mendix 11.

## Backing Up Your App

Make sure you have either committed your latest changes to Team Server, or created a backup of your local app before you start the conversion.

## Preparing Your App in Studio Pro 10.24

It is recommended that you first upgrade your app in Studio Pro 10.24, so you can upgrade to Mendix 11.

To prepare your app in Studio Pro 10.24, follow these steps:

1. Download the latest patch release of Studio Pro 10.24.
1. Open your app in Studio Pro 10.24.
1. Allow Studio Pro to update and convert the app.

## Upgrading Widgets, Modules, and Marketplace Components {#upgrade-widgets}

Now that your app is upgraded, you are ready to use the modules, widget and marketplace components. To minimize the chance of problems, it is recommended to upgrade to the versions that are compatible with the 10.24 LTS, and the next major version.

In general, you should not remove and re-import modules unless this is recommended in the component’s release notes. If you do remove and re-import a component, you may lose data or configuration related to the component.

## Testing and Completing the Upgrade

After the upgrade of your marketplace content, take the next steps:

1. Fix any deprecation warnings you see in development in Studio Pro, as well as in the Mendix.
1. Runtime using your console and browser console.
1. Review the major changes in the sections below
1. Run your app, test all functionality, and ensure it works without error
1. Back up or commit your 10.24 app so you can return to it if necessary.

Your app is now ready to be upgraded to Mendix 11. You can now close the app in Studio Pro 10.

## Upgrading Your App in Studio Pro 11

Open your app in Studio Pro 11 and allow Studio Pro to update and convert your app to version 11. Mendix will update your app automatically.


## Notable and Breaking Changes

* Studio Pro 10.21 and above requires your application to use Java 21. The Java version of an application can be configured in the runtime settings. Java 21 is available in 9.24.23 and above. Please consider the Java Version Migration guide for a list of changes between Java versions. For on-premises deployments, ensure that JDK 21 is installed in the environments where Mendix 10 applications are deployed.
* Atlas 3 to Atlas 4 (opt-in upgrade). A lot less manual work compared to Atlas 2 to 3 but still will require some migration Takuma Gottschewski
* In Published REST operation where Accept header is sent as generic */* or application/* then XML would be returned instead of the intended JSON.  This has been changed to always send JSON.
* Consumed REST Service: removed app setting 'Automatically encode parameter values in Send REST request microflow activities' Before Mendix 10.21 Consumed REST Services would not encode parameters while modeling, but parameters would be encoded when used from a Send REST request activity. This gave some confusion, so in version 10.21 a setting was introduced to enable or disable this behavior. This setting has been removed in Mendix 11. For consistency parameters are now never automatically encoded both from the Consumed REST Service and the Send REST Request activity. If you want encoding for parameters, you need to add that yourself.Legacy migration of System.Imagethumbnail files has been removed. This was introduced in Mendix 6. If you are migrating from a Mendix 6 application then first migrate to Mendix 10 and run the application in production to finish migration, before migrating further to Mendix 11.
* The JVM parameters -Djava.security.manager and -Djava.security.policyare no longer supported, as the Java Security Manager is deprecated and non-functional in Java 21.
* The Task Queue setting 'System context tasks' ("Runtime" tab in App Settings) is removed. This setting was deprecated since Mendix 9.6. Queued tasks are now always executed in a context equivalent to the one in which they were created. If an app still has this setting enabled, this results in a consistency check error. There is a quick fix option available in the error's context menu to disable the setting and resolve the error.
* The Core.getRuntimeVersion()no longer contains the build number. The format used to be <major>.<minor>.<patch>.<build>, now it will be <major>.<minor>.<patch>. 
* The type parameter for the EventActionInfoclass has been removed, as it was superfluous. This means EventActionInfo<..> info = new EventActionInfo<..>(..) won't compile anymore. The <..> parts need to be removed.
* String concatenation in expressions for empty/null values has changed. These values are no longer concatenated like 'null', instead they are omitted from the string. The old behavior can be achieved by using an if-else expression (e.g. 'Value: ' + (if $value != empty then $value else 'null') instead of 'Value: ' + $value).
* The default value of the custom runtime setting DataStorage.OptimizeSecurityColumns was changed to true.
* XPath API now uses limited XPath by default. We added a setting to make it not limited (“limited“ means that arithmetic operators are not allowed as those are unsafe)
* Following the MariaDB JDBC driver default, if you use MariaDB or MySQL with a DatabaseJdbcUrl we no longer accept jdbc:mysql: as a protocol. Use jdbc:mariadb: instead.
* For MariaDB and MySQL, we no longer set the sql_mode driver parameter. Unless overridden in DatabaseJdbcUrl, the database default will be used.
* SELECT * in combination with UNION and ORDER BY is no longer allowed in Runtime as it leads to queries that are not accepted by most database engines
OR (if we fix the issue DAT-4032)
* We fixed the issue where SELECT * in combination with UNION and ORDER BY would fail on most database engines
* When COALESCE function in OQL has attributes of different numeric types, the result type is defined according to type precedence. Before, the result type would match the type of the first argument.
