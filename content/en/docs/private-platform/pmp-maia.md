---
title: "Maia in Private Mendix Platform"
url: /private-mendix-platform/maia/
description: "Provides information on Maia options in Private Mendix Platform."
weight: 51
---

## Introduction

Mendix AI Assistance (Maia) refers to Mendix Platform capabilities that leverage [artificial intelligence (AI)](https://www.mendix.com/glossary/artificial-intelligence-ai/) and [machine learning (ML)](https://www.mendix.com/glossary/machine-learning/) to assist developers in application development. Maia is designed to help development teams in modeling and delivering Mendix applications faster, more consistently, and with higher quality. 

## Maia Capabilities in Private Mendix Platform

Mendix AI Assistance (Maia) in Private Mendix Platform has the following capabilities: 

Starting point for app creation:

* **Start with Maia** - a starting point in Studio Pro that helps you to start the app development process. Based on a required text description and an optional image or PDF, it generates an app that includes a domain model, data management overview pages, test data, and a tailored homepage. For more information, see [Start with Maia](/refguide/start-with-maia/).

Recommenders:

* **Best Practice Recommender** – helps you inspect your app against Mendix development best practice detecting and pinpointing development anti-patterns and, in some cases, automatically fixing them. For more information, see [Best Practice Recommender](/refguide/best-practice-recommender/).
* **UI Recommender** – helps you easily add new widgets to a page in Mendix Studio Pro without losing the context of what you are currently working on. For more information, see [UI Recommender](/refguide/ui-recommender/).
* **Workflow Recommender** – helps you model and configure workflows in Mendix Studio Pro. It gives you contextualized recommendations on the next best activity in your workflow based on context-related information. For more information, see [Workflow Recommender](/refguide/workflow-recommender/).

Generators:

* **Maia for Domain Model** – helps you generate new [domain models](/refguide/domain-model/), and explain and provide suggestions for existing domain models. For more information, see [Maia for Domain Model](/refguide/maia-for-domain-model/). 

    For Private Mendix Platform, this feature is supported for version 11.6 and newer.
    
* **Maia for Pages** – helps you generate a [page](/refguide/page/). It helps you add and configure widgets based on a text input and an optional image. After a page is generated, you can continue in the same session to ask Maia for further improvements and explanations. For more information, see [Maia for Pages](/refguide/maia-for-pages/).

    For Private Mendix Platform, this feature is supported for version 11.6 and newer.

* **Maia for Workflows** – helps you generate a [Workflow](/refguide/workflows/). By providing a use case via text input or an image, Maia can help you start creating your workflows. For more information, see [Maia for Workflows](/refguide/maia-for-workflows/).
* **Validation Assist** – helps you build validation microflows in a more automated way using pre-built expressions. For more information, see [Validation Assist](/refguide/validation-assist/).

## LLM Providers for Maia

Maia has been standardized on Claude Sonnet 3.7, but Private Mendix Platform supports connectivity to a broad set of models.

Starting in Private Mendix Platform 2.6, you can connect to the following Large Language Models:

* [AWS Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/models-regions.html)
  * Claude Sonnet 3.7
  * Claude Sonnet 4.0
  * Claude Sonnet 4.5
  * GPT-OSS-120B
* [Azure](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/concepts/models-sold-directly-by-azure)
  * 03-mini

### Example Configuration for Supported AWS Bedrock Claude Models

```text
{
  "api_key": "sk-your-global-api-key",
  "bedrock_region": "eu-central-1",
  "models": {
    "small_text": "bedrock/eu.anthropic.claude-haiku-4-5-20251001-v1:0",
    "small_files": "bedrock/eu.anthropic.claude-haiku-4-5-20251001-v1:0",
    "large_text": "bedrock/eu.anthropic.claude-sonnet-4-5-20250929-v1:0",
    "large_files": "bedrock/eu.anthropic.claude-sonnet-4-5-20250929-v1:0",
    "fallback": "bedrock/eu.anthropic.claude-sonnet-4-20250514-v1:0"
}
```

### Example Configuration for Supported Azure Models

```text
{
  "api_key": "...",
  "azure_api_version": "2025-01-01-preview",
  "azure_api_base": "https://<your-id>.cognitiveservices.azure.com/",
  "models": {
    "small_text": "azure/o3-mini",
    "small_files": "azure/o3-mini",
    "large_text": "azure/o3-mini",
    "large_files": "azure/o3-mini",
    "fallback": "azure/o3-mini"
  }
  ```