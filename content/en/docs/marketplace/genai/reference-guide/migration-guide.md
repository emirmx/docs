---
title: "Release and Migration Guide for GenAI Modules"
url: /appstore/modules/genai/genai-for-mx/migration-guide/
linktitle: "Release and Migration Guide"
description: "Describes the combined releases of various GenAI-related modules and their inter-module dependencies. It also includes migration steps and notices about deprecations and removals"
weight: 1
---
## Introduction

During most regular release cycles, upgrading GenAI modules is seamless and does not require manual intervention. However, in some cases, breaking changes to the database or code are unavoidable in order to enable future improvements. 

This document is intended for consumers of GenAI modules. For releases that introduce impactful changes, it outlines the affected module versions, describes the nature of the changes, and specifies any actions that must be taken when upgrading to the newer versions.

{{% alert color="warning" %}}
Do not skip major versions as they may contain deprecations or require migration.

Modules remove deprecated entities, associations, and attributes in the subsequent major release, after they have been marked as deprecated. Deprecated domain model elements are indicated by the `_DEPR` suffix in the previous major version. 

This means that major versions containing deprecations should not be skipped during upgrades.

For example, if you are currently using V3.x.x and want to upgrade to V5.0.0, you must first upgrade to V4.0.0, deploy the application, and perform all required migration steps before proceeding to the next version. Skipping a major version may result in data loss, broken logic, or failed deployments.

{{% /alert %}}

## General recommendations

