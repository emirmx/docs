---
title: "Create a single agent"
url: /appstore/modules/genai/how-to/howto-single-agent/
linktitle: "Creating A Single Agent"
weight: 60
description: "This document guides you through creating an agent by integrating knowledge bases, function calling and prompt management in your Mendix application to build powerful GenAI usecases."
---

## Introduction
TBD

This document explains how to use function calling in your smart app. To do this, you can use your existing app or follow the [Build a Smart App from a Blank GenAI App](/appstore/modules/genai/how-to/blank-app/) guide to start from scratch, as demonstrated in the sections below.

Through this document, you will:

* Understand how to implement function calling within your Mendix application.
* Learn to integrate GenAI capabilities to address specific business requirements effectively.

### Prerequisites {#prerequisites}

TBD

Before integrating function calling into your app, make sure you meet the following requirements:

* An existing app: To simplify your first use case, start building from a preconfigured set up [Blank GenAI Starter App](https://marketplace.mendix.com/link/component/227934). For more information, see [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/how-to/blank-app/). 

* Be on Mendix Studio Pro 10.12.4 or higher.

* Installation: Install the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle from the Mendix marketplace. If you start with the Blank GenAI App, skip this installation.

* Intermediate knowledge of the Mendix platform: Familiarity with Mendix Studio Pro, microflows, and modules.

* Basic understanding of GenAI concepts: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarize yourself with the [concepts](/appstore/modules/genai/using-gen-ai/).

* Understanding Function Calling and Prompt Engineering: Learn about [Function Calling](/appstore/modules/genai/function-calling/) and [Prompt Engineering](/appstore/modules/genai/get-started/#prompt-engineering) to use them within the Mendix ecosystem.

## Single Agent Use Case {#use-case}

TBD

IMAGE PLACEHOLDER




## Testing and Troubleshooting {#testing-troubleshooting}

TBD

Before testing, ensure that you have completed the Mendix Cloud GenAI, OpenAI, or Bedrock configuration as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/how-to/blank-app/), particularly the [Infrastructure Configuration](/appstore/modules/genai/how-to/blank-app/#config) section. 

To test the Chatbot, go to the **Home** icon to open the chatbot interface. Start interacting with your chatbot by typing in the chat box.
For example, type—`Write a message to my colleague Max asking about a meeting to discuss the content for our next GenAI how-to.` or `How many bank holidays do I have in December?`

Congratulations! Your chatbot is now ready to use.

If an error occurs, check the **Console** in Studio Pro for detailed information to assist in resolving the issue.
