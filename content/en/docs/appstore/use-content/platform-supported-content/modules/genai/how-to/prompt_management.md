---
title: "Integrate Prompt Management into your app"
url: /appstore/modules/genai/how-to/howto-prompt-management/
linktitle: "Integrate Prompt Management"
weight: 30
description: "This document guides you through integrating prompt management in your Mendix application to enable users to prompt engineer at runtime."
aliases:
---

## Introduction

This document explains how to integrate the [Prompt Management](/appstore/modules/genai/genai-for-mx/prompt-management/) capabilities of the ConversationalUI module in your smart app. This how-to rebuilds a simplified version of an example that is implemented in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475). To follow along,you can use your existing app or start from scratch as described in the [Build a Smart App from a Blank GenAI App](/appstore/modules/genai/how-to/blank-app/) guide.

Through this document, you will:

* Understand how to implement prompt management in your Mendix application.
* Enable AI experts to prompt engineer in your running application.
* Learn how you can send a crafted prompt to an LLM of your choice.

### Prerequisites {#prerequisites}

Before integrating prompt management into your app, make sure you meet the following requirements:

* An existing app: either an app that you've already built or preferable the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475). You can also start from scratch using the [Blank GenAI App](https://marketplace.mendix.com/link/component/227934).

* Installation: If not done already, install the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle from the Mendix marketplace.

* Access to an LLM of your choice. In this how-to the [Mendix Cloud GenAI Resources Packs](/appstore/modules/genai/MxGenAI/) are used, but you can use any provider with a connector that is compatible with [GenAICommons](/appstore/modules/genai/genai-for-mx/commons/) (such as [OpenAI](/appstore/modules/genai/reference-guide/external-connectors/openai/) or [Amazon Bedrock](/appstore/modules/aws/amazon-bedrock/)). 

* Basic understanding of GenAI concepts: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarize yourself with the [concepts](/appstore/modules/genai/using-gen-ai/).

