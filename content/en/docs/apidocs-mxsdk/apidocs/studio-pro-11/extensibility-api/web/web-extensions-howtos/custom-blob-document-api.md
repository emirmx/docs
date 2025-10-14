---
title: "Register New Document Types with Corresponding Editor"
linktitle: "Introduce New Document Types"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/custom-blob-document-api/
---

## Introduction

This how-to describes how to introduce a new document type and provide a custom editor so users can edit documents of that type.

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/).

## Custom Document Model

Studio Pro lets you extend its metamodel by introducing custom document types. These documents can store arbitrary data that must be serializable as strings. If an editor (a user-defined UI component) is registered for a document type, documents of that type will appear in the UI like any other built-in document type (for example, constants, Java Actions, or pages). In particular, they will appear in the New Document and Find Advanced dialogs, context menus for adding documents, the App Explorer, and other UI elements that show Studio Pro documents. Custom editors can be registered to appear either as tabs or as modal dialogs.

## Registering a New Document Type

To register a new document type, generate a new extension called `myextension` as described in the [Getting started guide](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/).
Below we explain which files of the generated extension to edit and what those edits achieve.

First, replace the contents of `src/main/index.ts` with:

```typescript {hl_lines=["8-24"]}
import { IComponent, getStudioProApi } from "@mendix/extensions-api";
import { personDarkThemeIcon, personDocumentType, personLightThemeIcon } from "../model/constants";
import { PersonInfo } from "../model/PersonInfo";

export const component: IComponent = {
    async loaded(componentContext) {
        const studioPro = getStudioProApi(componentContext);
        await studioPro.app.model.customBlobDocuments.registerDocumentType<PersonInfo>({
            type: personDocumentType,
            readableTypeName: 'Person',
            defaultContent: {
                firstName: '',
                lastName: '',
                age: 0,
                email: ''
            }
        });
        await studioPro.ui.editors.registerEditorForCustomDocument({
            documentType: personDocumentType,
            editorEntryPoint: 'editor',
            editorKind: 'tab',
            iconLight: personLightThemeIcon,
            iconDark: personDarkThemeIcon
        })

    }
}
```

Then add a new file `src/model/constants.ts` with the following contents:

```typescript
export const personDocumentType = 'myextension.Person';
export const personLightThemeIcon = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAERlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAA6ABAAMAAAABAAEAAKACAAQAAAABAAAAGKADAAQAAAABAAAAGAAAAADiNXWtAAABKElEQVRIDd2Vyw3CMBBEAxIUAWVQBxIcKIBiuNAAFVAIV2iAA2cKoAGYF9nIctaxscIBRhrZ2Z3d9T9N8++YaoIb8ShexYcjfWz40FRhraib+MwQDdpijKXci7nEsZ8YYrOoSe6LEdsLpurFtW1yudiskjXPFSaHufGciFxwqZ9cLcJNWXnjAK2Zi7NdOsKcjlwdcIlygaV+crUIl8jbwnauj5F4CY2ujw0fmiTCAndDtXC2g+HzNq8JJVau9m2Jl+CkKAYxEbfi2ZE+Nnxo4jjeqQ5Sx3QnZThTH4gNX5yc7/cx9WLavovGKJfizJG+NXKSJy+afO2raI3oE1vyqaAA+OpjRwHWtqYIMdZekdMEUy15/NBkl8WsICMbz4ng2A3+y1TOH8ALNqHxhf/P+xwAAAAASUVORK5CYII=';
export const personDarkThemeIcon = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABHNCSVQICAgIfAhkiAAAAWdJREFUSIm1ljFuwkAQRd/giFTkABS5gMsolBRcIFBwCOTGNUfgDtDRJ9yDioaCKlJ8B0dYmyLjZGLtrh0Jj7SyNPP3f894dtbinHP0aIM+yQHuYkERuQdegDnwBIw1VABH4BV4c86VQRIXMGABXADXsi7AIsjjIR4AG0NwAnIgBUa6UvWdDG4DDLoI1OQlkAFJJMtEMWUtEhXQstTksxCxR2hmRP6UCwMamppnXcnN/sx8k6FPYGlqHixLRCAx32RZ++05mOtz65y7Btsu3I1XYNvgwmZwJty1XbNINYOzL4MxgIg8/Pftjb1bLmgZFSJSiAgiMvHEJhorYhxWoAY+Gt9RnyvP3lUDY/f+ipr67fmuX258U6ACPoEd8Kxrp74KmBp8rhz7H58JetsUWCtRcwZVwLqtTTsdNM3kAHzoOtg3V0z8oCmov1FhwP0NO93U77g2Qje5cETJvHaLKzMqcAvr/a/iC+JcVEP5CMhEAAAAAElFTkSuQmCC';
```

and another file `src/model/PersonInfo.ts` in the same directory with contents 

```typescript
export type PersonInfo = {
    firstName: string;
    lastName: string;
    age: number;
    email: string;
}
```

Rename the file `src/ui/index.tsx` to `src/ui/editor.tsx` and paste the following contents into it:

