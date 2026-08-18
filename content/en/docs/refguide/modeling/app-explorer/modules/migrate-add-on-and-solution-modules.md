---
title: "Migrating from Add-on Modules and IP Protection"
linktitle: "Migrating from Add-on Modules"
url: /refguide/migrate-add-on-and-solution-modules/
weight: 35
draft: true
description: "Describes the deprecation of add-on modules and intellectual property protection, the capabilities that replace them, and the capabilities that have no replacement."
---

{{% alert color="warning" %}}
The add-on module type and intellectual property (IP) protection are deprecated as of Studio Pro 11.18, and are removed in Studio Pro 12.0. They remain fully supported for the entire support duration of Mendix 11. Solutions are not deprecated.
{{% /alert %}}

{{% todo %}}Draft. Blocking items before publication: (1) the successor capability names below are working titles and are not final; (2) usability of entities, attributes, and associations has no successor design — the Domain Model Elements section states this as an open gap, which needs a product decision, not a docs decision; (3) Docs team sign-off is required for the Planned Replacements section, which describes unreleased functionality; (4) Marketplace and Commercial Solution Partner Program stakeholders need to be informed before this page goes live; (5) 11.18, 11.21, and 12.0 are stated throughout this page on Tom's instruction — confirm all three are committed and publicly announceable before publishing, because a deprecation page that names a removal version is a commitment.{{% /todo %}}

## Introduction

Mendix is deprecating the mechanism that hides part of a module's implementation from the developers who consume it. This page describes what is deprecated, what is not, which capabilities replace it, and which capabilities have no replacement.

Read this page if you publish add-on modules, publish an adaptable solution, or govern a shared core that other teams build on.

### What Is Deprecated

