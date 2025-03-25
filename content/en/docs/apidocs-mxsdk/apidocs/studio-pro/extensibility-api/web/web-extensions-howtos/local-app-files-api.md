---
title: "Local App Files Api"
url: /apidocs-mxsdk/apidocs/extensibility-api/web/local-app-files-api/
weight: 5
---

# Introduction

This guide will show you how to interact with the local application files from within an extension

# Prerequisites

This guide builds ontop of the [getting started guide](/apidocs-mxsdk/apidocs/extensibility-api/web/getting-started/). Please complete that guide before starting this one.

# Adding some interactivity

Open src/ui/index.tsx

replace the contents of the file with the following code:

```typescript
import { studioPro } from "@mendix/extensions-api";
import { StrictMode, useCallback } from "react";
import { createRoot } from "react-dom/client";

const saveFile = async () => {
        await studioPro.app.files.putFile("HelloWorld.txt", "Hello world from a file!");
        studioPro.ui.messageBoxes.show("info", "Saving HelloWorld.txt");
    };

const loadFile = async () => {
        const message = await studioPro.app.files.getFile("HelloWorld.txt");
        studioPro.ui.messageBoxes.show("info", `Loaded HelloWorld.txt it contained: ${message}`);
    };

const deleteFile = async () => { 
        await studioPro.app.files.deleteFile("HelloWorld.txt");
        studioPro.ui.messageBoxes.show("info", "Deleted HelloWorld.txt");
    };

createRoot(document.getElementById("root")!).render(
    <StrictMode>
        <h1>Mendix Studio Pro Extension</h1>
        <p>Hello from an extension!</p>
        <p>
            <button onClick={saveFile}>Save Hello world File</button>
            <button onClick={loadFile}>Load Hello world File</button>
            <button onClick={deleteFile}>Delete Hello world File</button>
        </p>
    </StrictMode>
);
```

This code adds 3 new buttons to the tab. We also add 3 new event handlers for saving, loading and deleting a file called HelloWorld.txt

Lets have a look at all the changes:

```typescript
const saveFile = async () => {
        await studioPro.app.files.putFile("HelloWorld.txt", "Hello world from a file!");
        studioPro.ui.messageBoxes.show("info", "Saving HelloWorld.txt");
    };
```
The saveFile callback calls the putFile api setting the filename to `HelloWorld.txt` and containing `Hello World from a File!`

```typescript
const loadFile = async () => {
        const message = await studioPro.app.files.getFile("HelloWorld.txt");
        studioPro.ui.messageBoxes.show("info", `Loaded HelloWorld.txt it contained: ${message}`);
    };
```
The loadFile callback calls the getFile api requesting to load `HelloWorld.txt`

```typescript
const deleteFile = async () => { 
        await studioPro.app.files.deleteFile("HelloWorld.txt");
        studioPro.ui.messageBoxes.show("info", "Deleted HelloWorld.txt");
    };
```
The deleteFile callback calls the deleteFile api requesting to delete `HelloWorld.txt`

```typescript
createRoot(document.getElementById("root")!).render(
    <StrictMode>
        <h1>Mendix Studio Pro Extension</h1>
        <p>Hello from an extension!</p>
        <p>
            <button onClick={saveFile}>Save Hello world File</button>
            <button onClick={loadFile}>Load Hello world File</button>
            <button onClick={deleteFile}>Delete Hello world File</button>
        </p>
    </StrictMode>
);
```
The final part of our changes adds 3 new buttons which call the callbacks we defined at the top of the file when clicked.

## Some restrictions

The App files api allows you to modify files within your applications folder. It will not serve restricted files such as the mpr file or contents of some folders like .git folder. Additionally it will not files outside of the app folder.

# Conclusion

You should now be able to save, load and delete files from within the app folder using an extension.

# Extensibility Feedback

 If youd like to provide us with some additional feedback you can complete a small [Survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback)

 Any feedback is much appreciated.
