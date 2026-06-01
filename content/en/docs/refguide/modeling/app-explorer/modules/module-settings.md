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
This setting is available only for add-on and solution module types.
{{% /alert %}}

This is the version number of the module. The version should be a semantic version (meaning, it should consist of at least three parts: major, minor, and patch version). For more information on semantic versions, see [Semantic Versioning](https://semver.org/).

Mendix recommends setting a new version every time changes are made to the module.

## Package {#package}

{{% alert color="info" %}}
This setting is available from Studio Pro 11.12 and above.
{{% /alert %}}

The **Package** section in the **General** tab displays package identification information for your module. This information is used to track modules across updates and enable reliable module upgrades.

### How Package Management Works

Studio Pro 11.12 introduces improved package management to enable reliable module tracking and updates. The key improvements are delivered through a new `manifest.json` file format in module packages (*.mpk* files):

* **Package identification** – Each module receives a package ID that uniquely identifies it throughout all versions. This allows Studio Pro to reliably track modules across updates, even if the module name changes.
* **Document mapping** – For modules with package IDs, a GUID mapping is stored for all documents in the module. This enables Studio Pro to match documents between versions even if they have been renamed, making updates and merges more reliable.
* **Package integrity** – Each package includes a checksum (SHA-256 hash) that verifies the integrity of the package contents.
* **Metadata tracking** – The manifest includes information about the package name, version (following semantic versioning), type, and the Mendix metamodel version used to create it.

For more information on how to use package management when updating modules, see [Updating Marketplace Modules](/refguide/updating-marketplace-modules/).

#### Automatic Package ID Assignment

When you open an app in Studio Pro 11.12 or above, each module automatically receives a package ID. The package ID is a GUID that is generated deterministically:

* If a module has a Marketplace component ID (for example, 170 for Community Commons), that ID is used to generate the package ID.
* If a module does not have a Marketplace component ID, the package ID is generated based on your app ID and module ID. This ensures the ID remains consistent across devices when multiple developers work independently.

The package ID remains constant throughout all versions of the module. It is stored in the module's `manifest.json` file, which is included in the root of the module package (*.mpk* file). This allows Studio Pro to identify the module even if its name changes.

### Module ID

The **Module ID** is the unique identifier for your module. This ensures update compatibility between modules. If two modules share the same ID, you can use one to update the other. If the IDs differ, Studio Pro treats them as distinct modules and does not allow updates between them.

Click the pencil icon next to the Module ID to change it if needed.

{{% alert color="warning" %}}
Modifying the Module ID prevents others from updating their versions of your module from its original source. You will still be able to update your module from its original source.
{{% /alert %}}

You can also generate a new Module ID by clicking the **Generate New ID** button. This is useful when you want to create a fork or derivative of the module that is treated as a separate module.

### Source ID

The **Source ID** identifies the original module this module is based on. Studio Pro uses this to trace a module back to its origin. If two modules share the same Source ID, Studio Pro recognizes them as versions of the same original module.

In most cases, you do not need to change this value. You only need to change the Source ID in the edge case where you are forking a module (for example, if you downloaded Community Commons and want to release a custom company version of it). In this scenario, keep the Source ID set to the original module so you can still receive updates from the original source, while using a different Module ID for your forked version.

Click the pencil icon next to the Source ID to change it if needed.

{{% alert color="warning" %}}
Only modify the Source ID if you are intentionally creating a fork of an existing module and understand the implications.
{{% /alert %}}

### Checksum

The **Checksum** displays a verification checksum for the module. This is a read-only SHA-256 hash calculated from the module package contents. The checksum serves two purposes:

* **Integrity verification** – Ensures the module package has not been corrupted or tampered with
* **Version identification** – Acts as a unique identifier for a specific version of the module, allowing comparison of two different binaries for equality

The checksum is automatically calculated when you export the module and is stored in the module's `manifest.json` file.

## Exporting Modules with Package Management

When you export a module package (by right-clicking the module in the App Explorer and selecting **Export module package**), Studio Pro asks whether to keep the existing Module ID or generate a new one:

* **Generate New ID** – Replaces the current Module ID with a newly generated one and updates the Source ID to the previous Module ID, then proceeds with the export. Use this option if you are creating a fork or derivative of the module.
* **Keep ID** – Proceeds with the export using the existing Module ID. Use this option for regular updates to the same module.

The Module ID ensures that consumers can update their version of the module with your new version. If you change the Module ID, Studio Pro treats it as a different module.

## Read More

* [Modules](/refguide/modules/)
* [Updating Marketplace Modules](/refguide/updating-marketplace-modules/)
* [Configure Add-on and Solution Modules for Publishing](/refguide/configure-add-on-and-solution-modules/)
* [Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/)
* [Creating Solutions](/appstore/creating-content/sol-solutions-guide/)
