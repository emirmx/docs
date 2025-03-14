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

Before diving into building your own connector, first assess whether you actually need to start from scratch. If your provider follows a similar API structure to an existing connector, it is probably wise to take the existing connector's code as a basis for your module and modify what is needed. E.g., if your provider's API is REST-based with JSON payloads that look like OpenAI's JSON snippets, it is likely you can reuse a lot of the microflows and logic from the OpenAIConnector. Additionally, you might have a use case where you are running your own custom model on a private server or another cloud environment and want to connect to it, rather than relying on existing connectors. Even in such cases, the OpenAIConnector can still serve as a solid starting point, allowing you to adapt and extend it to fit your specific needs. We published a blog on [How to Run Open-Source LLMs Locally with the OpenAI Connector and Ollama](https://www.mendix.com/blog/how-to-run-open-source-llms-locally-with-the-openai-connector-and-ollama/), which might help you already.

However, if your provider has a different authentication mechanism, uses an SDK (like Bedrock's Java SDK), or requires a different request-response format, creating a new connector may be necessary. If that's the case, this guide will walk you through the process of integrating your provider while ensuring full compatibility with our ConversationalUI module.

## How to Determine the Right Approach When Building Your Own Connector?

When developing your own GenAI Connector, there are two possible approaches:

1. Starting from an existing connector (e.g., OpenAIConnector) 
2. Building from scratch (starting from the Echo Connector) 

Choosing the right approach depends on how similar your target provider's API is to OpenAI's. The best way to determine this is to inspect the API of the provider/model and compare it to OpenAI's API structure.

### Starting from the OpenAIConnector

If the provider's API is the same or very similar to OpenAI's, this might be a good indicator that you can duplicate the OpenAIConnector module and make the necessary adjustments. Some logical modifications include:
- Small changes in the request/response payload (e.g., extra or fewer fields, slightly different JSON structure). 
- Modifying the base URL to match the provider's endpoint structure. 
- Adding additional query parameters in the URL or payload. 
- Adapting the authentication mechanism, for example, switching from Bearer Token or API key headers to OAuth. 

This approach allows you to reuse a well-structured connector, minimizing development effort while ensuring compatibility with ConversationalUI.

### Starting from the Echo Connector

If the provider's API differs significantly from OpenAI's, it's best to start from the Echo Connector. This ensures that the input and output structures are properly aligned with GenAICommons, making integration with ConversationalUI straightforward.
A reason to take this approach might be if the provider requires a different protocol. This often means the communication structure and authentication methods will differ significantly, making it easier to build the connector from scratch rather than modifying an existing REST-based connector.

Additionally, refer to [GenAI Commons](/appstore/modules/genai/commons/) to explore what is available out of the box, which can help you accelerate development. Pay close attention to:
- The domain model (data structure) to see how existing entities can be reused. 
- The "connector building" folders, which contain useful microflows and helper activities for working with different providers. 

If you would like to look inside of the GenAICommons module, you can inspect this [public repository](https://github.com/mendix/genai-showcase-app). This structured approach ensures that you can build your GenAI Connector efficiently while taking full advantage of the existing Mendix functionalities. 

## Build your own connector

{{% alert color="info" %}}
The echo connector is a module in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) and can be used as a starting point to build your own connector.
{{% /alert %}}

### Chat Completions: With History

In this guide, we will focus on implementing chat completions, a fundamental capability supported by most LLMs.  To make the process more practical, we will develop an example connector—an Echo Connector. This simple connector will return the same text as output that was given in the input, but it will be fully compatible with the chat capabilities of GenAICommons and ConversationalUI.
To enable chat completion, the key microflow to understand is ChatCompletions_WithHistory that is in the GenAICommons module. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/ChatCompletions_WithHistory.png" >}}

This microflow plays a crucial role because it is the core process used across all our connectors and the ConversationalUI-based chat interface makes use of it. To properly integrate with it, the microflow needs to supply two essential input objects:
- DeployedModel - Represents the specific model being used and determines which connector (microflow) is being called
- Request - Contains the details of the user's input and conversation history as well as other configurations.

And one output object:
- Response - Contains the details of the LLM's results.

Since this structure is already standardized, no modifications are needed for the Request entity itself. Instead, when implementing a new connector, the focus should be on properly mapping the request data from the existing Request object to the format expected by the specific provider, which in our case will be the Echo Connector. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/GenAICommons_DomainModel.png" >}}

Just as the Request entity structures input for the LLM, the [Response](/appstore/modules/genai/genai-for-mx/commons/#response) entity defines how the output from the model must be formatted so it can be properly displayed in the chat interface. When an LLM returns a result, it needs to be converted into the Response entity’s format to ensure compatibility with ConversationalUI.

The Response entity includes key attributes such as:
- Messages - A single message that the model generated. 
- Tool Call - A request from the model to call one or multiple tool(s), for example a microflow. Available tools are defined in the request via the [ToolCollection](/appstore/modules/genai/genai-for-mx/commons/#toolcollection).

Since different providers return responses in different formats, when implementing a new connector, the focus should be on mapping the provider’s response to match the Response entity’s structure. 

### Deployed Model

#### Specialization

The Request and Response objects are crucial for utilizing the chat functionalities provided to users in ConversationalUI. However, in order to correctly call and interact with an LLM model, we also need to configure it properly. This is where the [DeployedModel](/appstore/modules/genai/genai-for-mx/commons/#deployed-model) entity comes into play. It represents a GenAI model that can be invoked by a Mendix application, ensuring that the connector knows which microflow to call and how to communicate with the model. Additionally, it includes a set of generic attributes that are commonly found across different LLM providers. However, since each provider may have additional model-specific details, DeployedModel does not cover all necessary attributes. To accommodate this, you will need to create a new entity within your connector that inherits from the GenAICommons.DeployedModel entity. This allows you to extend it with any provider-specific attributes required for your integration.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/DeployedModel_Entity.png" >}}

For our Echo Connector, we create specialization from DeployedModel to ensure it includes any additional attributes needed to function properly. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/EchoDeployedModel_Entity.png" >}}

#### Authentication

Your model will require an authentication method based on your provider’s requirements. Since authentication mechanisms vary between providers, you need to ensure your connector properly handles credentials and access tokens. This might involve API keys, OAuth tokens, or other authentication strategies depending on the provider you are integrating with.

To facilitate seamless model invocation, it's best to create an entity to store authentication details. A Configuration entity will be associated with the specialized EchoDeployedModel, allowing you to manage credentials separately from the deployed model itself. The specific attributes needed in this Configuration entity will depend on the requirements of your model. A basic example can look like this:

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/EchoConnector_DomainModel.png" >}}

#### Microflow

The `Microflow` attribute, which exists in the generic DeployedModel entity, needs to be set when creating EchoDeployedModel objects. This attribute is crucial because it determines which microflow will be executed when invoking the model. In the ChatCompletions_WithHistory microflow, the Java action `ChatCompletions_WithHistory` executes the correct microflow based on the DeployedModel type and its Microflow attribute. This design ensures that the action remains provider-agnostic, allowing different models to be integrated as long as they adhere to the same request-in and response-out interface. 

When creating your specialized EchoDeployedModel objects, you need to set the Microflow attribute to point to the correct microflow that will handle requests for your model (in this case, the Echo model’s implementation). To set the Microflow attribute you can use the `DeployedModel_Create` or `DeployedModel_SetMicroflow` java actions in the GenAICommons module.
<!--- not needed imo
By properly configuring these attributes, your connector will be able to process chat requests within the existing framework. 

Earlier, we introduced the Echo Connector, a simple example connector where the input text is returned as the output. Despite its simplicity, it serves as a great demonstration of how to structure a connector to work seamlessly with ConversationalUI. By following the same approach, any LLM provider's response can be formatted correctly for integration into our chat interface.
-->
To achieve this, we need to define a microflow that will handle the request and generate a response in the expected format. This microflow will be used as the Microflow attribute for the EchoDeployedModel objects, ensuring that when an Echo model is called, it follows the same structure required for chat interactions.

The Microflow attribute in all our current connectors is set to the GenAICommons.Request_ExecuteFromConnector java action. This Java action automates key processes such as function calling and storing usage information out of the box. To properly keep track of your consumption, the token-related attributes in the Response object need to be set correctly. Please keep this in mind when formulating the microflow to ensure accurate usage tracking and cost monitoring.

One of its input parameters is CallModelMicroflow, which is where we define the actual logic for calling the LLM. This is the microflow that directly interacts with the provider’s API or SDK, sending requests and processing responses. By customizing CallModelMicroflow, you can integrate your specific model while still leveraging the standardized execution flow provided by GenAICommons.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/Request_ExecuteFromConnector.png" >}}

So, I have created the microflow below to be used as the Microflow attribute for the EchoDeployedModel objects.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/ChatCompletions_CallLLM.png" >}}

And I have created the microflow below to be used as the CallModelMicroflow input parameter for the GenAICommons.Request_ExecuteFromConnector java action.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-byo/EchoDeployedModel_CallLLM.png" >}}

As the microflow returns a Response object, it is fully compatible with the reusable components from the GenAICommons and ConversationalUI modules. This ensures that the response can be seamlessly processed and displayed in the existing chat interfaces, without requiring any additional customization on the UI side.