| Capability | Where it is configured | Status |
| --- | --- | --- |
| The add-on module type, exported as an *.mxmodule* file | [Module Settings](/refguide/module-settings/#add-on-module) | Deprecated in 11.18, removed in 12.0 |
| IP protection, through the **Export level** property with the values **Hidden** and **Usable** | [Configuring Add-on and Solution Modules for Publishing](/refguide/configure-add-on-and-solution-modules/#export-level) | Deprecated in 11.18, removed in 12.0 |
| IP protection of solution modules | [Module Settings](/refguide/module-settings/#solution-module) | Deprecated in 11.18, removed in 12.0 |

### What Is Not Deprecated

* **Solutions.** Building, selling, distributing, implementing, and upgrading an [adaptable solution](/appstore/creating-content/sol-adapt/) is unaffected. The *.mxsolution* package, the [Solution](/refguide/solution-tab/) tab of **App Settings**, and the solution lifecycle all remain.
* **Modules and module reuse.** Every module continues to be exportable, importable, versionable, and updatable. Reuse through the [Mendix Marketplace](/appstore/) is unaffected.
* **The solution module type.** Whether the type itself remains is a separate, open decision. What is certain is that it stops providing IP protection.

### What Continues to Work in Studio Pro 11

Deprecated does not mean removed. Deprecation in 11.18 changes nothing about how the features behave, and they are supported for the entire support duration of Mendix 11:

* Existing add-on modules keep working. You can import them, consume them, and update them.
* Existing *.mxmodule* and *.mxsolution* packages remain valid, and existing **Export level** settings remain in effect.
* You can still create new add-on modules and new solution modules, and still set **Export level** to **Hidden** or **Usable**.
* The reference documentation for these features continues to describe current behavior in full. See [Configuring Add-on and Solution Modules for Publishing](/refguide/configure-add-on-and-solution-modules/) and [Consuming Add-on Modules and Solutions](/refguide/consume-add-on-modules-and-solutions/).

### Timeline {#timeline}

| Version | What happens |
| --- | --- |
| Studio Pro 11.18 | The add-on module type and IP protection are deprecated. No behavior changes. |
| Studio Pro 11.18 to the end of Mendix 11 support | Fully supported. You can keep building, publishing, consuming, and updating protected modules. |
| Studio Pro 11.21 | The minimum Studio Pro version whose components can be imported into a Mendix 12 app. |
| Studio Pro 12.0 | The add-on module type and the **Export level** property are removed. |

{{% alert color="info" %}}
As with every new major version of Studio Pro, there is a minimum version for the components that can be imported into it, and for Mendix 12 that version is Studio Pro 11.21. A component built in an earlier version cannot be imported into a Mendix 12 app.

If you publish a component and want one version of it to work in both Mendix 11 and Mendix 12, build it in Studio Pro 11.21 or above and convert your protected modules to app modules, following the steps in [Migrating an Existing Module](#migrating).
{{% /alert %}}

No action is required to keep working in Studio Pro 11. Act now only if you publish a component that has to support Mendix 12, or if you want to adopt the replacement capabilities early.

## Two Migration Tracks {#tracks}

IP protection is used for two unrelated purposes, and the two have different answers. Establish which one applies to you before reading further, because a statement that is true for one is false for the other.

| Track | Purpose | Typical user | Summary of the answer |
| --- | --- | --- | --- |
| [Governance](#governance) | Stop implementation teams from changing a shared core by accident, and keep upgrades clean | Enterprise platform team, central build team | Replacements exist, and some are an improvement |
| [Commercial IP](#commercial) | Stop a paying customer from copying an implementation or switching off a license check | Independent software vendor, Commercial Solution Partner | No technical replacement. Only non-technical fallbacks |

The distinction that decides which track your answer comes from is enforcement. The replacements are soft boundaries: they signal intent, and the consumer can override them. A soft boundary satisfies governance, because a colleague who overrides a documented boundary can be held to it. A soft boundary does not satisfy commercial protection, because a counterparty who overrides it is exactly the person the boundary existed to stop.

## Governance Track {#governance}

If you use IP protection so that implementation teams adapt only what they are supposed to adapt, the replacements cover your case, and one long-standing tradeoff disappears.

Today, [Adaptable Solutions](/appstore/creating-content/sol-adapt/#three-parts) describes an inherent tradeoff: what is part of the common core and IP-protected cannot be adapted, while what can be adapted can never be protected. That is why an adaptable solution has to be split into an immutable core and an adaptable core in the first place.

Soft, overridable boundaries remove that tradeoff. A document can be marked as not-to-be-changed while still remaining changeable in the exceptional case that requires it. Consumers stop hitting dead ends where the only way forward was to ask the publisher for a new version, and publishers keep the signal that says which parts are safe to touch.

### What This Costs

Be clear-eyed about the loss:

* **Discipline becomes conventional, not enforced.** A consumer who wants to change a core document can. Nothing prevents it.
* **Merge conflicts become possible.** An immutable core could not conflict on upgrade, by construction. A core that the consumer is able to edit can conflict. Upgrades move from drop-in to merge. See [Updating Marketplace Modules](/refguide/updating-marketplace-modules/).
* **Encapsulation weakens.** **Export level: Hidden** made a microflow uncallable from outside the module, which is an enforced API boundary. A soft equivalent is a recommendation.
* **A boundary override propagates.** If a consumer removes a boundary and re-exports the module onward, the change travels with the module. In an internal distribution chain, the original guidance can be gone before it reaches the developers it was written for.

## Commercial IP Track {#commercial}

If you sell a solution or a Marketplace component and rely on IP protection so that customers cannot copy your implementation or disable your metering, **there is no technical replacement**, and none is planned.

This is stated plainly because the alternative is worse: planning a migration around a capability that does not exist. The remaining options are all available today, and all of them are a reduction in what the platform does for you.

| Fallback | Protection preserved | What it costs |
| --- | --- | --- |
| Legal framework | None. It deters and gives you the right to act | Already recommended as a complement to technical restrictions. It becomes the primary control rather than a backstop |
| Distribution as a deployment package only | Complete. The customer never receives the model | The customer loses all design-time adaptability. For anyone who bought an adaptable solution, this is a reduction in what they bought |
| Proprietary logic in custom Java | Substantial, for logic that can live in Java | Not low-code. It raises the skill floor, and it does nothing for domain models, pages, or microflow logic |

### Legal Framework

Terms and conditions are already documented as the layer that covers what technical restrictions cannot, in [Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/). Review yours against the assumption that no part of your model is technically concealed from a customer who holds it. In particular, check that your agreement obliges the customer to leave usage metering and license validation intact, because the platform will not.

### Distribution as a Deployment Package Only

Shipping a Mendix Deployment Archive (MDA) rather than the model preserves protection completely. The customer deploys and configures, but never opens the model. This is already documented as an option in [Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/).

Weigh this against why the customer chose an adaptable solution. If design-time adaptation is part of what you sell, this fallback removes it.

### Proprietary Logic in Custom Java

Logic implemented in a [Java action](/refguide/java-actions/) and shipped as a compiled JAR is not visible in the model. Some publishers already use this for genuinely proprietary algorithms.

Its limits are severe: it covers algorithmic logic only, it cannot express a domain model, a page, or microflow orchestration, and it requires Java skills on your build team. Treat it as a way to protect a specific algorithm, not as a way to protect a solution.

## Capability Mapping {#mapping}

The following table maps each capability of the deprecated mechanism to what replaces it. **Available now** capabilities are released and documented. **Planned** capabilities are described in [Planned Replacements](#planned) and are not commitments.

| Capability today | Replacement | Track | Availability |
| --- | --- | --- | --- |
| Stable module identity across renames and versions | [Module ID](/refguide/module-settings/#module-id), [checksum](/refguide/module-settings/#checksum), and the [package manifest](/refguide/module-settings/#package-manifest) | Both | Available now |
| Clean, drop-in upgrade of an unmodified core | [Module update with customizations retained](/refguide/updating-marketplace-modules/) | Governance | Available now |
| Version numbers, previously meaningful only for protected modules | [Module version](/refguide/module-settings/#module-version) for every module type | Both | Available now |
| Detection of whether a consumer changed a module | [Checksum](/refguide/module-settings/#checksum), which detects modification after import | Governance | Available now |
| **Export level: Hidden**, in its role of making a document unchangeable | A soft, consumer-overridable read-only status per document | Governance | Planned |
| **Export level: Usable**, in its role of declaring the intended API of a module | A dedicated container in the module that lists the documents a consumer should start from | Governance | Planned |
| **Export level: Hidden**, in its role of preventing calls from outside the module | Under consideration. Not designed | Governance | No replacement designed |
| **Export level: Usable** on entities, attributes, and associations | None. See [Domain Model Elements](#domain-model-elements) | Governance | Open gap |
| Publisher-controlled extension points, currently built by pairing a protected module with an open one | Declared substitution points that a consumer fills with their own implementation | Governance | Planned |
| Cross-references between the inseparable modules of a solution core | Declared module-to-module dependencies with minimum versions | Governance | Planned |
| Concealment of an implementation from a paying customer | None. See [Commercial IP Track](#commercial) | Commercial | No replacement |
| Tamper-proof custom usage metering | None. See [Usage Metering and Entitlement Enforcement](#metering) | Commercial | No replacement |
| Enforceable entitlement and license validation | None. See [Usage Metering and Entitlement Enforcement](#metering) | Commercial | No replacement |
| The add-on module type and the *.mxmodule* artifact | A module with properties, exported as a standard module package | Both | Planned |

## Available Now {#available-now}

These capabilities are released and are the only part of the replacement set you can rely on today. Adopting them early is the most useful preparation you can do, and it is worth doing whether or not you currently use IP protection.

### Package Management {#package-management}

Studio Pro 11.12 and above give every module a [Module ID](/refguide/module-settings/#module-id) that is stable across versions and across renames, a SHA-256 [checksum](/refguide/module-settings/#checksum), and a `manifest.json` file in the module package. Studio Pro uses these to track a module reliably rather than matching on the module name.

This is the foundation the rest of the replacement set is built on, and it removes a real class of failure: a rename no longer breaks the link between a module in an app and its source package.

### Module Updates That Retain Customizations

In Studio Pro 11.12 and above, updating a module performs a three-way merge between the version you originally imported, the version in your app, and the new version, so your customizations survive the update. See [Updating Marketplace Modules](/refguide/updating-marketplace-modules/).

This is what makes soft boundaries viable. An immutable core upgraded cleanly only because nobody could change it. A merge produces the same outcome without requiring immutability.

Two limits are documented and matter to the governance track: a merge can produce [conflicts](/refguide/resolving-conflicts/) where you and the publisher changed the same element, and if document mapping fails you cannot keep your customizations and must replace the module in full.

### Module Versions for Every Module Type

[Module version](/refguide/module-settings/#module-version) is available for all module types in Studio Pro 11.12 and above. Previously it was meaningful only for add-on and solution modules. Version your modules to semantic versioning rules now, so that consumers can reason about your updates before anything else changes.

## Planned Replacements {#planned}

{{% alert color="warning" %}}
This section describes functionality that has not been released. It is included so that you can plan, and it is not a commitment. Capability names are working titles and will change. No version is given for any item, because none is committed. Do not make a purchasing or architectural decision that depends on a specific item in this section shipping.
{{% /alert %}}

### Read-Only Documents

Every document in a module gets a status that is either editable or read-only. A publisher sets it per document, or sets a default for the whole module and applies it to existing documents in bulk. A consumer sees a lock indicator and a warning when opening a read-only document, and can remove the status to edit it.

* **Replaces:** **Export level: Hidden** in its role of making a document unchangeable, and the immutable common core of the [three-parts model](/appstore/creating-content/sol-adapt/#three-parts).
* **Granularity:** the document. **Export level** applied to individual elements, including individual attributes. This is not a like-for-like substitution.
* **Does not:** protect IP, prevent a document from being called from outside the module, or prevent the consumer from removing the status.
* **Migration:** documents that are currently hidden map to read-only.

### A Container for Entry-Point Documents

A module gains a formal container that lists the documents a consumer is meant to start from, replacing the hand-maintained folder convention that publishers apply today. It is declarative: a signpost, not a restriction.

* **Replaces:** **Export level: Usable** in its role of declaring the intended API of a module.
* **Improves on today:** a [page can never be marked **Usable**](/refguide/configure-add-on-and-solution-modules/#supported-documents), so publishers currently expose a navigation microflow whose only purpose is to open the page. Any document type can be an entry point, including a page. This workaround disappears.
* **Does not:** restrict anything. **Export level: Usable** was enforced — calling a non-usable microflow from another module produced a consistency error. A declared entry point does not stop a consumer from using anything else in the module.
* **Migration:** documents that are currently **Usable** map to entry points.

### Declared Substitution Points

A publisher nominates documents that a consumer may, or must, replace with their own implementation matching a required signature. The consumer's implementation is used instead of the module's own.

* **Replaces:** the pattern of shipping two modules — a protected module plus an open module that the consumer edits and the protected one calls. See [Combining Module Types](/appstore/creating-content/sol-architecting/#combining-module-types). Both collapse into one module.
* **Does not:** protect anything. It is a controlled extension point, not a boundary.
* **Until then:** keep shipping the paired module. It remains documented and remains an accepted exception to the cyclic dependency rule.

### Declared Module Dependencies

A publisher declares which other modules a module depends on, with a minimum version, and Studio Pro reports missing or outdated dependencies to the consumer.

* **Replaces:** the ability of solution modules to reference each other's internals because they were all part of one inseparable solution core. Once the core is not inseparable, coupling has to be declared and versioned.
* **Does not:** grant access to another module's internals. Declaring a dependency and being permitted to reach inside are different things.

### Module Documentation

A publisher writes documentation for a module in the model itself, and a consumer reads it in Studio Pro at the point of use.

This replaces nothing that protected modules provided, but it matters here. Once a boundary is a recommendation rather than a wall, the reason for the boundary is what makes a consumer respect it, and no flag can carry a reason.

### Reference Restriction

Making a document unreferenceable from outside its module — excluded from autocomplete and from selection, with a consistency error on direct reference — is under consideration. It is the only candidate that would restore the encapsulation half of **Export level: Hidden**.

It is not designed, and it is not certain to be built. It would not provide IP protection in any case, because the implementation stays readable.

## Capabilities With No Replacement {#gaps}

### Domain Model Elements {#domain-model-elements}

**Export level: Usable** applies to entities, attributes, and associations, including external entities. Every planned replacement operates on documents or on whole modules. Entities, attributes, and associations are elements inside the domain model document, not documents. **No successor is designed for them.**

This is not a corner case. A published module can depend on it directly. Consider a module that guarantees exactly one configuration object per enumeration value by keeping the persistent entity non-usable and exposing rules instead: without element-level usability, a consumer can create and commit objects of that entity and break the module's core invariant. A configuration entity where two attributes are public and the rest are internal has the same problem, at attribute granularity.

If you rely on element-level usability, this is the part of your module that has no migration route yet. Contact Mendix Support with your use case so it can be accounted for in the design.

### Usage Metering and Entitlement Enforcement {#metering}

[Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/) currently recommends hiding custom usage metering inside a protected module so that customers cannot disable it, and recommends entitlement management as a layer on top of that protection. Both recommendations depend on the consumer being unable to edit the logic.

Without that, both become advisory:

* **Usage metering.** A consumer can find and change the metering implementation. Under-reporting is not detectable at the platform level.
* **Entitlement enforcement.** A cryptographically signed license key is only as strong as the logic that validates it. If the validating microflow is editable, edition gating, rate limiting, key expiry, and runtime URL binding are all advisory.

There is no platform capability planned for either. The available responses are the [legal framework](#commercial) and, where the commercial stakes justify it, [distribution as a deployment package only](#commercial) or [implementing the check in custom Java](#commercial).

## Migrating an Existing Module {#migrating}

Nothing has to change for a module to keep working in Studio Pro 11. Migrate when you need one version of a component to support both Mendix 11 and Mendix 12, or when you want to adopt the replacement capabilities early.

Migrating means changing the module type from add-on or solution to app. The module's implementation becomes readable to consumers, the **Export level** property no longer applies to its documents and elements, and the module is exported as an *.mpk* package instead of an *.mxmodule* file. This is the only route that survives into Studio Pro 12.0.

### Changing the Module Type Loses Module Data

{{% alert color="warning" %}}
Switching a module between the app type and the add-on or solution type causes the loss of that module's data once the new version of the app is deployed. Switching between the add-on and solution types is safe.

This is the central constraint on any migration plan. If the module's entities hold no data, conversion is a settings change. If they hold customer data, conversion requires a data migration, and there is no in-product tooling for it.
{{% /alert %}}

For details, see [Module Type](/refguide/module-settings/#module-type).

### Converting a Protected Module to an App Module

For a publisher, the conversion itself is short:

1. In **App Explorer**, double-click **Settings** for the module you want to convert.
2. On the **Export** tab, set **Module Type** to **App module**. See [Module Type](/refguide/module-settings/#module-type).
3. Review what is now visible. Everything that was **Hidden** is readable, including anything you concealed for commercial rather than governance reasons. See the [Commercial IP Track](#commercial) before you publish.
4. Replace the boundaries you relied on with the [available](#available-now) and [planned](#planned) successors, and record the ones that have [no successor](#gaps).
5. Set a new module version. See [Module Version](/refguide/module-settings/#module-version).
6. Export the module and publish it as an *.mpk* package. See [Importing and Exporting Apps, Modules, Widgets, and Documents](/refguide/import-and-export/).

The work is not in the conversion. It is in steps 3 and 4, and in what your consumers have to do next.

### The Precedent to Plan Around

Modules that have already moved from protected to unprotected did so without a direct migration path. The documented procedure was to remove the old module and add the new one as a distinct module, which deletes incoming associations to the protected module's entities and requires manually restoring production data.

Treat that as the worst case your plan has to beat, not as the expected path. In-product migration of **Export level** settings to the successor capabilities is intended, sequenced after the successor capabilities themselves exist.

### What You Can Do Now

For publishers:

1. Adopt [package management](#package-management). Confirm that every module you publish has a stable module ID and a semantic version.
2. Decide whether you need one component version that supports both Mendix 11 and Mendix 12. If you do, plan to build in Studio Pro 11.21 or above and to convert before Studio Pro 12.0. See the [timeline](#timeline).
3. Identify which of the [two tracks](#tracks) you are in, per module. Many publishers are in both, for different modules.
4. Inventory your **Export level** settings. Record which documents are **Usable**, which are **Hidden**, and separately which entities, attributes, and associations are **Usable** — the last group is the [open gap](#domain-model-elements).
5. Inventory where you depend on concealment for commercial reasons rather than governance reasons: metering, license validation, and any algorithm whose value depends on being unreadable.
6. Establish, per module, whether its entities hold customer data. That determines whether conversion is a settings change or a data migration.
7. Review your terms and conditions against the [Commercial IP Track](#commercial).

For consumers of add-on modules and solutions:

1. Keep using them. Nothing changes for you for the entire support duration of Mendix 11.
2. Expect the modules you consume to become readable over time, and expect to be able to change parts of them that you cannot change today. Being able to change something is not a reason to change it: an overridden boundary is a boundary your publisher may decline to support.
3. When a publisher ships an unprotected successor, ask whether it arrives as an update to the same module or as a separate module to add alongside the old one. The second case is the one that costs you data. See [The Precedent to Plan Around](#migrating).

### Published Marketplace Components

Existing published add-on modules stay available, and publishing a new version of one continues to work throughout Mendix 11. What changes is the reach of those versions: a component built before Studio Pro 11.21 cannot be imported into a Mendix 12 app, so a protected component published today has no route into Mendix 12 without conversion.

When you publish an unprotected successor, the [deprecating content](/appstore/deprecate-content/) mechanism lets you mark the old component as deprecated with a mandatory reason and a pointer to the replacement.

{{% alert color="info" %}}
Marketplace content deprecation and platform feature deprecation are unrelated mechanisms that share a word. This page is about the second. [Deprecating Content](/appstore/deprecate-content/) is about the first.
{{% /alert %}}

## Improvements Worth Noting {#improvements}

Not everything about the current mechanism is worth preserving. Removing it also removes the following:

* **Pages could never be made usable.** The documented workaround is a usable navigation microflow that exists only to open a page. Entry-point documents accept pages directly.
* **Debugging never worked.** You cannot step into a microflow in a protected module; the debugger silently uses **Step Over**. See [Consuming Add-on Modules and Solutions](/refguide/consume-add-on-modules-and-solutions/#limitations). Once the implementation is visible, it is debuggable.
* **Hidden constants could not be configured.** A consumer cannot set the value of a hidden constant in **App Settings**.
* **Known limitations disappear with the mechanism.** Conflicting custom widgets between an app module and an add-on module have no automatic fix, deploying an app with add-on modules for Eclipse has known build failures, and a non-English default language throws an error when no translation is present.

## Read More

* [Modules](/refguide/modules/)
* [Module Settings](/refguide/module-settings/)
* [Configuring Add-on and Solution Modules for Publishing](/refguide/configure-add-on-and-solution-modules/)
* [Consuming Add-on Modules and Solutions](/refguide/consume-add-on-modules-and-solutions/)
* [Updating Marketplace Modules](/refguide/updating-marketplace-modules/)
* [Applying Intellectual Property Protection](/appstore/creating-content/sol-ip-protection/)
* [Adaptable Solutions](/appstore/creating-content/sol-adapt/)
* [Architecting Adaptable Solutions](/appstore/creating-content/sol-architecting/)
