---
title: "Ground your Large Language Model in Data - Mendix Cloud GenAI"
url: /appstore/modules/genai/how-to/howto-groundllm/
linktitle: "Ground your LLM in data - Mendix Cloud GenAI"
weight: 40
description: "This document guides you to ground your large language model in data in your Mendix application to enhance functionality."
---

## Introduction

This document explains how to add data into your smart app to be included when interacting with a Large Language Model (LLM). To do this, you can use your existing app or follow the [Build a Smart App from a Blank GenAI App](/appstore/modules/genai/how-to/blank-app/) guide to start from scratch.

Through this document, you will:

* Understand how to ground your LLM in data within your Mendix application using the [Mendix Cloud GenAI Resource Packs](appstore/modules/genai/mx-cloud-genai/resource-packs/).
* Learn to integrate GenAI capabilities to address specific business requirements effectively using a knowledge base.

### Prerequisites {#prerequisites}

Before implementing this capability into your app, make sure you meet the following requirements:

* Start from scratch: To simplify your first use case, start building from a preconfigured setup [Blank GenAI Starter App](https://marketplace.mendix.com/link/component/227934). For more information, see [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/how-to/blank-app/). 

* Install the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle (version **2.2.0 or higher**) from the Mendix marketplace. If you start with the Blank GenAI App, you can skip this installation.

* Have a Knowledge Base resource within the [Mendix Cloud GenAI Resource Packs](/appstore/modules/genai/mx-cloud-genai/resource-packs/). 

* Have data to be added into your LLM. For this example, we will be using a modified and streamlined version of the demo data found in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), in the `ExampleMicroflows` module > `Ground in data - Mendix Cloud` > `Example data set`. If you need to create the demo data yourself, you should be familar with import mappings and JSON structures.

* Intermediate understanding of GenAI concepts: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarize yourself with the [concepts](/appstore/modules/genai/using-gen-ai/).

