---
title: "Build your own GenAI connector"
url: /appstore/modules/genai/how-to/byo-connector
linktitle: "Build your own GenAI connector"
weight: 20
description: "A tutorial that describes how to build your own GenAI connector"
---

## Introduction

If you would like to create your own connection to the LLM model of your choice but would like to use the chat UI capabilities we have developed in our ConversationalUI module using the entities in GenAICommons, then you are at the right document to find out how you can get started with building your own GenAI Commons connector.

The practical advantages of building your own GenAI Commons connector are numerous. First, you can reuse all of our ConversationalUI components, allowing you to use the already existing chat interface and related functionalities. Second, starting from our starter apps is straightforward, allowing you to quickly set up and begin using the functionalities our different starter apps provide. These advantages not only save you valuable development time but also provide more time to customize the already existing functionalities. This guide will walk you through the process, ensuring you can seamlessly integrate your preferred LLM while leveraging our robust and user-friendly chat interface components. By following the steps outlined here, you’ll be able to create a custom connector that fits your specific needs, all while maintaining the high-quality user experience provided by our platform.

### Prerequisites

Before starting this guide, make sure you have completed the following prerequisites:

* Intermediate knowledge of the Mendix platform: Familiarity with Mendix Studio Pro, microflows, and modules is required.

* Basic understanding of GenAI concepts: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page to gain foundational knowledge and become familiar with the key [concepts](/appstore/modules/genai/get-started/).