* Basic understanding of Prompt Engineering: Learn about [Prompt Engineering](/appstore/modules/genai/get-started/#prompt-engineering) to use them within the Mendix ecosystem.

## Use Case {#use-case}
tbd

## Integrate Prompt Management {#integrate-prompt-management}

Prompt Management is a capability of the ConversationalUI module that is part of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle. It enables users to create and engineer prompts at runtime. The following steps describe how you can add the capabilities to your app and navigation:

1. Open the [Security settings](/refguide/security/#user-role) of your project and edit the user role that should later be able to create prompts at runtime. This is typically some sort of admin role, but this depends on your use case.

2. Search for the **ConversationalUI** module and select at least the module roles **PromptAdmin** and **User**.

3. Search for the **MxGenAIConnector** module and select the module role **Administrator**. Save the security settings.

4. Go to Navigation and add a new item "Prompt Management" to the menu, choose a suitable icon (for example `notes-paper-text` from the Atlas category) and use the `Show Page` On-Click event. Search for and select `Prompt_Overview` which is located in **ConversationalUI** > **USE_ME** > **Prompt Management**. Alternatively, you can add a button to a page and connect to the same page.

5. If you haven't started from a GenAI Starter App, you also need to add a navigation item that calls the **NAV_ConfigurationOverview_Open** microflow of the **MxGenAIConnector** (see [Configuration](/appstore/modules/genai/mx-cloud-genai/MxGenAI-connector/#configuration) for more details).

You can now run the app, login as administrator and test if you can navigate to the Prompt_Overview and MxCloud Configuration pages. If you already have a key for a **Text Generation** resource, you might import it now(for more details see [Mendix Cloud GenAI](/appstore/modules/genai/mx-cloud-genai/)).

## Create Your First Prompt {#reate-prompt}

You can now create your first prompt in the user interface. The final prompt will look like this:

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-prompt-management/prompt_details.png" >}}

### Initial Prompt

1. In the running app, open the *Prompt Management* overview page that was added to your navigation in the previous section.

2. Click the **New Prompt** button in the top-right corner.

3. You are asked to provide a **title** and **description** for your prompt. For title you can use `Product Description Generator` and optionally for description `How-To example to let the model generate a product description based on user's input`.

4. You can now choose a *Usage type* to either create a `Single-Call` or `Conversational` prompt. The main difference between those two is that *Conversational* prompts are meant to be used in chats which contain the whole conversation and no predefined-user prompt, while the *Single-Call* is meant for a single interaction between the user and the LLM. In this example we use the `Single-Call` type. Save the prompt.

5. You are now navigated to the prompt's details page which allows you to prompt engineer at runtime. Add to the **User Prompt** field the following prompt: `Generate a short product description for a chair.`. The user prompt is typically what the enduser writes, even though it can be prefilled by our own instructions.

6. You can now hit **Run** in the top-right corner to see the model's response. However, because you have not selected a model yet, you are asked to do so before running the test. If there are no models to select from, you first need to configure a model (for MxCloud you need to import a key on the configuration page that you have added in the previous section). You can later change the model by clicking the *Cog* icon left to the *Run* button.

7. Below you can observe the outcome. This is already sufficient for the first try. You can now save this version of the prompt by hitting the **Save As** button in the *prompt card*. For title use `Simple product description prompt` and save it. The prompt cannot be edited anymore.

### Iterate and First Test Case

8. To further improve your prompt and the user experience for the end users, you can now add some placeholder variables. Next to the version's dropdown you can click the icon-button with the plus to create a new draft version. Change the *User Prompt* to `Generate a short product description for a {{ProductName}}. The description should not be longer than {{NumberOf Words}} words. `. 

9. Notice that two variables were created in the right card. Those can later be used in your application to let users flexibly change the user prompt without even knowing what a prompt is and without the application to be changed nor restarted. You can now enter two values for the variables: `30` for **NumberOfWords** and `chair` for **ProductName**. Hit **Run** to see how the model changed the output considering a different prompt.

10. The values that you entered for the variables are only available in the prompt management capability, but not for your use case. You can now **Save As** the test case which makes it available for later test runs. Use `Chair 30 words` as title. 

### System Prompt and Multiple Test Cases

11. Save the prompt's version one more time as you did in *step 7*. Enter `Added user input` as title. For the final version, we can now add additional instructions to the *system prompt*. Enter `You are a sales assistant that can write engaging and inspiring product descriptions for our online marketplace. The user asks you to create a description for various products. You should always respond in {{Language}}.` and notice that the *Language* variable was created.

12. Add a new test case by clicking the `+` icon next to the test case drop down. For *Language* you can enter any language (preferably not English to test it properly), in this example `German` is used. The other two variables can be the same values as above: `30` and `chair`. **Run** the test case. Save the test case with the title `Chair 30 words German`.

13. Now that you saved at least two test cases, you can click the arrow next to the *Run* button to open a dropdown and click **Run All**. Both test cases are executed and you can compare the different input values. Note that the language variable was not filled for the first test case because it did not exist, so it might either be in English or a random language.

14. Once you are satisfied with your prompt, you can now save the version one more time with the title `Added system prompt and language`.

You now successfully created your first prompt. There are a few configurations that are still needed which will be explained later in this how-to.

## Create Context Entity {#context-entity}
In order to connect a prompt with the rest of your application, it is helpful to create an entity which contains attributes to fill in user's input into prompt variables

## Connect your Prompt with your App {#connect-prompt-with-app}

## Testing and Troubleshooting {#testing-troubleshooting}

Empty model selection

Empty owner field

No context entity found

Attributes do not match variables (same naming)

Encryption Key missing

## Read More {#read-more}
* If you seek more technical documentation and details, you can learn more on the [Prompt Management](/appstore/modules/genai/genai-for-mx/prompt-management/) documentation page. 

* As already mentioned, there is a more advanced prompt management example in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) called *Generate Product Description (Prompt Management)* which you can easily follow after completing this how-to.
