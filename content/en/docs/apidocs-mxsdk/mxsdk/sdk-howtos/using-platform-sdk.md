---
title: "Use the Platform SDK"
url: /apidocs-mxsdk/mxsdk/using-platform-sdk/
weight: 12
---

## Introduction 

This how-to provides guidance on using the Platform SDK to do the following:

* [Create a new app](#creating-app)
* [Open an existing app](#opening-existing-app)
* [Get information about the repository of an app](#getting)
* [Delete an app](#deleting)
* [Create a temporary working copy](#creating-temp)
* [Open the working copy model](#opening-working-copy)
* [Commit a temporary working copy](#committing)
* [Change the Platform SDK configurations](#changing)

## Platform Client

The entry point for the Mendix Platform SDK is `MendixPlatformClient`. In most cases, you will need to instantiate a new object from this class:

```ts
import { MendixPlatformClient } from "mendixplatformsdk";

const client = new MendixPlatformClient();
```

## Creating a New App {#creating-app}

The platform client allows you to create a new Mendix app by simply passing the app name:

```ts
const app = await client.createNewApp("My new App");

console.log(`App created with ID: ${app.appId}`);
```

You can pass the following options to `createNewApp`:

| Name | Description | 
|--- | --- |
| `repositoryType` | The type of repository to be used. Possible values: `svn` and `git`. |
| `summary` | A short description of the app. |
| `image` | The Base64-encoded data of the app image (height and width between 200px and 400px, with a maximum size of 5 MB). |
| `templateDownloadURL` | The URL of the download location of the app template package file (*.mpk*). If the template package is private, this URL must be authenticated with a signature. |
| `templateId` | The UUID of the app template on which the app should be based. |

If both `templateDownloadURL` and `templateId` are left blank, the app will be created using the standard blank app template in the latest Mendix version.

Here is an example for creating a Mendix app based on version 2.1.0 of the [Blank GenAI App](https://marketplace.mendix.com/link/component/227934) template:

```ts
const app = await client.createNewApp("My GenAI App", {
    templateId: "ba6ca01b-e2a4-45fa-870d-9e28b6acb845"
});
```

## Opening an Existing App {#opening-existing-app}

The platform client allows you to open an existing app using the app ID:

```ts
const app = client.getApp("33118fbf-7053-482a-8aff-7bf1c626a6d9");
```

{{% alert color="info" %}}
You can get the **App ID** (represented as **Project ID**) in the app's [Settings](/developerportal/collaborate/general-settings/) page after opening your app in **Apps**.
{{% /alert %}}

## Getting Information About the Repository of the App {#getting}

From the app object, you can get some information about its repository (such as the repository type, URL, and default branch name):

```ts
const repository = app.getRepository();
    
const repositoryInfo = await repository.getInfo();
console.log("Repository Info: ", repositoryInfo);

const commitMessages = (await repository.getBranchCommits("main")).items.map(commit => commit.message);
console.log("Commit messages: ", commitMessages);
```

## Deleting an App {#deleting}

The app object allows you to delete the corresponding Mendix app. 

```ts
await app.delete();
```

{{% alert color="warning" %}}
All resources of this app will be deleted permanently!
{{% /alert %}}

## Creating a Temporary Working Copy {#creating-temp}

To change your app, you need to create a temporary working copy of a particular Team Server branch, make the changes there, and then submit that working copy to Team Server:

```ts
const workingCopy = await app.createTemporaryWorkingCopy("main");

console.log(`Working ID: ${workingCopy.workingCopyId}`);
```

{{% alert color="warning" %}}
Working copy creation a resource intensive process, consider reusing previously created ones by invoking `app.getOnlineWorkingCopy(workingCopyId)`. All working copies are automatically deleted after 24 hours.
{{% /alert %}}

You can pass the following options to `createTemporaryWorkingCopy`:

| Name | Description |
|--- | --- |
| `commitId` | The ID of the commit on which the working copy should be based. If not passed, the working copy is created from the last commit in the specified branch. |

## Opening the Working Copy Model {#opening-working-copy}

After creating the working copy, you can load the model to make changes:

```ts
const model = await workingCopy.openModel();
```

## Committing a Temporary Working Copy {#committing}

After making changes, you need to commit the changes back to Team Server. Make sure to call `await model.flushChanges()` when committing right after making changes, as this makes sure that the SDK has been able to send the changes:

```ts
await model.flushChanges();
await workingCopy.commitToRepository();
```

You can pass the following options to `commitToRepository`:

| Name | Description |
|--- | --- |
| `branchName` | You can specify a branch other than the working copy base branch. In that case, set `force` to `true`. |
| `commitMessage` | Specify a custom commit message instead of the default message ("Imported model changes from online working copy"). |
| `targetCommitId` | This commit ID will be set to the working copy base commit ID if not specified. |
| `force` | Set to `true` to commit to a branch that is different from the working copy's base branch. |

## Changing the Platform SDK Configurations {#changing}

By default, the Platform SDK reads your personal access token from the environment variable (for more details, see [How to Set Up your Personal Access Token](/apidocs-mxsdk/mxsdk/set-up-your-pat/)). However, you can change this configuration. For example, you can load it from a file, as in this example:

```ts
setPlatformConfig({
    mendixToken: fs.readFileSync("mendix-token.txt", {encoding: "utf8"})
});
```

By default, the Platform SDK prints some logs to the console. You can customize the logging experience using the following APIs:

```ts
disableLogger()          // Disables all logging
enableLogger()           // Enable logging through the console
setLogger(customLogger); // Override the logger object
```

The custom logger object should have the following methods:

```ts
info(message?: string, ...optionalParams: any[]): any;
warn(message?: string, ...optionalParams: any[]): any;
error(message?: string, ...optionalParams: any[]): any;
```
