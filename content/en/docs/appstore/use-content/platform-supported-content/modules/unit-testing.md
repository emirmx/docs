---
title: "Unit Testing"
url: /appstore/modules/unit-testing/
description: "Describes the configuration and usage of the Unit Testing module, which is available in the Mendix Marketplace."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

The [Unit Testing](https://marketplace.mendix.com/link/component/390/) module enables developers to implement, run and manage unit tests within Mendix applications. Test your microflows, Java actions, and other logic by implementing test microflows and JUnit (Java) tests. Use the module to automate testing, detect issues early, and improve code quality. 

### Limitations

* The Unit Testing module does not prevent multiple users to run unit tests in parallel. Ensure that only a single unit test run is executed at a given time to prevent inconsistent results.

### Dependencies
#### Marketplace dependencies

* [Accordion](https://marketplace.mendix.com/link/component/117895/)
* [Atlas Core](https://marketplace.mendix.com/link/component/117187/)
* [Atlas Web Content](https://marketplace.mendix.com/link/component/117183/)
* [Data Widgets](https://marketplace.mendix.com/link/component/116540/)

{{% alert color="info" %}}
For module version 9.1.0, the [Community Commons](/appstore/modules/community-commons-function-library/) module is required as a dependency. This dependency has been removed in module version 9.2.0.
{{% /alert %}}

{{% alert color="info" %}}
For module versions below 9.1.0, the [Object Handling](/appstore/modules/object-handling/) module is required as a dependency.
{{% /alert %}}

#### Java dependencies

The following Java dependencies are shipped with the module. For Mendix versions above 10.3.0, these dependencies are obtained and managed using [Managed Dependency]()
* *junit-4.13.1.jar*
* *commons-io-2.11.0.jar*
* *commons-lang3-3.12.0.jar*
* *httpclient5-5.0.3.jar*
* *httpcore5-5.0.3.jar*
* *hamcrest-2.2.jar*

## Installation

1. Follow the instructions in [How to Use Marketplace Content](/appstore/use-content/) to import the Unit Testing module into your app.

## Configuration
1. Add the module role **TestRunner** from the module to all app user roles that should be able to run and view unit tests.
1. In the runtime settings of your app, configure the **Startup** microflow for the after startup property. If there is already an after startup microflow set, add the **Startup** microflow as an action in the existing microflow.
1. Add the **UnitTestOverview** microflow to your navigation structure, or include the **UnitTestOverview** snippet on a custom page.
1. The following steps are optional:
    * To run unit tests in a deployed environment, set the **UnitTesting.Enabled** constant to true.
        * By default, the Unit Testing module is only enabled for local development environments.
        * We recommend to keep this constant set to false for production environments.
    * To exclude JUnit tests, set the **UnitTesting.FindJUnitTests** constant to false. JUnit tests are included by default. 
        * Set the **UnitTesting.RemoteApiEnabled** constant to true and provide a password for **UnitTesting.RemoteApiPassword**.
        * When hosting in a cloud node or on-premises, configure the request handler for the **unittests/** path.

## Usage

### Implementation
#### Creating Microflow Unit Tests

To create a new microflow test in a module, add a microflow with a name that starts with **TEST_** or **UT_** (case insensitive). Configure the test microflow as follows:

{{< figure src="/attachments/appstore/platform-supported-content/modules/unit-testing/unit-test-example.png" alt="architecture-overview-diagram" >}}

1. The test microflow should have either no return type, a boolean return type, or a string return type.
    * Test microflows without a return type are considered to be successful as long as no assertions fail and no exceptions are thrown.
    * For string return type, a non-empty string is interpreted as an error message and will fail the test. 
1. The test microflow should have either no input parameters, or a single input parameter with the exact name **UnitTestContext**. 
    * When using the UnitTestContext parameter, use data type Object and **UnitTesting.UnitTestContext** as the entity. 
    * During test execution, a UnitTestContext object will be passed to this parameter. You can use this object to get access to the name of the test and any assertion results. This can be useful when you have multiple assertions in a single test and want to use the outcome of previous assertions in your test logic.
1. Use the **Assert using expression** microflow action to add assertions. The assertion action verifies if the expression result matches the expected value, and has the following parameters:
    * **Name**: The name of the assertion, used in the timeline
    * **Expression**: The expression you want to verify, which should result in a boolean value.
    * **FailureMessage**: The message to show in the timeline in case the assertion fails. We recommend to include the actual and expected value in this message.
    * **StopOnFailure**: When set to true, the test execution will stop immediately if this assertion fails. When set to false, the unit test microflow execution will continue and you can still verify the result of other assertions. Note that a failed assertion will always result in a failed test. 
1. Use the **Report step**  microflow action to log key steps of the test execution. Any reported steps will be shown in the timeline.
1. It is possible to create **Setup** and **Teardown** microflows that are invoked once before and/or after each test run, regardless of whether the test run consists of one or multiple unit tests. 
    * When you add a microflow with the exact name **Setup** to your module, this will be invoked before each test run.
    * When you add a microflow with the exact name **TearDown** to your module, this will be invoked after each test run.

#### Creating Java Unit Tests (with JUnit)

The Java unit test runner is driven by [JUnit](https://junit.org/junit5/) res a general understanding of JUnit version 4. A JUnit test method is run if it exists somewhere in the module namespace (that is, it is part of a Java class that lives somewhere in the javasource/yourmodulename folder). A Java function is recognized as a test if it is public, non-static, parameter-less, and annotated with the `org.junit.Test` annotation. Multiple tests can exists in a single class, but JUnit does not guarantee the execution order of the tests.

{{% alert color="info" %}}
For examples of Java unit tests, see javasource/unittesting/JUnitExample1.java and javasource/unittesting/JUnitExample2.java in your app directory.
{{% /alert %}}

Configure a Java test class as follows:
* Use the `org.junit.Test` annotation for each test method.
* It is possible to create methods that are invoked before and after running a test
    * When adding the `org.junit.Before` annotation to a method, this will be invoked before each test.
    * When adding the `org.junit.After` annotation to a method, this will be invoked after each test.
    * When adding the `org.junit.BeforeClass` annotation to a method, this will be invoked once before running all tests inside the class
    * When adding the `org.junit.AfterClass` annotation to a method, this will be invoked once after running all tests inside the class
* It is possible to use the `TestManager.instance().reportStep` method to track key steps of the test execution. The last step successfully reached in a unit test is reported back in the test result. This makes it easier to inspect where things go wrong.
* You can base unit test classes on the **AbstractUnitTest** class. This class provides some time measurement functions and direct access to the **reportStep** function.

### Navigation
To view and run unit tests, open the unit testing overview from your navigation. The overview consists of two parts:
* On the left side, you can find a list of all modules with unit tests.
    * Expand a module to view the unit tests for that module
    * Use the buttons on top to:
        * Run or reset all tests
        * Filter the unit tests based on their status
* On the right side, you can find the details for a selected module or unit test, including their last result, if available.

{{< figure src="/attachments/appstore/platform-supported-content/modules/unit-testing/unit-test-overview.png" alt="Unit Test Overview" >}}

### Execution
You can run unit tests on three levels:
* Click **Run all tests** to run all the unit tests for all test suites
* Click **Run tests** to run all the unit tests for a single test suite
* Click **Run test** to run a single unit test

After a unit test has been run, additional details about the test result will appear:
* For microflow tests: the test result and a timeline of test execution activities, such as assertions, reported steps and exception details. 
* For JUnit tests: the test result and last reported step. In case of a failure, you will see the exception message and stack trace.

You can reset the test result(s) on the same three levels using the Reset buttons.

By default, all changes that are made when running a microflow test (committing new or changed objects) will be rolled back at the end of the test run. You can change this behaviour per test suite using the **Rollback microflow tests after execution** checkbox, which is visible after selecting a test suite. Note that the option only affects tests within the selected test suite.

{{% alert color="alert" %}}
If the checkbox is cleared, all changes made by the microflows that you test are saved to the database. This can result in populating the database with unwanted test data. As a best practice, do not clear the checkbox unless it is required by your specific use case.
{{% /alert %}}

### Using the Remote API
The module includes a remote API for running unit tests. The API supports starting a new unit test run and verifying the status. 

{{% alert color="info" %}}
By default, the remote API is disabled. You can enable the API by setting the **UnitTesting.RemoteApiEnabled** constant to true. 
{{% /alert %}}

#### Starting a new unit test run
A new test run through the remote API can be started by using the `unittests/start` endpoint. For the password, provide the value that is configured in the **UnitTesting.RemoteApiPassword** constant.

{{% alert color="info" %}}
To ensure the security of the API, it is required to configure a password. If no value is configured for `UnitTesting.RemoteApiPassword`, the remote API will NOT be enabled, even if the `RemoteApiEnabled` constant is set to true.
{{% /alert %}}

Here is an example:

```json
POST http://localhost:8080/unittests/start
{
	"password" : "<password>"
}
```

You will receive a 204 NO CONTENT response if the test run was started successfully. 

#### Verifying the status of a unit test run

For a unit test run that was initiated using the remote API, you can poll for the status of the test run by sending a request to `unittests/status`. 

Here is an example:

```json
POST http://localhost:8080/unittests/status
{
	"password" : "<password>"
}
```

This is an example response:

```json
{
    "failures": 1,
    "tests": 10,
    "runtime": -1,
    "failed_tests": [{
        "error": "This exception is doopery nice",
        "name": "MyFirstModule.Test_ThisTestIsSupposedToFailToCheckExceptionRendering",
        "step": "Starting microflow test 'MyFirstModule.Test_ThisTestIsSupposedToFailToCheckExceptionRendering'"
    }],
    "completed": false
}
```

The completed flag will be `false` as long as the test run is not finished. The `runtime` flag will return the total runtime of the test run in milliseconds after the test run has finished.

## Read More

* [How to Test Microflows Using the UnitTesting Module](/refguide/testing-microflows-with-unit-testing-module/)
