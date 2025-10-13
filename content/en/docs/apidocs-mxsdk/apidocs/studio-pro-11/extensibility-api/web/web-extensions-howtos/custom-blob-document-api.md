---
title: "Register New Document Types with Corresponding Editor"
linktitle: "Introduce New Document Types"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/custom-blob-document-api/
---

## Introduction

This how-to describes how to introduce a new document type and provide a custom editor to allow users to edit new documents of the introduced type.

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* [Get Started with the Web Extensibility API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/).

## Custom Document Model

Studio Pro provides you with possibility to extend its metamodel by introducing your own document types. These new documents are allowed to store arbitrary data,
which must be serializable as strings. If an editor, which is a user-defined UI component, is registered for a document, new document will appear in UI as any other
built-in document type (e.g., constants, Java Actions or pages). In particular, it will appear in New Document and Find Advanced dialogs, context menus for adding
new documents, App explorer and other UI elements that show Studio Pro documents. Custom editors can be registered as either tabs or modal dialogs.

## Registering a New Document Type

To start with registering a new document type, generate a new extension called `myextension`, as described in [getting started guide](/apidocs-mxsdk/apidocs/web-extensibility-api-11/getting-started/).
We will explain which files of the generated extension to edit and then explain what the edits achieve.

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

Then add a new file `src/model/contants.ts` with contents:

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

Then rename the file `src/ui/index.tsx` to `src/ui/editor.tsx` and paste the following contents into it:

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
    }, [studioPro]);

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

Lastly, we need to update the build instructions and manifest. To do so, replace the contents of `build-extension.mjs` with 

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

and the contents of `manifest.json` with 

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

In `src/main/index.ts`, we are first registering a new document type. When a new document type is registered, you can perform all
CRUD operations on this document type, but it will not yet be shown in the UI since no editor for it has yet been registered. Note
that you can optionally provide `readableTypeName` which will be shown instead of full type name in every user-facing context such
as in logs and in the Studio Pro UI. You can optionally customize the (de)serialization of the document contents to string, which
defaults to `JSON.stringify` and `JSON.parse`. The next call to `studioPro` API registers editor for our document type. It registers
`editor` entry point of our extension as the editor that will be shown when user interacts with the document in StudioPro (for example 
through App Explorer or Find Results). This editor will be shown as a tab, but you can also register editors to be shown as 
modal dialogs. Lastly, icons for both light and dark theme are registered. Those icons will be shown in all UI elements where 
document icon is needed.

The first highlighted block of code in `src/ui/editor.tsx` listens for changes in documents to make sure that the last version of currently 
active document is shown. Note that the document can be changed outside currently open editor by calls to custom blob document API,
as well as by some Studio Pro operations such as undoing changes. In the next highlighted block, we fetch the document contents 
whenever new document is open or updated. Lastly, we make sure that the current changes can be saved.

We also highlighted changes that need to be made to `build-extension.mjs` and `manifest.json` to make sure that `editor` entry point
is properly built and loaded.

## Extensibility Feedback

If you would like to provide us with additional feedback, you can complete a small [survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback).

Any feedback is appreciated.