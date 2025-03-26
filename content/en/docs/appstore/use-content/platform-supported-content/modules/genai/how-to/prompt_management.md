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

* An existing app: either an app that you've already built or you can also start from scratch using the [Blank GenAI App](https://marketplace.mendix.com/link/component/227934).

* Installation: If not done already, install the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle from the Mendix marketplace.

* Access to an LLM of your choice. In this how-to the [Mendix Cloud GenAI Resources Packs](/appstore/modules/genai/MxGenAI/) are used, but you can use any provider with a connector that is compatible with [GenAICommons](/appstore/modules/genai/genai-for-mx/commons/) (such as [OpenAI](/appstore/modules/genai/reference-guide/external-connectors/openai/) or [Amazon Bedrock](/appstore/modules/aws/amazon-bedrock/)). 

* Basic understanding of GenAI concepts: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarize yourself with the [GenAI Concepts](/appstore/modules/genai/using-gen-ai/).

* Basic understanding of Mendix: knowledgeable of simple page building, microflow modelling and domain model creation (everything is explained step-by-step below).

## Use Case {#use-case}

This how-to shows how you can build a very simple user interface where users can create a product description for their products. By enriching the app with GenAI, you can let an LLM generate the product description based on a prompt that you configured. The how-to will explain how you can integrate the prompt management capabilities to your app and how you can craft your first prompt in the UI at runtime. In the user interface, the user can provide input for the product name and the desired length of the description. The input will be inserted into the prompt that the admin previously created which will then be sent to the LLM. The user can inspect the response.

This use case is a simplified version of the *Generate Product Description (Prompt Management)* example of the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) which you can also test yourself to improve your knowledge.

## Integrate Prompt Management {#integrate-prompt-management}

Prompt Management is a capability of the ConversationalUI module that is part of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle. It enables users to create and engineer prompts at runtime. The following steps describe how you can add the capabilities to your app and navigation:

