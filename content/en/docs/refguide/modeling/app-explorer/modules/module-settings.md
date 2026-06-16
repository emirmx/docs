---
title: "Module Settings"
url: /refguide/module-settings/
weight: 10
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

Module settings allow you to set Java managed dependencies, choose the type of the module, and set a version for certain module types.

To open module settings, double-click **Settings** in the required module.

{{< figure src="/attachments/refguide/modeling/app-explorer/modules/module-settings/settings.png" class="no-border" >}}

## Java Dependencies

You can add managed dependencies for each module on the **Java Dependencies** tab. For more information, see [Managed Dependencies](/refguide/managed-dependencies/).

{{< figure src="/attachments/refguide/modeling/app-explorer/modules/module-settings/module-settings-java-dependencies.png" class="no-border" >}}

## Export

Select the **Export** tab:

{{< figure src="/attachments/refguide/modeling/app-explorer/modules/module-settings/module-settings-export.png" class="no-border" >}}

## Configure

Configure section contains basic settings of the module.

### Module Type {#module-type}

There are three types of modules, and the choice of type depends on the purpose of the module. You can choose one the following:

* [App module](#app-module)
* [Add-on module](#add-on-module)
* [Solution Module](#solution-module)

{{% alert color="warning" %}}
If you switch from an add-on or solution module to an app module, or from an app module to a solution or an add-on module, the module data is lost once the new version of the app is deployed. Switching from an add-on to a solution module and from a solution to an add-on module is possible without loss of data. 
{{% /alert %}}

#### App Module {#app-module}

An app module is a standard way of structuring your app. Use app modules to distinguish between functional domains: create an app module for each relevant domain and put all pages, microflows, entities, and other documents in one place.

An app module is exported as a package file (*.mpk* ) that includes the full source code of the module.

#### Add-on Module {#add-on-module}

An add-on module is a standalone module that is not dependent on other modules. It is used as a separate element (for example, as a connector). 

An add-on module is exported as a module file (*.mxmodule*) that only exposes the elements with the **Usable** export level. For more information on export levels, see [Configuring Add-on and Solution Modules for Publishing](/refguide/configure-add-on-and-solution-modules/). Its source cannot be inspected by the consumer of the module.

If you are creating functionality that can be exported and used by other users separately and independently of the rest of the app, you can set your module to an add-on type.

When the module is set as the add-on module, it gets the letter **A** as an icon.

#### Solution Module {#solution-module}

Solution modules are only used for developing a solution and are an inseparable part of it. The set of solution modules used for the solution form the solution core. Solution modules are exported as a solution package and distributed as a solution to multiple consumers. For more information, see [Creationg Solutions](/appstore/creating-content/sol-solutions-guide/) in the *Marketplace Guide*.

When the module is set as the solution module, it gets the letter **S** as an icon.

### Module Version

{{% alert color="info" %}}
From Studio Pro 11.12 and above version is available for all module types. For earlier versions of Studio Pro version is available only for Add-on and Solution module types. 
{{% /alert %}}  

This is the version number of the module. The version should be a semantic version (meaning, it should consist of at least three parts: major, minor, and patch version). For more information on semantic versions, see [Semantic Versioning](https://semver.org/).

Mendix recommends setting a new version every time changes are made to the module.

## Package {#package}

{{% alert color="info" %}}
This section is available from Studio Pro 11.12 and above.
{{% /alert %}}

Studio Pro 11.12 introduces improved package management to enable reliable module tracking and updates.

For more information on how to use package management when updating modules, see [Updating Marketplace Modules](https://github.com/mendix/docs/blob/development/refguide/updating-marketplace-modules).

The key improvements are delivered through new module properties and a new `manifest.json` file format in module packages (*.mpk* files):

- **Package identification** – Each module receives a Module ID that uniquely identifies it throughout all versions. This allows Studio Pro to reliably track modules across updates, even if the module name changes.
- **Package integrity** – Each package includes a checksum (SHA-256 hash) that is used to verify the integrity of the package itself and of the imported module in the App.
- **Metadata tracking** – The manifest includes information about the package name, version (following semantic versioning), type, and the Mendix metamodel version used to create it.

The **Package** section on the **General** tab shows package identification information for the module. Studio Pro uses this information to track modules across versions, which is the foundation for more reliable module updates.

### Module ID {#module-id}

The **Module ID** is a unique identifier for the module that stays the same across all versions of the module. It is used to determine whether two module packages represent the same module, and is therefore the basis for module update compatibility: if two modules share the same Module ID, Studio Pro can update one with the other; if the IDs differ, Studio Pro treats them as distinct modules.

#### Automatic Module ID Assignment

The Module ID is assigned automatically and is stable across devices, so multiple developers working on the same module independently end up with the same value:

* For a module imported from the Marketplace, the Module ID is derived from the Marketplace component ID.
* For any other module, the Module ID is derived from the App ID and the module name. This ensures the ID remains consistent across devices when multiple developers work independently.

When you open an existing app in Studio Pro 11.12 or above, every module that does not yet have a Module ID receives one automatically. No action is required from you.

#### Manual Module ID override

In some cases you might want to override the automatically assigned Module ID. To change it, click the pencil icon next to it. A dialog opens where you can enter a new value.

{{% alert color="warning" %}}
Changing the Module ID may break existing installations that depend on this module. Only override this value if you know what you are doing.

Marketplace will not allow you to publish a module with ModuleID that is already used by another publisher.

{{% /alert %}}

### Checksum {#checksum}

The **Checksum** is a read-only SHA-256 hash that uniquely identifies a specific version of the module package. It serves two purposes:

* **Integrity** – This checksum is a hash of the contents of the original module package that StudioPro can use to detect if there were any user modifications after the module was imported.
* **Version identification** – Lets you compare two module packages for equality without inspecting their contents.

The checksum is shown only for modules that were imported from a module package. For modules created directly in your app, the checksum is calculated when you export the module and is not displayed in the **Package** section.

### Package Manifest {#package-manifest}

When you export a module package (an *.mpk* file), Studio Pro 11.12 and above adds a `manifest.json` file to the package alongside the existing metadata. The manifest contains:

* The Module ID and Module name
* The Module version
* The Checksum of the package
* The Mendix metamodel version used to create the package
* The list of files included in the package

The legacy `package.xml` metadata file is still included for backward compatibility.



## Read More

* [Modules](/refguide/modules/)
* [Update modules](refguide/updating-marketplace-modules)
* [Configure Add-on and Solution Modules for Publishing](/refguide/configure-add-on-and-solution-modules/)
* [Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/)
* [Creating Solutions](/appstore/creating-content/sol-solutions-guide/)
