---
title: "Software Composition"
linktitle: "Software Composition"
url: /control-center/software-composition/
description: "Describes the Software Composition page in the Mendix Control Center."
weight: 20
---

## Introduction

A Mendix app can consist of the Mendix Model (which includes pages, domain model, microflows, etc.), custom Java, and JavaScript. Additionally, it can use reusable components such as standard marketplace modules, widgets, Java libraries, npm packages, and the runtime version. These reusable components are dependencies, namely, components you are dependent on for your Mendix app to run.  

Over time, these dependencies can become deprecated, outdated, or vulnerable. Enterprises also have policies on which of these reusable components can or cannot be used based on support, license, etc. It is important to have an easily accessible, clear view of component dependencies through the development lifecycle in order to address any security finding raised by your admins or security teams.

To enable this, the **Software Composition** page in Control Center provides visibility into the component dependencies in each app environment. The components displayed here will be based on the [Software Bill of Materials (SBOM)](/refguide/sbom-generation/).

{{% alert color="warning" %}}Advanced software composition capabilities are currently available to all. In the future, access to these capabilities will be subject to your license.{{% /alert %}}

## Prerequisites {#prerequisites}

To be able to see the software composition information, make sure that you meet the following prerequisites:

* Software Bill of Materials (SBOM) generation and the associated Software Composition capabilities are compatible with the following versions of Studio Pro: 9.24.26 and above, 10.6.12 and above, 10.12.3 and above. 

    {{% alert color="warning" %}}Make sure you upgrade to a compatible Studio Pro version to continue to use Software Composition. Previously supported Studio Pro versions (9.24.22 to 9.24.25, 10.6.9 to 10.6.11, 10.10.0 to 10.12.2, and 10.13) will no longer result in SBOM generation and visibility in Software Composition. Any historical data within Software Composition remains accessible regardless of the upgrade.{{% /alert %}}

* Software composition visibility is only possible for deployment packages created via the platform services. It is not available if you manually upload the locally-created deployment package. SBOMs are created behind the scenes for each deployment package. For more information, see [Create Deployment Package](/refguide/create-deployment-package-dialog/).

* You must be using free or licensed Mendix Cloud or Mendix Cloud Dedicated, or Mendix on Kubernetes. 

* If your deployment package was deployed before June 14, 2024, you must create and deploy a new deployment package in order to get the software composition information populated on this page.

## Software Composition Generation {#software-composition-generation}

A software bill of materials (SBOM) is generated in the following circumstances:

* When a new deployment package with the compatible Mendix Runtime version is created via the Mendix Portal
* Using the **App** > **Tools** > **Generate Bill of Materials** menu option in Studio Pro 10.18 and above

Click **View build output** in the deployment package details in the Mendix Portal to see the log details. For details of SBOM generation, see [SBOM Generation](/refguide/sbom-generation/).

You can find the component dependencies for each non-expired, deployment package in the [Software Composition](/developerportal/deploy/software-composition/) page of **Apps** in the Mendix Portal. 

After the creation of a deployment package, it may take up to a day for the **Software Composition** page to become visible. Mendix is working to improve the performance on this front.

## Overview {#overview}

