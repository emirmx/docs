---
title: "Ground your Large Language Model in Data - Mendix Cloud GenAI"
url: /appstore/modules/genai/how-to/howto-groundllm/
linktitle: "Ground your LLM in data - Mendix Cloud GenAI"
weight: 40
description: "This document guides you to ground your large language model in data in your Mendix application to enhance functionality."
---

## Introduction

This document explains how to add data into your smart app to be included in your Large Language Model (LLM). To do this, you can use your existing app or follow the [Build a Smart App from a Blank GenAI App](/appstore/modules/genai/how-to/blank-app/) guide to start from scratch.

Through this document, you will:

* Understand how to ground your LLM in data within your Mendix application using the [Mendix Cloud GenAI Resource Packs](appstore/modules/genai/mx-cloud-genai/resource-packs/).
* Learn to integrate GenAI capabilities to address specific business requirements effectively using a knowledge base.

### Prerequisites {#prerequisites}

Before implementing this capability into your app, make sure you meet the following requirements:

* An existing app: To simplify your first use case, start building from a preconfigured set up [Blank GenAI Starter App](https://marketplace.mendix.com/link/component/227934). For more information, see [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/how-to/blank-app/). 

* Installation: Install the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle from the Mendix marketplace. If you start with the Blank GenAI App, skip this installation. Please make sure to have the bundle version **2.2.0 or higher**.

* Have a Knowledge Base resource within the [Mendix Cloud GenAI Resource Packs](appstore/modules/genai/mx-cloud-genai/resource-packs/). 

* Have data to be added into your LLM. For this example, we will be using a modified and streamlined version of the demo data found in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), in the `ExampleMicroflows` module > `Ground in data - Mendix Cloud` > `Example data set`.

* Advanced knowledge of the Mendix platform: Familiarity with Mendix Studio Pro, microflows, and modules.

* Intermediate understanding of GenAI concepts: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarize yourself with the [concepts](/appstore/modules/genai/using-gen-ai/).