* Understanding Large Language Models (LLMs) and Prompt Engineering: Learn about [LLMs](/appstore/modules/genai/get-started/#llm) and [prompt engineering](/appstore/modules/genai/get-started/#prompt-engineering) to effectively use these within the Mendix ecosystem.

### GenAI For Mendix

Before diving into building your own connector, first assess whether you actually need to start from scratch. If your provider follows a similar API structure to an existing connector, it is probably wise to take the existing connector's code as a basis for your module and modify what is needed. E.g., if your provider's API is REST-based with JSON payloads that look like OpenAI's JSON snippets, it is likely you can reuse a lot of the microflows and logic from the OpenAIConnector. Additionally, you might have a use case where you are running your own custom model on a private server or another cloud environment and want to connect to it, rather than relying on existing connectors. Even in such cases, the OpenAIConnector can still serve as a solid starting point, allowing you to adapt and extend it to fit your specific needs. Our team has a blog on "Leveraging the OpenAI Connector to run open-source large language models locally with Ollama", if this is a topic you are interested in please check out our blog.

However, if your provider has a different authentication mechanism, uses an SDK (like Bedrock's Java SDK), or requires a different request-response format, creating a new connector may be necessary. If that's the case, this guide will walk you through the process of integrating your provider while ensuring full compatibility with our ConversationalUI module.

## How to Determine the Right Approach When Building Your Own Connector?

When developing your own GenAI Connector, there are two possible approaches:

1. Starting from an existing connector (e.g., OpenAIConnector) 
2. Building from scratch (starting from the Echo Connector) 

Choosing the right approach depends on how similar your target provider's API is to OpenAI's. The best way to determine this is to inspect the API of the provider/model and compare it to OpenAI's API structure.

**Scenario 1**: Starting from the OpenAIConnector

If the provider's API is the same or very similar to OpenAI's, this might be a good indicator that you can duplicate the OpenAIConnector module and make the necessary adjustments. Some logical modifications include:
- Small changes in the request/response payload (e.g., extra or fewer fields, slightly different JSON structure). 
- Modifying the base URL to match the provider's endpoint structure. 
- Adding additional query parameters in the URL or payload. 
- Adapting the authentication mechanism, for example, switching from Bearer Token or API key headers to OAuth. 

This approach allows you to reuse a well-structured connector, minimizing development effort while ensuring compatibility with ConversationalUI.

**Scenario 2**: Starting from the Echo Connector

If the provider's API differs significantly from OpenAI's, it's best to start from the Echo Connector. This ensures that the input and output structures are properly aligned with GenAICommons, making integration with ConversationalUI straightforward.
A reason to take this approach might be if the provider requires a different protocol. This often means the communication structure and authentication methods will differ significantly, making it easier to build the connector from scratch rather than modifying an existing REST-based connector.

Additionally, refer to [GenAI Commons](/appstore/modules/genai/commons/) to explore what is available out of the box, which can help you accelerate development. Pay close attention to:
- The domain model (data structure) to see how existing entities can be reused. 
- The "connector building" folders, which contain useful microflows and helper activities for working with different providers. 

We have a public repository where you can access additional logic and use existing components. This structured approach ensures that you can build your GenAI Connector efficiently while taking full advantage of the existing Mendix functionalities. 

## Build your own connector

In this guide, we will focus on implementing chat completions, a fundamental capability supported by most LLMs.  To make the process more practical, we will develop an example connector—an Echo Connector. This simple connector will return the same text as output that was given in the input, but it will be fully compatible with the chat capabilities of GenAICommons and ConversationalUI.
To enable chat completion, the key microflow to understand is ChatCompletions_WithHistory that is in the GenAICommons module. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/ChatCompletions_WithHistory.png" >}}

This microflow plays a crucial role because it is the core process used across all our connectors and the ConversationalUI-based chat interfaces makes use of it. To properly integrate with it, the microflow needs to supply two essential input objects:
- DeployedModel - Represents the specific model being used and determines which connector (microflow) is being called
- Request - Contains the details of the user's input and conversation history as well as other configurations.

And one output object:
- Response - Contains the details of the LLM's results.

All these objects are based on standard entities from GenAICommons. To begin, the first step is to examine the [Request](/appstore/modules/genai/commons/#request) entity. This entity is responsible for structuring the input sent to the LLM and ensuring it follows the expected format for GenAICommons compatibility. It follows a predefined structure, ensuring that all necessary details are included when sending a request to the LLM and should already be in the correct format when this microflow is called. If our chat interfaces are used, whenever a user asks a question, a Request object is created automatically 

Since this structure is already standardized, no modifications are needed for the Request entity itself. Instead, when implementing a new connector, the focus should be on properly mapping the request data from the existing Request object to the format expected by the specific provider, which in our case will be the Echo Connector. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/GenAICommons_DomainModel.png" >}}

Just as the Request entity structures input for the LLM, the [Response](/appstore/modules/genai/commons/#response) entity defines how the output from the model must be formatted so it can be properly displayed in the chat interface. When an LLM returns a result, it needs to be converted into the Response entity’s format to ensure compatibility with ConversationalUI.

The Response entity includes key attributes such as:
- Messages - A list containing both historical messages and the new response from the model. This ensures that the full conversation history is maintained and displayed correctly in the chat interface. 
- Tool Call - Information that may be generated by the model in certain scenarios, such as function call patterns. 

Since different providers return responses in different formats, when implementing a new connector, the focus should be on mapping the provider’s response to match the Response entity’s structure. This step ensures seamless integration with the chat interface.

These two objects, Request and Response, are crucial for utilizing the chat functionalities provided to all users in ConversationalUI. However, in order to correctly call and interact with our LLM model, we also need to configure it properly.

This is where the DeployedModel entity comes into play. It defines the model that will be invoked, ensuring that the connector knows which LLM to call and how to communicate with it. By setting up this entity correctly, we can integrate our model seamlessly into the ConversationalUI framework.

The  [DeployedModel](/appstore/modules/genai/commons/#deployed-model) entity represents a GenAI model that can be invoked by a Mendix application. It includes a set of generic attributes that are commonly found across different LLM providers. However, since each provider may have additional model-specific details, DeployedModel does not cover all necessary attributes.
To accommodate this, you will need to create a new entity within your connector that inherits from the GenAICommons.DeployedModel entity. This allows you to extend it with any provider-specific attributes required for your integration.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/DeployedModel_Entity.png" >}}

For our Echo Connector, we will create an extended entity to ensure it includes any additional attributes needed to function properly. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/EchoDeployedModel_Entity.png" >}}

When creating your specialized DeployedModel entity and later working with its objects, there are two important factors to keep in mind.

First, your model will require an authentication method based on your provider’s requirements. Since authentication mechanisms vary between providers, you need to ensure your connector properly handles credentials and access tokens. This might involve API keys, OAuth tokens, or other authentication strategies depending on the provider you are integrating with.

To facilitate seamless model invocation, it's best to create an entity to store authentication details. A Configuration entity will be associated with the specialized EchoDeployedModel, allowing you to manage credentials separately from the deployed model itself. The specific attributes needed in this Configuration entity will depend on the requirements of your model. For now, I will create a basic example to demonstrate the structure.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/EchoConnector_DomainModel.png" >}}

The second important factor needs to be considered when creating EchoDeployedModel objects and that is the Microflow attribute, which exists in the generic DeployedModel entity. This attribute is crucial because it determines which microflow will be executed when invoking the model.

In the ChatCompletions_WithHistory microflow, the Java action named ChatCompletions_WithHistory is responsible for executing the correct microflow based on the DeployedModel type and its Microflow attribute. This design ensures that the action remains provider-agnostic, allowing different models to be integrated as long as they adhere to the same request-response structure. 

When creating your specialized EchoDeployedModel objects, you need to set the Microflow attribute to point to the correct microflow that will handle requests for your model (in this case, the Echo model’s implementation). The best method to set the Microflow attribute is by using the DeployedModel_Create or DeployedModel_SetMicroflow java actions in the GenAICommons module.

By properly configuring these attributes, your connector will be able to process chat requests within the existing framework. 

Earlier, we introduced the Echo Connector, a simple example connector where the input text is returned as the output. Despite its simplicity, it serves as a great demonstration of how to structure a connector to work seamlessly with ConversationalUI. By following the same approach, any LLM provider's response can be formatted correctly for integration into our chat interface.

To achieve this, we need to define a microflow that will handle the request and generate a response in the expected format. This microflow will be used as the Microflow attribute for the EchoDeployedModel objects, ensuring that when an Echo model is called, it follows the same structure required for chat interactions.
The Microflow attribute in all our current connectors is set to the GenAICommons.Request_ExecuteFromConnector java action. This Java action automates key processes such as function calling and storing usage information out of the box.

One of its input parameters is CallModelMicroflow, which is where we define the actual logic for calling the LLM. This is the microflow that directly interacts with the provider’s API or SDK, sending requests and processing responses. By customizing CallModelMicroflow, you can integrate your specific model while still leveraging the standardized execution flow provided by GenAICommons.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/Request_ExecuteFromConnector.png" >}}

So, I have created the microflow below to be used as the Microflow attribute for the EchoDeployedModel objects.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/ChatCompletions_CallLLM.png" >}}

And I have created the microflow below to be used as the CallModelMicroflow input parameter for the GenAICommons.Request_ExecuteFromConnector java action.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/EchoDeployedModel_CallLLM.png" >}}

As the microflow returns a Response object, it is fully compatible with the reusable components from the GenAICommons and ConversationalUI modules. This ensures that the response can be seamlessly processed and displayed in the existing chat interfaces, without requiring any additional customization on the UI side.