1. Open the [Security settings](/refguide/security/#user-role) of your project and edit the user role that should later be able to create prompts at runtime. This is typically some sort of admin role, but this depends on your use case.

2. Search for the **ConversationalUI** module and select at least the module roles **PromptAdmin** and **User**.

3. Search for the **MxGenAIConnector** module and select the module role **Administrator**. Save the security settings.

4. Go to Navigation and add a new item "Prompt Management" to the menu, choose a suitable icon (for example `notes-paper-text` from the *Atlas* category) and use the `Show Page` On-Click event. Search for and select `Prompt_Overview` which is located in **ConversationalUI** > **USE_ME** > **Prompt Management**. Alternatively, you can add a button to a page and connect to the same page.

5. If you haven't started from a GenAI Starter App, you also need to add a navigation item that calls the **NAV_ConfigurationOverview_Open** microflow of the **MxGenAIConnector** (see [Configuration](/appstore/modules/genai/mx-cloud-genai/MxGenAI-connector/#configuration) for more details).

You can now run the app, login as administrator and test if you can navigate to the Prompt_Overview and MxCloud Configuration pages. If you already have a key for a **Text Generation** resource, you might import it now (for more details see [Mendix Cloud GenAI](/appstore/modules/genai/mx-cloud-genai/)).

## Create Your First Prompt {#reate-prompt}

You will now create your first prompt in the user interface. The final prompt will look like this:

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-prompt-management/prompt_details.png" >}}

### Initial Prompt

1. In the running app, open the *Prompt Management* overview page that was added to your navigation in the previous section.

2. Click the **New Prompt** button in the top-right corner.

3. You are asked to provide a **title** and **description** for your prompt. For title you can use `Product Description Generator` and optionally for description `How-To example to let the model generate a product description based on user's input`.

4. You can now choose a *Usage type* to either create a `Single-Call` or `Conversational` prompt. The main difference between those two is that *Conversational* prompts are meant to be used in chats which contain the whole conversation and no predefined-user prompt, while the *Single-Call* is meant for a single interaction between the user and the LLM. In this example use the `Single-Call` type. **Save** the prompt.

5. You are now navigated to the prompt's details page which allows you to prompt engineer at runtime. Add to the **User Prompt** field the following prompt: `Generate a short product description for a chair.`. The user prompt is typically what the enduser writes, even though it can be prefilled by our own instructions.

6. You can now hit **Run** in the top-right corner to see the model's response. However, because you have not selected a model yet, you are asked to do so before running the test. If there are no models to select from, you first need to configure a model (for MxCloud you need to import a key on the configuration page that you have added in the previous section). You can later change the model by clicking the *Cog* icon left to the *Run* button.

7. In the outcome card you can observe the model's response. This is already sufficient for the first try. You can now save this version of the prompt by hitting the **Save As** button in the *prompt card*. For title use `Simple product description prompt` and save it. The prompt cannot be edited anymore.

### Iterate and First Test Case

8. To further improve your prompt and the user experience for the end users, you can now add some placeholder variables. Next to the version's dropdown you can click the icon-button with the plus to create a new draft version. Change the *User Prompt* to `Generate a short product description for a {{ProductName}}. The description should not be longer than {{NumberOfWords}} words. `

9. Notice that two variables were created in the right *test case card*. Those can later be used in your application to let users flexibly change the user prompt without even knowing what a prompt is and without the application to be changed nor restarted. You can now enter two values for the variables: `30` for **NumberOfWords** and `chair` for **ProductName**. Hit **Run** to see how the model changed the output considering a different prompt.

10. The values that you entered for the variables are only available in the prompt management capability, but not for your use case. You can now **Save As** the test case which makes it available for later test runs. Use `Chair 30 words` as title. 

### System Prompt and Multiple Test Cases

11. Save the prompt's version one more time as you did in *step 7*. Enter `Added user input` as title. For the final version, additional instructions can now be added as part of the *system prompt*. Enter `You are a sales assistant that can write engaging and inspiring product descriptions for our online marketplace. The user asks you to create a description for various products. You should always respond in {{Language}}.` and notice that the *Language* variable was created.

12. Add a new test case by clicking the `+` icon next to the test case drop down. For *Language* you can enter any language (preferably not English to test it properly), in this example `German` is used. The other two variables can be the same values as above: `30` and `chair`. **Run** the test case. Save the test case with the title `Chair 30 words German`.

13. Now that you saved at least two test cases, you can click the arrow next to the *Run* button to open a dropdown and click **Run All**. Both test cases are executed and you can compare the different input values. Note that the language variable was not filled for the first test case because it did not exist, so it might either be in English or a random language.

14. Once you are satisfied with your prompt, you can now save the version one more time with the title `Added system prompt and language`.

You now successfully created your first prompt. There are a few configurations that are still needed which will be explained later in this how-to.

## Create User Interface {#context-entity}
In order to connect a prompt with the rest of your application, it is helpful to create an entity which contains attributes to use user's input to fill the prompt variables. In this section you will both create the entity and user interface. The final page will look like this:

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-prompt-management/prompt_user_interface.png" >}}

1. In Studio Pro, go to your module's domain model (for new apps *MyFirstModule*). Create an entity with the name `Product`.

2. Add the following attributes:
    * **ProductName** as *String*
    * **NumberOfWords** as *Integer*
    * **Language** as *String*
    * **ProductDescription** as *String* and set length to unlimited

3. Change the **Access rules** of the entity to grant *read-write* access for *ProductName*, *NumberOfWords* and *ProductDescription* to your **User** and **Administrator** role. Give the roles permission for `Allow creating new objects`. Save the entity.

4. Create a blank responsive web page called **Product_NewEdit** with `Atlas_Default` as layout.

5. Add a dataview to the page. Set the *Form orientation* to `Vertical`. Select your newly created entity `Product` as data source *Context*. Click **OK**. Let Studio Pro automatically fill the content of the data view.

6. Remove the `Language` input box, because this will not be filled by users.

7. Grant access rights to the page for both roles by changing `Visible for` in the *navigation* category of the page's properties.

8. Add a button `Generate product description`, which will later execute the prompt. Place the button right before the `Product Description` input field.

9. Go to your app's navigation. Add a new item called `Add product` which should *On click* `Create object` of entity `Product` and open the `Product_NewEdit` page.  For icon you may choose `add` from the *Atlas* category. Alternatively, you can add a button to a page and connect to the same page via the create object event. 

Now a user can create a new product in the UI, but the process was not yet enhanced with any AI.

## Connect your Prompt with your App {#connect-prompt-with-app}

In this section, the prompt that was already created needs to be connected with our user interface to let an LLM create the product description for us.

### Finalize Your Prompt {#finalize-prompt}

You first need to configure some additional settings for the prompt before it can be used in your app.

1. Run the app. Navigate to your prompt.

2. Click the **microflow** icon with the `X` left to the *Run* button. A pop-up is opened where you can select the context entity. Search for **Product** and select the entity that was created in the previous section. When starting from the Blank GenAI App, this should be **MyFirstModule.Product**. Click **Save**.

3. Notice that the *Microflow* icon changed to indicate that the context entity was selected correctly. In the background it was checked if all variables can be found in the attributes of the selected entity. If the variables were spelled differently than the attribute names, you should see a warning sign in the icon and a helpful text when you click on it. Below the three variables an info text appears indicating that you have not used all attributes as variables. This is nothing to worry about, just a helpful hint in the case that you missed a variable. In our example, the `ProductDescription` attribute is a placeholder for the model's response and thus not part of the user or system prompt.

4. Navigate back to the Prompt Overview (via the breadcrumb `Overview`).

5. Hover over the *Ellipsis* icon (three horizontal dots) in the row of your prompt and click the **Select Prompt in use** button. On this page, you need to select a version that you want to set to `In Use` which means it is selected for production and later selected in your microflow logic. Select the latest version `Added system prompt and language` and click **Select**.

### Enable Generation Microflow {#generation-microflow}

Now you will create the microflow that is called when a user hits the button. This microflow execute a call to the LLM and sets the *ProductDescription* attribute's value to the model's response. The microflow can also be found in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) in **ExampleMicroflows** > **Programmatic Prompt** > **ACT_Product_GenerateProductDescription** and will look like this:

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-prompt-management/prompt_microflow.png" >}}