* Basic understanding of [Prompt Engineering](/appstore/modules/genai/get-started/#prompt-engineering).

## Ground you LLM in Data Use Case {#use-case}

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/diagram.png" >}}

### Choosing the Infrastructure {#infrastructure}

Since this tutorial is focused on the [Mendix Cloud GenAI Resource Packs](appstore/modules/genai/mx-cloud-genai/resource-packs/), make sure to have the [Mendix Cloud GenAI Connector](/appstore/modules/genai/mx-cloud-genai/MxGenAI-connector/) that is part of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle on the marketplace.

### Creation of Domain Model Entity {#domainmodel}

Considering that your application should be able to store the information, you will need to create attributes for the knowledge you want to save there. For this example, based on the bellow-mentioned [demo data](/appstore/modules/genai/how-to/howto-groundllm/#demodata), we create one `Description` attribute with type `String`. 

### Demo Data {#demodata}

You can use your own data to be uploaded into the knowledge base. However, for this example and as previously mentioned, we use a modified and streamlined version of the demo data found in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), in the `ExampleMicroflows` module > `Ground in data - Mendix Cloud` > `Example data set`. This demo data contains a single attributes named `Description` that provides information on how to solve basic IT support issues. For this information, the following is created: 

* A `JSON File` with examples about IT support solutions, such as *'If the software crashes every time you try to save your document, first ensure you have the latest updates installed. Try...'*.

* An `Import Mapping` where the JsonObject is mapped into the domain model entity. 

### Load data into Knowledge Base {#kb}

To start, you need to create microflows that allow you to upload data into your knowledge base.

#### Loading Microflow {#loadkingkb}

1. Create a new microflow called, for example, `ACT_TicketList_LoadAllIntoKnowledgeBase`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/loaddataintokb_example.png" >}}

2. Add the `Retrieve Objects` action. Here you can configure it as follow:
    
    * Source: `From database`
    * Entity: You link here your domain model, which in this example would be the `MyFirstModule.Ticket`
    * Range: `All`
    * Object name: Depending on your use case, you can add the best fitting name. For this example, we name it `TicketList`

3. Next, add the `Chunks: Initialize ChunkCollection` action where you select the `GenAICommons.ChunkCollection_Create` in the `Microflow` parameter. You can keep the *Use return variable* as Yes and named the object `ChunkCollection`, for example. 

4. Next, as seen in the image above, we include a `Loop` where the Iterator has as *Loop type* `For each (item in the list)`, the *Iterate over* is the List collected in step 2, which in this case is named `TicketList`, and lastly, you can add a *Loop object name*, such as `IteratorTicket`. After saving those seetings, add a `Chunks: Add KnowledgeBaseChunk to ChunkCollection` action inside the loop. Here you can configure it as follow:

    * Chunk collection: `$ChunkCollection` form step 3
    * Input Text: Write as an expression the entity from the loop, with your parameter, which in this case is `$IteratorTicket/Description`
    * Human readable ID: `empty`
    * Mx object: Select your loop's entity, such as `$IteratorTicket`
    * Use return value: No

5. Move outside the loop, and add the `Retrieve Objects` action. Here you can configure it as follow:
    
    * Source: `From database`
    * Entity: `MxGenAIConnector.MxCloudKnowledgeBase`
    * Range: `First`
    * Object name: `MxCloudKnowledgeBase`

6. Next, add the `Connection: Get` action where you select the `MxGenAIConnector.MxKnowledgeBaseConnection_Create` in the *Microflow* parameter. As *parameter values*, select:

    * MxCloudKnowledgeBase | Type: `Variable` | Variable: `MxGenAIConnector.MxCloudKnowledgeBase`
    * CollectionName | Type: `Expression` | Expression: `'Ticket Solutions'`

You can keep the *Use return variable* as Yes and named the object `MxKnowledgeBaseConnection`, for example.

7. Next, add the `Embed & Repopulate Collection` action where you select the `MxGenAIConnector.ChunkCollection_Embed_RepopulateCollection` in the *Microflow* parameter. As *parameter values*, select:

    * Connection | Type: `Variable` | Variable: `MxGenAIConnector.MxKnowledgeBaseConnection`
    * ChunkCollection | Type: `Variable` | Expression: `GenAICommons.ChunkCollection`

You can keep the *Use return variable* as Yes and named the object `IsSuccess`, for example.

8. Next, include a decision where:
    * Caption: for example, `Replace Success`
    * Decision Type: `Expression`
    * Expression: `$IsSuccess`

    1. If the decision is `false`, an `End event` action can be added where a microflow return value can be set to `false` with a return variable name as `Variable`.

    2. If the decision is `true`, an `End event` action can be added where a microflow return value can be set to `true` with a return variable name as `Variable`.

You finished this microflow! Now, you need to create a new one for the creation of the dataset. What this second microflow does is to check if there is information in the database. If there is, it contiues it flows, and if it does not, it adds the JSON file created above in the [demo data section](/appstore/modules/genai/how-to/howto-groundllm/#demodata).

#### Data set Microflow {#dataset}

9. Create a new microflow called, for example, `Tickets_CreateDataset`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/loaddataintokb_example2.png" >}}

10. Add the `Retrieve Objects` action. Here you can configure it as follow:
    
    * Source: `From database`
    * Entity: You link here your domain model, which in this example would be the `MyFirstModule.Ticket`
    * Range: `First`
    * Object name: Depending on your use case, you can add this name. For this example, it is called `Ticket`.

11. Next, include a decision where:
    * Caption: for example, `Tickets?`
    * Decision Type: `Expression`
    * Expression: `$Ticket = empty`

    1. If the decision is `false`, an End event is added.

    2. If the decision is `true`,  continue to the next step by adding a new action.

12. After the decision `true`, add the `Create Variable` action, where the value includes the JSON text mentioned in the [demo data](/appstore/modules/genai/how-to/howto-groundllm/#demodata). The variable name can be set, for example, `TicketJSON`.

13. Next, add the `Import With Mapping` action with the following configurations:

    * Variable: `TicketJSON` created in the previous step
    * Mapping: You map it to your mapping mentioned in the [demo data section](/appstore/modules/genai/how-to/howto-groundllm/#demodata)
    * Range: `All`
    * Commit: `Yes without events`
    * Store in variable: `Yes`
    * Variable name: For this example, we set it as `Variable`

Now that we finished this step, you need to join both logics and add it to the hompage for the knowledge base to be populated.

#### Joining the Microflows {#joiningmf}

14. Create a new microflow called, for example, `ACT_TicketList_CreateData_InsertIntoKnowledgeBase`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/loaddataintokb_example3.png" >}}

15. Add a `Call Microflow` action where you call the `MyFirstModule.Tickets_CreateDataset` microflow created above. 

16. Next, add a `Call Microflow` action where you call the `MyFirstModule.ACT_TicketList_LoadAllIntoKnowledgeBase` microflow created above. For the *Use return variable*, you select `No`.

You finished the first microflows.

### Chat Setup {#chatbotmicroflows}

To continue, create and adjust certain microflows as shown below. 

1. Locate the pre-built microflow `ChatContext_ChatWithHistory_ActionMicroflow` in the **ConversationalUI** > **USE_ME** > **Conversational UI** > **Action microflow examples** folder and copy it into your `MyFirstBot` module.

2. Locate the pre-built microflow `ACT_FullScreenChat_Open` in **ConversationalUI > USE_ME > Pages**. Right-click on the microflow and select **Include in project** to copy it into your `MyFirstBot` module.

3. Locate the `New Chat` action in the `ACT_FullScreenChat_Open` microflow. Inside this action, change the following:

    * The `Action microflow` input parameter to your new `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow` from your `MyFirstBot` module.

    * The `System prompt` input parameter to a prompt that fits your use case. For example, `'You are a helpful assistant supporting the IT department with employees requests. Use the knowledge base and previous support tickets as a database to find a solution to the users request without disclosing sensitive details or data from previous tickets.'`.

    * The `Provider name` input parameter can be changed to a text fitting the purpose better. For example, `My GenAI provider configuration`.

Now that we have the `MyFirstBot.ACT_FullScreenChat_Open` microflow configured, we can adjust the `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow`.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/chatcontext_microflow_example.png" >}}

1. Locate your `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow` microflow in your `MyFirstBot` module.

2. After the `Request found` decision, add a `Retrieve Objects` action. Here you can configure it as follow:
    
    * Source: `From database`
    * Entity: `MxGenAIConnector.MxCloudKnowledgeBase`
    * Range: `First`
    * Object name: `MxCloudKnowledgeBase`

3. Following this action, continue with the `Tools: Add Mendix Cloud Knowledge Base` action with settings as showed in the picture below.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-goundllm/tool_mendixcloudgenai_example_action.png" >}}

 The rest of the actions stay the same as they are currently set. Now that we have the basics, we focus on the demo data and how to ground the LLM with this information. 



### Navigation Setup

For your application to run as expected, you need to make sure that your navigation calls the right microflows. 

* Chatbot: Call on the `MyFirstModule.ACT_FullScreenChat_Open` microflow created in the [Customizing Chatbot Microflows section](/appstore/modules/genai/how-to/howto-groundllm/#chatbotmicroflows).

* Create Demo Data and Populate KB: Call on the `MyFirstModule.ACT_TicketList_CreateData_InsertIntoKnowledgeBase` created in the [Joining the Microflows section](/appstore/modules/genai/how-to/howto-groundllm/#joiningmf).

* Mendix Cloud Configuration: If you started from a Blank GenAI App, the Settings should be included already. In case you started from your application, call the `MxGenAIConnector.NAV_ConfigurationOverview_Open` microflow. 

## Testing and Troubleshooting {#testing-troubleshooting}

Before testing, ensure that you have completed the Mendix Cloud GenAI configuration as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/how-to/blank-app/), particularly the [Mendix Cloud GenAI Configuratio](/appstore/modules/genai/how-to/blank-app/#mendix-cloud-genai-configuration) section. 

To test the Chatbot, first ensure you click on the **Create Demo Data and Populate KB** icon/text to populate the knowledge base, and to set up your keys to be able to use the Mendix Cloud GenAI Resource Packs. Then, go to the **Chatbot** icon to open the chatbot interface. Start interacting with your chatbot by typing in the chat box something related to your knowledge base.
For this example, it would be `My computer crashes every time, what can I do?`.

Congratulations! You granted your LLM in data and your chatbot is now ready to use.

{{% alert color="info" %}}
In case you get stuck in the microflows, you can find them in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), under the ExampleMicroflows module > Ground in data - Mendix Cloud.
{{% /alert %}}

If an error occurs, check the **Console** in Studio Pro for detailed information to assist in resolving the issue.
