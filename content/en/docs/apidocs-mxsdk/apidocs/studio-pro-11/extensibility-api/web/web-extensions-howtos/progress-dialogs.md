 ---
title: "Show a Progress Dialog Using Web API"
linktitle: "Show a Progress Dialog"
url: /apidocs-mxsdk/apidocs/web-extensibility-api-11/progress-dialogs/
---
 
 ## Showing a Progress Dialog
 First, create a menu that you will use to open the progress dialog. 

 To show a progress dialog, you will need to call the method `studioPro.ui.dialogs.showProgressDialog(<title>, <steps>)`, where:

* `<title>` is a string which will be displayed in the title bar of the dialog.
* `<steps>` is an array of `ProgressDialogStep`, which will run in the same order provided in the array. A `ProgressDialogStep` object containing the following properties:
    * `title` — the title of the step. It is highlighted when the step is running
    * `description` —  the description of the step. It will show at the bottom of the dialog next to the progress bar
    * `action` — the action that the step will perform. It returns `Promise<true | string>`. If the step fails, string should be the reason for the failure. Otherwise, `true` will be returned.

A checkmark icon will be shown next to the step title after the step has completed successfully. But if one of the steps fails, the dialog will close and the remaining steps will not be executed.

The `showProgressDialog` method returns a `Promise<ProgressDialogResult>`. `ProgressDialogResult` is an object that contains the following properties:
* `result` - a string that is either `Success`, `Failure` or `UserCancelled`
    * `Success` is returned when all the steps have returned true
    * `Failure` is returned when one step has failed, causing the dialog to close
    * `UserCancelled` is returned when the user closes the dialog themselves and interrupts the process
* `failedStep` (optional) - it is an object of type `FailedProgressStepResult` which describes the actual step that has failed

The `FailedProgressStepResult` object contains the following properties:
* `stepTitle` - the title of the step that has failed, causing the whole process to fail
* `error` - a string which describes the error or exception that has occurred during the step execution

In this example, we will create a menu to show the modal progress dialog, and run three steps. This is done inside the `loaded` event in the main entry point (`src/main/index.ts`). For more information, see [Create a Menu Using Web API](/apidocs-mxsdk/apidocs/web-extensibility-api-11/menu-api/).

```typescript
import { ComponentContext, IComponent, ProgressDialogStep, getStudioProApi } from "@mendix/extensions-api";

export const component: IComponent = {
    async loaded(componentContext: ComponentContext) {
        const studioPro = getStudioProApi(componentContext);

        const step1: ProgressDialogStep = {
            title: "Step 1",
            description: "Executing Step 1",
            action: async () => {
                // perform action
                return true;
            }
        };

        const step2: ProgressDialogStep = {
            title: "Step 2",
            description: "Executing Step 2",
            action: async () => {
                // perform action
                return true;
            }
        };

        const step3: ProgressDialogStep = {
            title: "Step 3",
            description: "Executing Step 3",
            action: async () => {
                // perform action
                return true;
            }
        };

        // menu to call `showProgressDialog` method
        await studioPro.ui.extensionsMenu.add({
            caption: "Sample Progress Dialog",
            menuId: `myextension.start-progress-menu`,
            action: async () => {
                const steps = [step1, step2, step3];
                const progressDialogResult = await studioPro.ui.dialogs.showProgressDialog("Sample Progress Dialog", steps);

                switch (progressDialogResult.result) {
                    case "Success":
                        await studioPro.ui.messageBoxes.show("info", "Process completed successfully");
                        break;

                    case "UserCancelled":
                        await studioPro.ui.messageBoxes.show("info", "Process was cancelled by the user");
                        break;

                    case "Failure": {
                        const errorMessage = `Step '${progressDialogResult.failedStep?.stepTitle}' has failed'`;
                        const errorDetails = progressDialogResult.failedStep?.error ?? "";

                        await studioPro.ui.messageBoxes.show("error", errorMessage, errorDetails);
                        break;
                    }
                }
            }
        });
    }
};
```

It is recommended to always wrap your step action body in a `try/catch` block so that you can be in control of the error that gets returned to the user:
```typescript
const step: ProgressDialogStep = {
            title: "Step X",
            description: "Executing Step X",
            action: async () => {
                try {
                    // perform action
                    return true;
                } catch (error: Error | unknown) {
                    return error instanceof Error ? error.message : "An error occurred while performing Step X";
                }
            }
        };
```

When running, the progress dialog will look like this:
{{< figure src="/attachments/apidocs-mxsdk/apidocs/extensibility-api/web/dialogs/sample-progress-dialog.png" >}}

## Extensibility Feedback

If you would like to provide us with additional feedback, you can complete a small [survey](https://survey.alchemer.eu/s3/90801191/Extensibility-Feedback).

Any feedback is appreciated.