```typescript {hl_lines=["16-22", "24-35", "37-43"]}
import React, { StrictMode, useCallback, useEffect, useState } from "react";
import { createRoot } from "react-dom/client";
import { getStudioProApi, IComponent, StudioProApi } from "@mendix/extensions-api";
import type { PersonInfo } from "../model/PersonInfo";

function PersonEditor(input : { studioPro: StudioProApi, documentId: string }) {
    const {studioPro,documentId} = input;
    const [person, setPerson] = useState<PersonInfo>({
        firstName: "",
        lastName: "",
        age: 0,
        email: "",
    });
    const [documentVersion, setDocumentVersion] = useState(0); // Used to trigger re-fetching the document

    useEffect(() => {
        studioPro.app.model.customBlobDocuments.addEventListener("documentsChanged", ({ documents }) => {
            if (documents.some(doc => doc.id === documentId)) {
                setDocumentVersion(v => v + 1); // Trigger re-fetch of the document
            }
        });
    }, [studioPro, documentId]);

    useEffect(() => {
        studioPro.app.model.customBlobDocuments
            .getDocumentById<PersonInfo>(documentId)
            .then(documentFromModel => {
                if (documentFromModel && !("error" in documentFromModel)) {
                    setPerson(documentFromModel.document.contents);
                }
            })
            .catch(err => {
                studioPro.ui.messageBoxes.show("error", "Error loading document", "Details: " + err?.message || err);
            });
    }, [studioPro, documentId, documentVersion]);

    const savePerson = useCallback(async () => {
        try {
            await studioPro.app.model.customBlobDocuments.updateDocumentContent<PersonInfo>(documentId, person)
        } catch (error) {
            studioPro.ui.messageBoxes.show("error", "Error saving document", "Details: " + ((error as {message?: string})?.message  || error));
        }
    }, [studioPro, documentId, person]);

    const labelStyle = { display: 'inline-block', width: '300px' };

    return (
        <div style={{ backgroundColor: 'white', padding: '20px' }}>
            <h2>Person Editor</h2>
            <div>
                <label style={labelStyle}>
                    First name:
                    <input
                        type="text"
                        value={person.firstName}
                        onChange={e => setPerson({ ...person, firstName: e.target.value })}
                    />
                </label>
            </div>
            <div>
                <label style={labelStyle}>
                    Last name:
                    <input
                        type="text"
                        value={person.lastName}
                        onChange={e => setPerson({ ...person, lastName: e.target.value })}
                    />
                </label>
            </div>
            <div>
                <label style={labelStyle}>
                    Age:
                    <input
                        type="number"
                        value={person.age}
                        onChange={e => setPerson({ ...person, age: Number(e.target.value) })}
                    />
                </label>
            </div>
            <div>
                <label style={labelStyle}>
                    Email:
                    <input
                        type="email"
                        value={person.email}
                        onChange={e => setPerson({ ...person, email: e.target.value })}
                    />
                </label>
            </div>
            <div style={{ marginTop: 8 }}>
                <button onClick={savePerson}>Save</button>
            </div>
        </div>
    );
}

export const component: IComponent = {
    async loaded(componentContext, args: { documentId: string; }) {
        const studioPro = getStudioProApi(componentContext);
        createRoot(document.getElementById("root")!).render(
            <StrictMode>
                <PersonEditor studioPro={studioPro} documentId={args.documentId} />
            </StrictMode>
        );
    }
};
```

Lastly, update the build instructions and manifest. To do so, replace the contents of `build-extension.mjs` with

```javascript {hl_lines=["16-19"]}
import * as esbuild from 'esbuild'
import {copyToAppPlugin, copyManifestPlugin, commonConfig} from "./build.helpers.mjs"
import parseArgs from "minimist"

const outDir = `dist/myextension`
const appDir = "/Users/petar.vukmirovic/Mendix/App-Guide-Test-Blob"
const extensionDirectoryName = "extensions"

const entryPoints = [
    {
        in: 'src/main/index.ts',
        out: 'main'
    }   
]

entryPoints.push({
    in: 'src/ui/editor.tsx',
    out: 'editor'
})

const args = parseArgs(process.argv.slice(2))
const buildContext = await esbuild.context({
  ...commonConfig,
  outdir: outDir,
  plugins: [copyManifestPlugin(outDir), copyToAppPlugin(appDir, outDir, extensionDirectoryName)],
  entryPoints
})

if('watch' in args) {
    await buildContext.watch();
} 
else {
    await buildContext.rebuild();
    await buildContext.dispose();
}
```

and replace the contents of `manifest.json` with

```json {hl_lines=["6"]}
{
  "mendixComponent": {
    "entryPoints": {
      "main": "main.js",
      "ui": {
        "editor": "editor.js"
      }
    }
  }
}
```

### Walking through the example code

In `src/main/index.ts`, we first register a document type. When a document type is registered, you can perform all CRUD operations on it, but it won't appear in the UI until an editor is registered for it. Note that you can optionally provide `readableTypeName`, which will be shown instead of the full type name in user-facing contexts such as logs and the Studio Pro UI. You can also customize serialization of the document contents to a string; by default the API uses `JSON.stringify` for serialization and `JSON.parse` for deserialization.

The next call to the Studio Pro API registers an editor for our document type. It registers the `editor` entry point of our extension as the editor that will be shown when a user interacts with the document in Studio Pro (for example, through the App Explorer or Find Results). This editor will be shown as a tab, but you can also register editors to be shown as modal dialogs. Finally, icons for both the light and dark themes are registered; those icons appear wherever a document icon is needed.

The first highlighted block of code in `src/ui/editor.tsx` listens for changes to documents to ensure that the most recent version of the currently active document is shown. Note that the document can be changed outside the currently open editor by calls to the custom blob document API, or by some Studio Pro operations such as undo. In the next highlighted block, we fetch the document contents whenever a new document is opened or an existing document is updated. Finally, we provide a way to save changes.

We also highlighted the changes needed in `build-extension.mjs` and `manifest.json` to ensure that the `editor` entry point is built and loaded properly.

## Extensibility Feedback

If you would like to provide us with additional feedback, you can complete a small [survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback).

Any feedback is appreciated.
