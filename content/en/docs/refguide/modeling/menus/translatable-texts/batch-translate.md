---
title: "Batch Translate"
url: /refguide/batch-translate/
weight: 30
---

## Introduction

**Batch translate** allows you to enter texts in one language which correspond to texts in another language.

Usually you will want to translate from the default language to a second language, but you can use any other dictionary of texts. For example, if your default language is *English, United States* you may already have translated the text into *Dutch, Netherlands* and you can use this as a reference for translating into *Dutch, Belgium* as there are likely to be more similarities.

{{< figure src="/attachments/refguide/modeling/menus/translatable-texts/batch-translate/batch-translate.png" width="600" >}}

## Using Batch Translate

Batch translate translates between two languages. When you select batch translate you will be asked to select the two languages you wish to use, a **Source language** to use as a reference, and a **Destination language** which is the one you want to update.

{{< figure src="/attachments/refguide/modeling/menus/translatable-texts/batch-translate/batch-translate-languages.png" alt="Select source and destination languages" width="600" >}}

### Documents/Modules

You can select one or more modules you want to use for batch translate. For example, you may have already got translations for imported and system modules and want to concentrate on translating your own modules.

Click **Select…** and check the modules you want to work on.

{{< figure src="/attachments/refguide/modeling/menus/translatable-texts/batch-replace/batch-replace-modules.png" alt="Module selection screen" width="400" >}}

The default is to work on all modules in the app.

### Search

To search for a particular phrase in the source language text, type what you want to search for. It is not possible to search for text in the destination language.

{{< figure src="/attachments/refguide/modeling/menus/translatable-texts/batch-translate/batch-translate-search.png" alt="Batch translate search" width="600" >}}

By default, all the translatable text from the selected module (or modules) will be shown.

Each found text will be displayed in the **Source** column.
The **#** column shows the number of times it occurs in the selected module (or modules).

If you select a line, you can look in the **Show occurrence** section to see the **Object** containing the text and the **Document** it appears in. Double-clicking or clicking **Show occurrence** will open the document and select the object so you can easily see the context.

{{% alert color="success" %}}
Tip: move the dialog box to one side to get a better look at the document.
{{% /alert %}}

### Translation

{{% alert color="info" %}}
Alternatively, you can use the AI-powered translation tool [Translation Generator](/refguide/translation-generator/) to generate translations for you. You can enable it via **Preferences** > the **New Features** tab > the **Maia** section.
{{% /alert %}}

In **Translation**, type new text that you want to use instead of the existing text. Click **Translate** to confirm the replacement.

{{< figure src="/attachments/refguide/modeling/menus/translatable-texts/batch-translate/batch-translate-translate.png" width="600" >}}

If you have two texts which are identical in the source language but different in the translation language, you will have to review and change these individually. This is not very common but imagine, for example, you have used `Order Lines` to both *describe the lines on an order* and to *label a button which sorts some lines*. See [Working in the Currently Selected Language](/refguide/translatable-texts/#selected-language) in *Language Menu* to find how to change individual texts.

## Exporting and Importing Text {#export-import}

If you want to translate a language outside Studio Pro, you can export the translatable texts to the Microsoft Excel (*.xlsx*) format, make changes, and then import the changes from the updated Excel file.

This is particularly useful if you are working on multiple apps and you want to apply your translations to a different app.

### Export to Excel {#export}

Click **Export to Excel…** to export the currently displayed text items to a Microsoft Excel (*.xlsx*) format file.

The file will be in the format shown below:

{{< figure src="/attachments/refguide/modeling/menus/translatable-texts/batch-translate/batch-translate-excel.png" alt="Sample Excel file" width="400" >}}

**Row 1** – *Filter:* indicates the modules which are included in the exported file.

**Row 2** – indicates the source and translation language. The first column represents the current text, the second column the *translation* text.

**Rows 3+** – show the current texts

You can make changes in column B which will be processed if the file is imported.

### Import from Excel {#import}

Click **Import from Excel…** to import a correctly-constructed Microsoft Excel (*.xlsx*) format file.

This does the following:

* The selected module (or modules) are set to the ones in the *Filter:* line of the file
* Any texts which are empty in column B will be ignored
* Any texts in column A which do not match translatable texts in the selected module (or modules) will be ignored
* Any text in column B which is not ignored is entered into the **Translation** column

Changes will only be made if you click **Translate**.

{{% alert color="warning" %}}
The formats of the Excel files for batch translate and batch replace are similar. You will be warned if you try to import a batch replace file or a batch translate file with the incorrect languages but you can still import it if you ignore the warning.
{{% /alert %}}

## Read More

* [Translation Generator](/refguide/translation-generator/)
* [Mendix AI Assistance (Maia)](/refguide/mendix-ai-assistance/)