On the **Overview** tab, you can see a list of all the deployed apps and their environments, if applicable. You can also see the number of findings for each severity level, as configured on the [Scoring Criteria](#scoring) tab.

{{< figure src="/attachments/control-center/security/software-composition/software_composition_overview.png" >}}

### Insights

The **Insights** cards display the number of findings of each severity level. Each card also includes an indication of how that number has evolved over the past 28 days, under the form of a percentage.

For details on severity levels, refer to the [Scoring Criteria](#scoring) section.

### App List

The following options are available above the list of apps:

* A search box to search for information within the list.
* A filter to display apps based on the type of cloud. 
* The {{% icon name="office-sheet" %}}**Export All** option, which allows you to export all the information in the list to an Excel file.

The app list contains the following information:

* **App Name** — The name of the app.
* **Environment** — The name of the environment.
* **Runtime** — The Mendix Runtime version.
* **Findings** — The number of findings of each type, color-coded according to severity level.
* **Technical Contact** — The Technical Contact of the app.
* **Target Cloud** — The type of cloud where the deployment package is deployed. Currently, the following types of cloud are supported:
    * Mendix Free Cloud
    * Mendix Cloud (including Mendix Cloud Dedicated)
    * Mendix on Kubernetes (connected)
* Column customization ({{% icon name="view" %}}) — You can customize the columns in the list by clicking the {{% icon name="view" %}} icon and selecting or deselecting options.
* **View details** — Clicking this opens the [Application Environment Summary](#app-env-summary) page, if it is available. The **View details** button is grayed out when an SBOM is not available for the selected application environment. Ensure you are on a compatible runtime version and have created a new deployment package in order to have components visible here.

To export the information corresponding to selected items in the list to an Excel file, select the checkboxes of the items in the list, then click **Selection Export** that appears at the bottom of the page.

### Application Environment Summary {#app-env-summary}

If you click **View Details** for an app in the list on the **Overview** tab, the **Application Environment Summary** page opens. This displays details about all the components used by the selected app.

At the top of the page, you can find the following information:

* The app name
* A color-coded summary of the findings
* The environment name
* The Mendix Runtime version
* The Technical Contact
* The type of cloud where the deployment package is deployed

In the upper, right corner of the page, you can click {{% icon name="download-bottom" %}}**SBOM** to download the software bill of materials (SBOM).  

A software bill of materials (SBOM) is a *.json* file in the CycloneDX format. It contains a description about the Mendix app and the components (dependencies) put into it. For more information, see [SBOM Generation](/refguide/sbom-generation/).

Different versions of Studio Pro support different component dependencies. For details on component dependencies supported per version, refer to the [Supported Features](/refguide/sbom-generation/#supported-features) section in *SBOM Generation*.

The page is divided into two tabs: **Findings** and **Component Usage**.

#### Findings

The **Findings** tab lists all the findings which impact that particular app environment.

{{< figure src="/attachments/control-center/security/software-composition/findings.png" >}}

The following options are available above the list:

* A search box to search for information within the list.
* A filter to display list items according to the type of finding. 
* The {{% icon name="office-sheet" %}}**Export All** option, which allows you to export all the information in the list to an Excel file.

The finding list contains the following information:

* **Severity** — The severity of the finding related to that component.
* **Finding Type** — The type of finding, which can be **Outdated** or **Deprecated**.
* **Component** — The component used in the app.
* **Version** — The version of the component that is used in the app.
* **Type** — The type of component.
* **Support type** — This shows the support type of the Marketplace component. It can be **Mendix**, **Partner**, or **Community**. For more information, refer to [Content Support Categories](/appstore/marketplace-content-support/#category).
* **Age** — The number of days that the finding has been applicable, computed as follows:

    * Deprecated components: the current date - the date when the component was deprecated    
    * Outdated components: the current date - the publish date of the first higher runtime compatible version

* Column customization ({{% icon name="view" %}}) — You can customize the columns in the list by clicking the {{% icon name="view" %}} icon and selecting or deselecting options.

#### Component Usage

The **Component Usage** tab displays a detailed view of all components used within the app.

{{< figure src="/attachments/control-center/security/software-composition/component_usage.png" >}}

The following options are available above the list:

* A search box to search for information within the list.
* A filter to display apps based on the type of cloud. 
* The {{% icon name="office-sheet" %}}**Export All** option, which allows you to export all the information in the list to an Excel file.

The component usage list contains the following information:

* **Component** — The name of the component.
* **Version** – The version of the component that is being used.
* **Type** — The type of component, which can be one of the following:
  
    * **Module** — Standard marketplace module imported from the Marketplace, such as [Community Commons](https://marketplace.mendix.com/link/component/170).
    * **Widget** — User interface elements downloaded from the Marketplace, such as [Charts](https://marketplace.mendix.com/link/component/105695).
    * **Framework** — The Mendix Runtime version, for example 10.12.0
    * **Jar** — Java libraries imported into your app using [Managed Dependencies](/refguide/managed-dependencies/), or those manually added in the **userlib** folder depending on the Studio Pro version used, such as `org.apache.commons.io`.
    * **npms** — `npm` libraries that are used in your [JavaScript actions](/refguide/javascript-actions/).
    * **Unknown** — When the type of the component is none of the above and hence undetermined.
    
* **Support type** – The support type of the Marketplace component. This can be **Mendix**, **Partner**, or **Community**.    
  For more information, refer to [Content Support Categories](/appstore/marketplace-content-support/#category).
* **License** – The end-user license for the component.
* **Latest version** – The latest version of the component.
* **Marketplace** – Whether the component is **Public** or **Private**. A public component is available to the whole Mendix community in the Marketplace, while a private component is available only via your [Company Content](/appstore/home-page/#company-content) page.
* **Latest Runtime Compatible Version** — The most recent runtime version to which the component is compatible.
* **Publisher** – The name of the organization that published the component.
* Column customization ({{% icon name="view" %}}) – You can customize the columns of the list by clicking the {{% icon name="view" %}} icon and selecting or deselecting options.

To export the information corresponding to selected items in the list to an Excel file, select the checkboxes of the items in the list, then click **Selection Export** that appears at the bottom of the page.

## Components {#all-components}

The **Components** tab gives an overview of all the unique components used across your app landscape. 

{{< figure src="/attachments/control-center/security/software-composition/all_components.png" >}}

### Insights

The **Insights** cards display the following details:

* **Marketplace** — The number of private and public Marketplace components used throughout your apps.
* **Support type** — The number of Marketplace components divided into content support categories.
* **Summary** — The number of findings in each severity category, along with their evolution (increase or decrease) over the past 28 days.

### Component List

The following options are available above the list of components:

* A search box to search for information within the list.
* A filter to display apps based on the type of cloud. 
* The {{% icon name="office-sheet" %}}**Export All** option, which allows you to export all the information in the list to an Excel file.

The component list contains the following information:

* **Component** — The name of the component.
* **Type** — The type of component, which can be one of the following:
  
    * **Module** — Standard marketplace module imported from the Marketplace, such as [Community Commons](https://marketplace.mendix.com/link/component/170).
    * **Widget** — User interface elements downloaded from the Marketplace, such as [Charts](https://marketplace.mendix.com/link/component/105695).
    * **Framework** — The Mendix Runtime version, for example 10.12.0
    * **Jar** — Java libraries imported into your app using [Managed Dependencies](/refguide/managed-dependencies/), or those manually added in the **userlib** folder depending on the Studio Pro version used, such as `org.apache.commons.io`.
    * **npms** — `npm` libraries that are used in your [JavaScript actions](/refguide/javascript-actions/).
    * **Unknown** — When the type of the component is none of the above and hence undetermined.
    
* **Support type** — The support type of the Marketplace component. This can be **Mendix**, **Partner**, or **Community**.    
  For more information, refer to [Content Support Categories](/appstore/marketplace-content-support/#category).
* **Version** — The version of the component that is being used.
* **Findings** — This shows the number of findings of each type, color-coded according to severity level.
* **License** — The end-user license for the component.
* **Marketplace** – Whether the component is **Public** or **Private**. A public component is available to the whole Mendix community in the Marketplace, while a private component is available only via your [Company Content](/appstore/home-page/#company-content) page.
* **Apps using component** – The number of apps where the component is used.
* **Latest version** — The latest version of the component.
* **Publisher** — The name of the organization that published the component.
* **View details** — Clicking this opens the [Component Details](#component-usage) page.
* Column customization ({{% icon name="view" %}}) – You can customize the columns of the list by clicking the {{% icon name="view" %}} icon and selecting or deselecting options.

To export the information corresponding to selected items in the list to an Excel file, select the checkboxes of the items in the list, then click **Selection Export** that appears at the bottom of the page.

### Component Details

If you click **View details** for a component on the **Components** tab, the **Component Details** page opens.

#### Findings

The **Findings** tab lists all the findings which impact that particular component.

{{< figure src="/attachments/control-center/security/software-composition/components_findings.png" >}}

The following options are available above the list:

* A search box to search for information within the list.
* A filter to display list items according to the type of finding. 
* The {{% icon name="office-sheet" %}}**Export All** option, which allows you to export all the information in the list to an Excel file.

The finding list contains the following information:

* **Severity** — The severity of the finding related to that component.
* **Finding Type** — The type of finding, which can be **Outdated** or **Deprecated**.
* **App Name** — The app in which the component is identified as a vulnerability.
* **Environment** — The name of the environment where the app is running.
* **Target Cloud** — The type of cloud where the deployment package is deployed.
* **Age** — The number of days that the finding has been applicable, computed as follows:

    * Deprecated components: The current date - The date when the component was deprecated    
    * Outdated components: The current date - The publish date of the first higher runtime compatible version

* Column customization ({{% icon name="view" %}}) — You can customize the columns in the list by clicking the {{% icon name="view" %}} icon and selecting or deselecting options.

#### Component Usage

The **Component Usage** tab displays a detailed view of all apps and environments where the component is used.

{{< figure src="/attachments/control-center/security/software-composition/components_comp_usage.png" >}}

The following options are available above the list:

* A search box to search for information within the list.
* A filter to display apps based on the type of cloud. 
* The {{% icon name="office-sheet" %}}**Export All** option, which allows you to export all the information in the list to an Excel file.

The component usage list contains the following information:

* **App Name** — The name of the app where the component is used.
* **Environment** — The name of the environment where the app using the component is deployed.
* **Runtime** — The runtime version to which the component is compatible.
* **Target Cloud** — The type of cloud where the deployment package is deployed.
* **Technical Contact** — The Technical Contact of the app.
* Column customization ({{% icon name="view" %}}) — You can customize the columns in the list by clicking the {{% icon name="view" %}} icon and selecting or deselecting options.

## Scoring Criteria {#scoring}

The **Scoring Criteria** tab allows you to adjust the conditions and severity for each type of finding. A finding is defined as a vulnerability identified in the components of an app. The settings on this tab determine how each such vulnerability is determined for apps, environments, and components.

{{< figure src="/attachments/control-center/security/software-composition/scoring_criteria.png" >}}

This is the information available on the **Scoring Criteria** tab:

* **Finding Type** — The type of vulnerability that components can have, along with its definition. This can be **Outdated** or **Deprecated**.
* **Severity** — The severity level of a finding. These are color-coded for easy identification throughout Software Composition, and can be:

    * **Critical**
    * **High**
    * **Medium**
    * **Low**

  For outdated components, you can adjust all severity levels.    
  For deprecated components, you can choose which severity level to assign.

* **Conditions** — Configure the minimum number of days that the condition must be true for the finding to become applicable.    
  For example, you can set a value of 100 for the **Critical** severity level if you want components that have been outdated for 100 days to be marked as **Critical**.
* **Status** — Toggle the selector on or off depending on which levels of severity you want to see.    
  For example, if you are only interested in **Critical** findings, you can toggle off all the rest.