Mendix recommends to take the following steps per release to ensure a smooth upgrade without data loss. For the details of each release, refer to the [Releases](#releases) section below.

* Read the full migration guide for the specific release and make sure you cover each module that is used in your app.
* Perform the upgrade in a non-production environment first.
* Keep the back up of your database before starting.
* Upgrade all modules to the versions listed in the upgrade matrix for the release.
* Update any custom application logic by referencing deprecated entities, associations, attributes, microflows, or enumerations.
* Run all required migration microflows upon starting the application (for example, as part of the after-startup microflow).
* Verify migration results in the running app.
* Test your application thoroughly.
* Perform the upgrade and migrations in the production environment.

## Releases {#releases}

The sections below describe each release increment for a set of modules that are released at the same time. If your upgrade path does not include any of the module releases listed below, no additional actions are required during the upgrade.

### Release March 2026 {#march-2026}

This section explains breaking changes and required actions for a set of GenAI modules released in early March 2026. These changes prepare the domain models for future enhancements, particularly to support Agent definitions using MCP tools and Knowledge Bases.

{{% alert color="warning" %}}

This release introduces breaking changes across several modules. Skipping these major versions is not supported, as you must perform required migration steps to prevent data loss or application failures in subsequent releases.

{{% /alert %}}

#### Affected Modules and their Versions

The following module versions are released as compatible with each other and should be upgraded together.

| Module | Previous Version | New Version | Contains deprecations | Requires migration |
| --------------------- | ----------------- | ------------- | ----------------- | ---- |
| GenAI Commons | 5.x.x | 6.0.0 | No | Yes, as part of dependent modules. |
| Agent Commons | 2.x.x | 3.0.0 | Yes | Yes |
| MCP Client | 2.x.x | 3.0.0 | Yes | No, but update required for other migrations. |
| OpenAI Connector | 7.x.x | 8.0.0 | Yes | Yes |
| Amazon Bedrock Connector | 9.x.x | 10.0.0 | No | Yes | 
| PgVector Knowledge Base | 5.x.x | 6.0.0 | Yes | Yes |
| Mendix Cloud GenAI Connector | 5.x.x | 6.0.0 | No | Yes |

{{% alert color="info" %}}
Even if a module does not include any deprecations, Mendix strongly recommends upgrading all modules together according to the table above. This ensures that migrations in dependent modules execute correctly.
{{% /alert %}}

#### Migration Guide

In this section, migration steps are grouped together by topic rather than by module, as some changes affect multiple modules.

##### Single MCP Tools used by Agent Definitions

The following modules require upgrade:

* Agent Commons: migrate from V2.x.x to V3.0.0 
* MCP Client: migrate from V2.x.x to V3.0.0 

###### What Changed {#changes}

* The association from entity `SingleMCPTool` towards the entity `MCPTool` has been deprecated.
* Entity `SingleMCPTool` has a new association `SingleMCPTool_ConsumedMCPService` and a new attribute `Tool`.
* Entity `MCPServerConfiguration` was renamed to `ConsumedMCPService` along with the corresponding page `ConsumedMCPService_Overview` and Java action `ConsumedMCPService_CreateMCPClient`.

###### Impact

Select the page and Java action mention in the [What Changed](#changes) section above if they are used in your application. Agent definitions containing Single MCP tools require migration to prevent failing agent calls at runtime. 

Data migration is only required if your app uses Agent definitions containing Single MCP tools.

###### Required Actions

To prevent the need to recreate existing data related to Agent definitions, perform the following steps:

1. Upgrade the [MCP Client]() module to V3.0.0 in your Mendix app.
2. Upgrade the [Agent Commons]() module to V3.0.0 in your Mendix app.
3. Run the data migration microflow upon starting your application (for example, include it in the after-startup microflow).

   **AgentCommons** > **USE_ME** > **Migration** > `SingleMCPTool_Migrate` microflow will set the new association and attribute on existing `SingleMCPTool` records.

4. Update any custom logic or pages in your app that refer the old entity or its attributes `MCPTool_DEPR` in the MCPClient module. Available tools are not cached anymore. In cases where the actual list of available tools is required, refer to the `ConsumedMCPService_ListTools` microflow.
5. Verify your application compiles and runs correctly before deploying to cloud environments.

{{% alert color="info" %}}
The `SingleMCPTool` entity and related attributes and association will be permanently removed in the next major version (V4.0.0) of the MCP Client module.
 
Ensure to run the migration microflow before upgrading to the next major version.
{{% /alert %}}

##### Consumed Knowledge Bases

The following modules require upgrade:

* GenAI Commons: migrate from V5.x.x TO V6.0.0 
* Amazon Bedrock Connector: migrate from V9.x.x to V10.0.0
* Mendix Cloud GenAI Connector: migrate from V5.x.x to V6.0.0
* OpenAI Connector: migrate from V7.x.x to V8.0.0
* PgVector Knowledge Base: migrate from V5.x.x to V6.0.0

###### What Changed

- A new entity `ConsumedKnowledgeBase` has been added to the domain model of GenAI Commons. Each connector that provides logic to interact with Deployed Knowledge Bases now provides a specialization for this new entity. 
- In n the **Amazon Bedrock Connector** module, entity `BedrockConsumedKnowledgeBase` was added as a specialization of `ConsumedKnowledgeBase`.
- In the **Mendix Cloud GenAI Connector** module, existing entity `MxCloudKnowledgeBaseResource` is now a specialization of `ConsumedKnowledgeBase`.
- In the **OpenAI Connector** module, existing entity `AzureAISearchResource` is now a specialization of `ConsumedKnowledgeBase`. The `DisplayName` attribute has been deprecated and replaced by the attribute on the generalization.
- In the **PgVector Knowledge Base** module, existing entity `DatabaseConfiguration` is now a specialization of `ConsumedKnowledgeBase`. The `DisplayName` attribute has been deprecated and replaced by the attribute on the generalization.

###### Impact
Agent definitions using KnowledgeBases require migration to prevent failing agent calls at runtime. 
Existing knowledge base configurations in any of the mentioned connector modules, require migration to prevent failing knowledge base calls at runtime.

Migration is only required if your app interacts with knowledge bases from any of the mentioned modules, or contains existing data for such knowledge base configurations.

###### Required Actions

To prevent having to recreate existing data concerning Agent definitions and knowledge base configurations, perform the following steps:

1. Upgrade the **GenAI Commons** module to v6.0.0 in your Mendix project.
1. If present, upgrade the **Agent Commons** module to v3.0.0 in your Mendix project.

1. If your app has the **Amazon Bedrock Connector** module:

   1. upgrade the **Amazon Bedrock Connector** module to v10.0.0 in your Mendix project.
   1. include logic to **run the data migration microflow** upon starting your application (e.g. include it in the after-startup):
      ```
      AmazonBedrockConnector > USE_ME > Migration > ConsumedKnowledgeBase_Migrate
      ```
      This microflow will make sure the new attributes on the generalization are set properly and the `DisplayName` field is migrated.
   1. if the **Agent Commons** is part of your app as well and there are Agents defined using knowledge bases, include the following initially excluded submicroflow into the project and add it as microflow call according to the annotation in the above-mentioned microflow:
      ```
      AmazonBedrockConnector > USE_ME > Migration > AmazonBedrock_KnowledgeBase_Migrate
      ```
      This microflow will set the `CollectionIdentifier` field on the `KnowledgeBase` entity, as well as the outgoing reference to the `ConsumedKnowledgeBase`.


1. If your app has the **Mendix Cloud GenAI Connector** module:

   1. upgrade the **Mendix Cloud GenAI Connector** module to v6.0.0 in your Mendix project.
   1. include logic to **run the data migration microflow** upon starting your application (e.g. include it in the after-startup):
      ```
      MxGenAIConnector > USE_ME > Migration > ConsumedKnowledgeBase_Migrate
      ``` 
      This microflow will make sure the new attributes on the generalization are set properly and the `DisplayName` field is migrated.
   1. if the **Agent Commons** is part of your app as well and there are Agents defined using knowledge bases, include the following initially excluded submicroflow into the project and add it as microflow call according to the annotation in the above-mentioned microflow:
      ```
      MxGenAIConnector > USE_ME > Migration > MxGenAI_KnowledgeBase_Migrate
      ```
      This microflow will set the `CollectionIdentifier` field on the `KnowledgeBase` entity, as well as the outgoing reference to the `ConsumedKnowledgeBase`.

1. If your app has the **OpenAI Connector** module:

   1. upgrade the **OpenAI Connector** module to v8.0.0 in your Mendix project.
   1. include logic to **run the data migration microflow** upon starting your application (e.g. include it in the after-startup):
      ```
      OpenAIConnector > USE_ME > Migration > ConsumedKnowledgeBase_Migrate
      ```
      This microflow will make sure the new attributes on the generalization are set properly and the `DisplayName` field is migrated.
   1. if the **Agent Commons** is part of your app as well and there are Agents defined using knowledge bases, include the following initially excluded submicroflow into the project and add it as microflow call according to the annotation in the above-mentioned microflow:
      ```
      OpenAIConnector > USE_ME > Migration > Azure_KnowledgeBase_Migrate
      ```
      This microflow will set the `CollectionIdentifier` field on the `KnowledgeBase` entity, as well as the outgoing reference to the `ConsumedKnowledgeBase`.

1. If your app has the **PgVector Knowledge Base** module:

   1. upgrade the **PgVector Knowledge Base** module to v6.0.0 in your Mendix project.
   1. include logic to **run the data migration microflow** upon starting your application (e.g. include it in the after-startup):
      ```
      PgVectorKnowledgeBase > USE_ME > Migration > ConsumedKnowledgeBase_Migrate
      ```
      This microflow will make sure the new attributes on the generalization are set properly and the `DisplayName` field is migrated.
   1. if the **Agent Commons** is part of your app as well and there are Agents defined using knowledge bases, include the following initially excluded submicroflow into the project and add it as microflow call according to the annotation in the above-mentioned microflow:
      ```
      PgVectorKnowledgeBase > USE_ME > Migration > PgVector_KnowledgeBase_Migrate
      ```
      This microflow will set the `CollectionIdentifier` field on the `KnowledgeBase` entity, as well as the outgoing reference to the `ConsumedKnowledgeBase`.

1. **Update any custom logic or pages** in your application that reference:
   1. The attributes `DisplayName_DEPR` on the `DatabaseConfiguration` and `AzureAISearchResource` entities. Instead, now use the `DisplayName` field that comes as part of the generalization. 
   1. The association `KnowledgeBase_DeployedModel_DEPR`. Instead, now use the `CollectionIdentifier` attribute on the `KnowledgeBase` entity, if needed in combination with the `KnowledgeBase_ConsumedKnowledgeBase` association. 
1. **Verify** your application compiles and runs correctly before deploying to cloud environments. 
1. **Remove** the migration logic from the app logic the moment it has run **at least once in every impacted environment**. It can, however, be triggered multiple times without harm.

{{% alert color="info" %}}
The `KnowledgeBase_DeployedModel_DEPR` association will be  **permanently removed** in the next major version of the **Agent Commons** module, which will be **v4.0.0**.

The `DisplayName_DEPR` attribute will be  **permanently removed** in the next major version of the **OpenAI Connector** module, which will be **v9.0.0**.

The `DisplayName_DEPR` attribute will be  **permanently removed** in the next major version of the **PgVector Knowledge Base** module, which will be **v7.0.0**.
 
Ensure the migration microflow has been run before upgrading to the next major version.
{{% /alert %}}
