---
title: "Best Practices for Adaptability"
url: /appstore/creating-content/sol-adaptability-best-practices/
linktitle: "Best Practices for Adaptability"
weight: 3
description: "Best practices for creating a solution for adaptation"
---

## Domain Model

The following sections describe best practices for your solution's domain model.

### Solution Module Defining the Data Model Core

Mendix recommends having the majority of your data model defined within solution modules in order to ensure stability. This also helps maintain a clear separation between which entities, attributes, and associations come from the publisher, and what is added during implementation. It also enables internal refactoring without having to take all customer instances into account.

### Extension Through Extension Entities (Composition Pattern or Specializations)

In general, Mendix recommends using a separate extension entity with a one-to-many or one-to-one relationship owned by the core entity. This enables adding additional attributes and associations during the implementation. Using a separate entity also allows for the introduction or removal of extension capabilities without large data migration. 

You can also use specializations. Mendix recommends considering this as an alternative when the composition pattern does not solve the need.  

This table outlines the main differences between composition patterns and specializations:

| Feature | Composition | Specializations |
| --- | --- | --- |
| **Easy to apply without data changes** | Yes, new extension objects can be created and linked to already existing core objects. | No, objects need to be re-created in order to change the entity/specialization type. |
| **Flexibility on security** | Core and extension have their own security rules. Core rules cannot be overridden. | Specialization allows for redefining security rules, even for entities that are defined inside solution modules. |
| **Multiple extension versions** (for example, `Vehicle` becomes both `Car` and `Train`) | Setup can be complex. | More suitable. |
| **Offline synchronization** | Fully supported. | Restrictions apply. For details, see [Offline Best Practices](/refguide/mobile/building-efficient-mobile-apps/offlinefirst-data/best-practices/#inheritance). |

### Example

Here is an example:

{{< figure src="/attachments/appstore/create-content/create-solutions/sol-adapt/sol-adaptability-best-practices/adaptability-domain-model-example.png" alt="Example of data model extensions"  class="no-border" >}}

In this example, the following details apply:

* **Company** is extended through composition, because that is the recommended approach and there are no requirements to reconsider. Additional entities, such as the **AccountManager**, can be created and linked.
* **Vehicle** is extended through specializations, because every customer can have multiple vehicle types, such as car, train etc.
* **Task** is extended through a specialization, because every customer has very unique requirements on entity access.
* **Logo** is not extensible.

## Making Part of the Logic Adaptable

You can make logic, that is microflows, nanoflows, and workflows, adaptable by placing documents into the open application modules. By using sub-flows, you can decide whether the entire flow can be adapted, or only specific parts of it.

| Purpose | Calling Flow | Called (Sub-)Flow |
| --- | --- | --- |
| Split (re-)usable core logic into (reusable) microflows | Solution module | Open application module |
| Make part of the core logic adaptable | Solution module | Open application module |
| Reuse core logic in an adaptable microflow | Open application module | Solution module |
| Split adaptable microflow | Open application module | Open application module (newly created flow) |

{{% alert color="info" %}}
All document types can be part of the solution module, but only nanoflows, microflows, and Java actions can currently be made usable (exposed via export level). Note that [solution modules and their export levels are deprecated](/refguide/module-settings/#solution-module).
{{% /alert %}}

## Creating an Adaptable UI

The same patterns that can be used for microflows can be used for making pages adaptable or partially adaptable. For this, you can use a combination of pages, editable layouts, and snippets, which can also be hidden.

| Object | Open App Module/UI Resource Module | Solution Module |
| --- | --- | --- |
| **Page** | Adaptable pages. | Core pages can currently be hidden and called through microflows and nanoflows. |
| **Snippet** | Make parts of the UI or the entire UI adaptable. | Core snippets can currently be hidden as long as it is for reuse in hidden core pages. |
| **Layout** | Main layout for the application. Use a solution-specific [master layout](/refguide/layout/#master-layout) to enable changing the layout of all pages by the customer. | Core layouts can currently be hidden and use an adaptable master layout. |
| **Building block** | Building blocks that are supposed to be used during adaptation need to reside in an open module. | Building blocks that are used during development of the core solution can currently be hidden. |
| **SASS files** | Define the theme and look and feel of your app. | No SASS definition support. Usage of existing theme and design properties as well as additional styling can be done through inline styling only. |

{{% alert color="info" %}}
The app title, favicon, and login pages are always adaptable, since they live at the app level.
{{% /alert %}}

### Cascading Theming Modules

For solution development, Mendix recommends using a layered approach to your theme modules to make them as adaptable as possible.  

For more information, see the [Brand Your Adaptable Solution](https://academy.mendix.com/link/paths/130/Brand-your-Adaptive-Solution) learning path, which includes details on how to structure your SASS files.

An ISV that maintains multiple adaptable solutions can structure their theme modules in the following way:

| Module | Implemented by | Shared by | Purpose |
| --- | --- | --- | --- |
| Atlas Core | Mendix | All ISV solution and customer implementations | Base for all Mendix Apps |
| ISV theme | ISV (shared UX team) | All ISV solution and customer implementations | Contains the default ISV theming |
| ISV solution specific | ISV (solution R&D team) | All customer implementations of this particular ISV solution | Additional styling for an individual solution |
| Customer theme | Customer implementation team | All customer implementations of any solution of the ISV | Overriding ISV-specific logic for a customer (for example, color palette and typography) |
| Customer app- specific | Customer implementation team | Customer implementation of a specific solution of the ISV | Overriding the styling for a specific customer instance |

{{% alert color="info" %}}
For an ISV with a single solution, this can be reduced to three modules: 

* Atlas Core
* ISV theme
* Customer theme
{{% /alert %}}

In general, Mendix recommends being explicit regarding the used design system, and creating the relevant building blocks. This allows for a consistent look and feel across the adaptation and core UI.

## Using Constants

The default value of a usable constant cannot be overridden at implementation, but the local Studio Pro value can be changed using the [Mendix Runtime settings](/developerportal/deploy/environments-details/#constants). Constants, included hidden constants, can always be configured as part of the environment settings.

For more information, see [Constant Default Value](/refguide/constants/#default-value) in the *Studio Pro Guide*.

## Translating the Implementation and Implementing Jargon

To make the application translatable during implementation, all translatable documents need to be stored in open application modules. Using the [batch translate](/refguide/batch-translate/) and [batch replace](/refguide/batch-replace/) features, the text can be translated or updated during implementation. This can be done, for example, to implement jargon by changing a default concept like “Asset” into a customer-specific word like “Car” .

{{% alert color="info" %}}
Only content in open application modules can be translated. Content in solution modules is currently locked down due to IP protection, which is deprecated and will be removed in a future major version.
{{% /alert %}}

Variables cannot easily be translated, nor can text be changed in a protected microflow, since this is locked down. As a workaround, consider using an editable Enumeration as an internationalization map combined with the [getCaption](/refguide/enumerations-in-expressions/#getCaption) function. 

## Java Source Code Protection

{{% alert color="warning" %}}
The **Hidden** export level for Java actions is deprecated as part of the deprecation of [add-on and solution module types](/refguide/module-settings/#module-type).
{{% /alert %}}

Currently, setting the export level to **Hidden** on a Java action prevents the action from being unpacked in the app directory at the same path as regular modules. The Java code is put in a package, but no obfuscation or other security measures take place — reverse engineering the package would still reveal the source code.

Mendix does not offer a facility to protect intellectual property in Java files. For source code obfuscation, Mendix recommends using third-party software.

## Read More

* [Using the Right Components](https://academy.mendix.com/link/modules/510/lectures/4050/2.1-Using-the-Right-Components)