6. In Studio Pro, go to the `Product_NewEdit` page. Open the button and change the *On click* event to `Call a microflow`. Click *New* to create a new microflow called `ACT_Product_GenerateProductDescription`. Click **Ok** to close the button's propteries.

7. Open the newly created microflow. First you need to grant the module roles access. Change the `Allowed roles` selection under the *Security* category and add both roles.

8. As a first action in the microflow, add a `Change object` action to change the **Language** attribute:
    * Object: `Product` (input parameter)
    * Member: `Language`
    * Value: `English` (you can use whatever language. This is just an example to show that you can have input for the prompt that is not defined by your users)

9. Add a `Retrieve` action to the microflow to retrieve the Prompt that you created in the UI:
    * Source: `From database`
    * Entity: `ConversationalUI.Prompt` (search for *Prompt*)
    * XPath constraint: `[Title = 'Product Description Generator']`
    * Range: `First`
    * Object name: `Prompt` (default)

10. Add the `Get Prompt For Context Object` action from the toolbox to get the `PromptToUse` object that has the variables replaced by the user's input:
    * Prompt: `Prompt` (the object that was previously retrieved in step 6)
    * Context object: `Product` (input parameter)
    * Object name: `PromptToUse` (default)

11. Add the `Create Request` action to set the system prompt:
    * System Prompt: `$PromptToUse/SystemPrompt` (expression)
    * Temperature: empty (expression; optional)
    * MaxTokens: empty (expression; optional)
    * TopP: empty (expression; optional)
    * Object name: `Request` (default)

12. Add the `Chat Completions (without history)` action to call the model:
    * DeployedModel: `$Prompt/ConversationalUI.Prompt_DeployedModel/GenAICommons.DeployedModel` (expression)
    * UserPrompt: `$PromptToUse/UserPrompt` (expression)
    * OptionalFileCollection: empty (expression)
    * OptionalRequest: `Request` (the object that was previously retrieved in step 8)
    * Obect name: `Response` (default)

13. Lastly, add a `Change object` action to change the **ProductDescription** attribute:
    * Object: `Product` (input parameter)
    * Member: `ProductDescription`
    * Value: `$Response/ResponseText` (expression)


You now successfully implemented prompt management and connected it to an example use case, so that users can now let the model generate a product description based on two input fields and the prompt that was previously created. When you run the app once more, you can test the use case yourself!

## Troubleshooting {#troubleshooting}

### Model selection is empty {#empty-model-selection}

When you want to run your prompt from the prompt management page, you need to select a model. If the list is empty, you likely have not configured a model yet using one of the platform-supported (or other GenAICommons compatible) connectors. Also make sure that the model support `SystemPrompt` as well as `Text` as output modality.

### Context Entity issues {#context-entity-issues}

When you are to select the `Context entity` in the UI but cannot find the one you're are seeking, you might need to restart your application after the entity was added to your domain model.

If the attributes do not match the variables, for example you noticed a warning in the UI or Console of your running app, you might have used inconsistent names for the `{{variables}}` inside of your prompts compared to the attribute names. Double check if they are exactly the same (no whitespace or other characters).

### "Owner" of Prompt is empty {#owner-is-empty}

If the `Owner` field on the `Prompt_Overview` page is empty, you are likely logged in as `MxAdmin` which doesn't have a name linked to it. For other users, the *Owner* field should be populated. This should not change the behavior of this how-to.

## Read More {#read-more}
* If you seek more technical documentation and details, you can learn more on the [Prompt Management](/appstore/modules/genai/genai-for-mx/prompt-management/) documentation page. 

* As already mentioned, there is a more advanced prompt management example in the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) called *Generate Product Description (Prompt Management)* which you can easily follow after completing this how-to.