* Basic understanding of [Prompt Engineering](/appstore/modules/genai/get-started/#prompt-engineering).

## Ground you LLM in Data Use Case {#use-case}

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/diagram.png" >}}

### Choosing the Infrastructure {#infrastructure}

Since this tutorial is focused on the [Mendix Cloud GenAI Resource Packs](/appstore/modules/genai/mx-cloud-genai/resource-packs/), make sure to have the [Mendix Cloud GenAI Connector](/appstore/modules/genai/mx-cloud-genai/MxGenAI-connector/) that is part of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle on the marketplace.

To use the functionalities of this tutorials, follow the [Navigate through the Mendix Cloud GenAI Portal](/appstore/modules/genai/mx-cloud-genai/Navigate-MxGenAI/) instructions to collect the resources keys.

### Creation of Domain Model Entity {#domainmodel}

Considering that your application should be able to store the information, you will need to create attributes for the knowledge you want to save there. For this example, based on the below-mentioned [demo data](/appstore/modules/genai/how-to/howto-groundllm/#demodata), we create one `Description` attribute with type `String`. 

### Demo Data {#demodata}

You can use your own data to be uploaded into the knowledge base. However, for this example and as previously mentioned, we use a modified and streamlined version of the demo data found in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), in the `ExampleMicroflows` module > `Ground in data - Mendix Cloud` > `Example data set`. This demo data contains the attribute `Description` that provides information on how to solve basic IT support issues. For this information, the following is provided: 

* A `JSON File` with examples about IT support solutions, such as *'If the software crashes every time you try to save your document, first ensure you have the latest updates installed. Try...'*.

* An `Import Mapping` where the JsonObject is mapped into the domain model entity. 

### Load data into Knowledge Base {#kb}

To start, you need to create microflows that allow you to upload data into your knowledge base.

#### Loading Microflow {#loadkingkb}

1. Create a new microflow called, for example, `ACT_TicketList_LoadAllIntoKnowledgeBase`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/loaddataintokb_example.png" >}}

2. Add the `Retrieve Objects` action. Here you can configure it as follow:
    
    * Source: `From database`
    * Entity: Select the entity that contains your knowledge, which in this example would be the `MyFirstModule.Ticket`
    * Range: `All`
    * Object name: `TicketList`

3. Next, add the `Chunks: Initialize ChunkCollection` action. You can keep the *Use return variable* as Yes and object name `ChunkCollection`.

4. Next, as seen in the image above, we include a `Loop` where the Iterator has as *Loop type* `For each (item in the list)`, the *Iterate over* is the List retrieved in step 2, which in this case is named `TicketList`, and lastly, you can add a *Loop object name*, such as `IteratorTicket`. After saving those settings, add a `Chunks: Add KnowledgeBaseChunk to ChunkCollection` action inside the loop. Here you can configure it as follow:

    * Chunk collection: `$ChunkCollection` from step 3
    * Input Text: Edit the expression and use the iterator object from the loop with the desired attribute, which in this case is `$IteratorTicket/Description`
    * Human readable ID: `empty` (as this is an optional value)
    * Mx object: Select the loop's iterator, such as `$IteratorTicket`
    * Use return value: No
    * Metadata collection: `empty` (as this is an optional value)

5. Right after the loop, add an `Retrieve` action to retrieve a `MxCloudKnowledgeBase`. In this example we use the first that is found in the database:
    
    * Source: `From database`
    * Entity: `MxGenAIConnector.MxCloudKnowledgeBase`
    * Range: `First`
    * Object name: `MxCloudKnowledgeBase`

6. Next, add the `Connection: Get` action from the `Mendix Cloud Knowledge Base` category:

    * MxCloudKnowledgeBase | Type: `Variable` | Variable: `MxCloudKnowledgeBase` (as retrieved in step 5)
    * CollectionName | Type: `Expression` | Expression: `'TicketSolutions'`

You can keep the *Use return variable* as Yes and object name `MxKnowledgeBaseConnection`.

7. Next, add the `Embed & Repopulate Collection` action to insert your knowledge into the knowledge base:

    * Connection | Type: `Variable` | Variable: `MxKnowledgeBaseConnection` (as retrieved in step 6)
    * ChunkCollection | Type: `Variable` | Variable: `GenAICommons.ChunkCollection` (as created in step 3)

You can keep the *Use return variable* as Yes and variable name `IsSuccess`.

8. Next (optional), include a decision:
    * Caption: for example, `Replace Success`
    * Decision Type: `Expression`
    * Expression: `$IsSuccess`

    1. If the decision is `true`, an `End event` action can be added where a microflow return value can be set to `true`. You may add an end-user facing message to explain that the insertion was successful.

    2. If the decision is `false`, an `End event` action can be added where a microflow return value can be set to `false`. You may add an end-user facing message to explain that the insertion has failed.

You successfully implemented the knowledge base insertion microflow! In the case that you do not have any data available in your app yet, you need to create a microflow for the creation of the dataset as described in the next section.

#### Data set Microflow {#dataset}
This microflow first checks if there is already a list of tickets in the database. If that is not the case, it imports a JSON string as described in [demo data section](/appstore/modules/genai/how-to/howto-groundllm/#demodata).

9. Create a new microflow, for example `Tickets_CreateDataset`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/loaddataintokb_example2.png" >}}

10. Add a `Retrieve` action:
    
    * Source: `From database`
    * Entity: Select the entity that contains your knowledge, which in this example would be the `MyFirstModule.Ticket`
    * Range: `First`
    * Object name: `Ticket`

11. Next, include a decision where:
    * Caption: `Tickets?`
    * Decision Type: `Expression`
    * Expression: `$Ticket = empty`

    1. If the decision is `false`, an End event is added because we do not need to import any tickets.

    2. If the decision is `true`,  continue to the next step.

12. In the `true` path, add the `Create Variable` action, where the `String `value includes the JSON text mentioned in the [demo data](/appstore/modules/genai/how-to/howto-groundllm/#demodata). Use `TicketJSON` as the variable's name.

13. Next, add the `Import With Mapping` action with the following configurations:

    * Variable: `TicketJSON` created in the previous step
    * Mapping: Use the mapping mentioned in the [demo data section](/appstore/modules/genai/how-to/howto-groundllm/#demodata)
    * Range: `All`
    * Commit: `Yes without events`
    * Store in variable: `No` (optional, not needed here)
    * Variable name: (optional) only when stored in variable

Now that both microflows are created, you need to combine and add them to the homepage for the knowledge base to be populated.

#### Joining the Microflows {#joining-microflows}

14. Create a new microflow `ACT_TicketList_CreateData_InsertIntoKnowledgeBase`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/loaddataintokb_example3.png" >}}

15. Add a `Call Microflow` action where you call the `MyFirstModule.Tickets_CreateDataset` microflow created above. 

16. Next, add a `Call Microflow` action where you call the `MyFirstModule.ACT_TicketList_LoadAllIntoKnowledgeBase` microflow created above. For the *Use return variable*, you select `No`.

You sucessfully added the logic to insert data into the knowledge base!

### Chat Setup {#chatbotmicroflows}

To use the knowledge in a chat interface, create and adjust certain microflows as shown below. 

1. Search for the pre-built microflow `ChatContext_ChatWithHistory_ActionMicroflow` in the **ConversationalUI** > **USE_ME** > **Conversational UI** > **Action microflow examples** folder and copy it into your `MyFirstBot` module.

2. Search for the pre-built microflow `ACT_FullScreenChat_Open` in **ConversationalUI > USE_ME > ConversationalUI > Pages**. Copy the microflow into your `MyFirstBot` module and afterwards select **Include in project** on the copied microflow.

3. Change the parameters of the `New Chat` action in the `ACT_FullScreenChat_Open` microflow:

    * The `Action microflow` input parameter to your new `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow` from your `MyFirstBot` module.

    * The `System prompt` input parameter to a prompt that fits your use case. For example, `'You are a helpful assistant supporting the IT department with employees requests. Use the knowledge base and previous support tickets as a database to find a solution to the users request without disclosing sensitive details or data from previous tickets.'`.

    * The `Provider name` input parameter can be changed to a text fitting the purpose better. For example, `My GenAI provider configuration`.

Now that we have the `MyFirstBot.ACT_FullScreenChat_Open` microflow configured, we can adjust the `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow` which is called when a user submits a message in the chat interface.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/chatcontext_microflow_example.png" >}}

1. Open your `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow` microflow in your `MyFirstBot` module.

2. After the `Request found` decision, add a `Retrieve` action. In this example we use the first that is found in the database as we did in the insertion microflow:
    
    * Source: `From database`
    * Entity: `MxGenAIConnector.MxCloudKnowledgeBase`
    * Range: `First`
    * Object name: `MxCloudKnowledgeBase`

3. Add the `Tools: Add Mendix Cloud Knowledge Base` action with settings as showed in the picture below:

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/tool_mendixcloudgenai_example_action.png" >}}

 The rest of the action can remain the same as they are currently set. Now that we implement everything we need, we can test the chat with enriched knowledge.

### Navigation Setup

For your application to run as expected, you need to make sure that you can call the following microflows from your navigation or home page: 

* Chatbot: Add the `MyFirstModule.ACT_FullScreenChat_Open` microflow created in the [Customizing Chatbot Microflows section](/appstore/modules/genai/how-to/howto-groundllm/#chatbotmicroflows).

* Create Demo Data and Populate KB: Add the `MyFirstModule.ACT_TicketList_CreateData_InsertIntoKnowledgeBase` created in the [Joining the Microflows section](/appstore/modules/genai/how-to/howto-groundllm/#joining-microflows).

* Mendix Cloud Configuration: If you started from a Blank GenAI App, the configuration page should already be included. In case you started from your application, add the `MxGenAIConnector.NAV_ConfigurationOverview_Open` microflow.

* Ensure that your admin role has the following module roles assigned: MxGenAIConnector.Administrator, ConversationalUI.User, MyFirstModule.Administrator


## Testing and Troubleshooting {#testing-troubleshooting}

Before testing, ensure that you have completed the Mendix Cloud GenAI configuration as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/how-to/blank-app/), particularly the [Mendix Cloud GenAI Configuration](/appstore/modules/genai/how-to/blank-app/#mendix-cloud-genai-configuration) section. 

To test the Chatbot, first ensure you click on the **Create Demo Data and Populate KB** icon/text to populate the knowledge base. Then, go to the **Chatbot** icon to open the chatbot interface. Start interacting with your chatbot by typing in the chat box something related to your knowledge base.
For this example, it would be `My computer crashes every time, what can I do?`.

Congratulations! You grounded your LLM in data and your chatbot is now ready to use.

{{% alert color="info" %}}
In case you get stuck in the microflows, you can find them in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), under the ExampleMicroflows module > Ground in data - Mendix Cloud.
{{% /alert %}}

If an error occurs, check the **Console** in Studio Pro for detailed information to assist in resolving the issue.
