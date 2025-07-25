---
title: "Test Web Services Using SoapUI"
url: /refguide/integration/testing-web-services-using-soapui/
weight: 30
description: "Describes how to create a SOAP project with SoapUI, create assertions in SoapUI, and build an automated test using SoapUI."
aliases: 
    - /howto/testing/testing-web-services-using-soapui/
---

## Introduction

You can publish your own web services in a Mendix application. These web services consist of operations. Other applications can call operations of this web service and you can return a result. This result is based on a microflow that will be executed when the web service is called. With SoapUI, you can create (automated) tests for these web services.

This how-to teaches you how to do the following:

* Create a SOAP project with SoapUI
* Create assertions in SoapUI
* Build an automated test using SoapUI

## Preparation

Before you start this how-to, make sure you have completed the following prerequisites.

* Download and install [SoapUI](https://www.soapui.org/downloads/soapui.html)
* Complete the how-to [Expose a web service](/howto/integration/expose-a-web-service/)

| Software | Version used in this how-to |
| --- | --- |
| SoapUI | 5.7.0 |

{{% alert color="warning" %}}
All images, names, and steps in this how-to are based on Studio Pro 9.24. When using other versions, the images and/or names on your screen may be different than the images or names used in this how-to.
{{% /alert %}}

## Publishing More Microflows as Web Service Operation

In [Expose a Web Service](/howto/integration/expose-a-web-service/), you published the microflow *GetCustomers* as web service operation. In this document, you will create two more microflows and publish them as web service operations.

1. Open the app you created in [How to Expose a Web Service](/howto/integration/expose-a-web-service/).
2. Create a microflow that creates a customer and returns a Boolean:

    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/microflow-return.jpg" class="no-border" >}}

3. Create a microflow that deletes a customer and returns a Boolean:

    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/microflow-delete.jpg" class="no-border" >}}

4. Publish both microflows as web service operations of the **CustomerWebService** as described in [How to Expose a Web Service](/howto/integration/expose-a-web-service/).

## Creating a New SOAP Project

In this section, you will create a new SOAP project.

1. Open **SoapUI**.
2. Press <kbd>Ctrl</kbd> + <kbd>N</kbd> to create a new SOAP project.
3. Run your app locally in Studio Pro.
4. Go to `http://localhost:8080/ws-doc/`.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/local-host.png" class="no-border" >}}
5. Enter the URL of the WSDL schema in the **Initial WSDL** field in SoapUI.
    The **Project Name** field will automatically be filled with the name of the web service, followed by *?wsdl*.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/initial-wsdl.png" class="no-border" >}}
6. Click **OK**. This will create a new SOAP project in SoapUI.

## Building a TestSuite, TestCase, and TestStep

In this section, you will build a TestSuite. A TestSuite contains one or more TestCase. Every TestCase contains one ore more TestSteps. 

1. In SoapUI, press <kbd>Ctrl</kbd> + <kbd>T</kbd> to create a new TestSuite. You can use the given name `TestSuite 1.
2. Click **OK**.
3. Select `TestSuite 1` and press <kbd>Ctrl</kbd> + <kbd>N</kbd> to create a new TestCase.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/testcase.png" class="no-border" >}}
4. Click **OK**.
5. Expand TestSuite 1 and TestCase 1.
6. Right-click **Test Steps(0)** and select **SOAP Request**.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/soap-request.png" class="no-border" >}}
7. Enter *Retrieve Customers* in the name field.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/retrieve-customers.png" class="no-border" >}}
8. Click **OK**.
9. Select **CustomerWebserviceSoap -> GetCustomers**.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/get-customers.png" class="no-border" >}}
10. Click **OK**.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/add-request.png" class="no-border" >}}
11. Click **OK**.
12. Open SOAP request **Retrieve Customers**.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/open-soap-request.png" class="no-border" >}} 
13. Change the follow value:

    ```java
    <Offset>?</Offset>
    <PageSize>?</PageSize>
    ```

    into

    ```java
    <Offset>0</Offset>
    <PageSize>10</PageSize>
    ```

14. Press <kbd>Alt</kbd> + <kbd>Enter</kbd> to submit the request. The customers you created in the previous how-to will be shown in the response. The response will look like the code below:

    ```xml
    <soap:Envelope xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://www.example.com/">
        <soap:Body>
            <tns:GetCustomersResponse>
                <Customer>
                    <Name>Jessica D. Rogers</Name>
                    <Address>2118 Collins Street</Address>
                    <ZipCode>PA 16646</ZipCode>
                    <City>Hastings</City>
                </Customer>
                <Customer>
                    <Name>Jamie S. Wentz</Name>
                    <Address>3269 Village View Drive</Address>
                    <ZipCode>MD 20872</ZipCode>
                    <City>Damascus</City>
                </Customer>
                <Customer>
                    <Name>Blake G. Race</Name>
                    <Address>4437 Sunny Glen Lane</Address>
                    <ZipCode>OH 44115</ZipCode>
                    <City>Cleveland</City>
                </Customer>
                <Customer>
                    <Name>Angela L. Dolly</Name>
                    <Address>3718 Alpha Avenue</Address>
                    <ZipCode>TX 75439</ZipCode>
                    <City>Ector</City>
                </Customer>
            </tns:GetCustomersResponse>
        </soap:Body>
    </soap:Envelope>
    ```

## Assertions

Assertions are used to validate the message received by a TestStep during execution, usually by comparing parts of the message (or the entire message) to some expected value. In this section, you will create an assertion that validates the number of customers.

1. Click **Assertions (1)**.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/assertions.png" class="no-border" >}}
    In chapter 3 step 9 the *Add SOAP Response Assertion* checkbox was checked. That is the first assertion you see in the list, *SOAP Response - VALID*.
2. Click the add assertion icon.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/assertion-icon.png" class="no-border" >}}
3. Click **Property Content**.
4. Click **XPath Match**.
5. Enter *count(//Customer)* in the XPath Expression field.
6. In step 1.4 of the previous how-to, you added some data. Enter the number of created customers in the **Expected Result** field. In this example, four customers were created.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/expected-result.png" class="no-border" >}}
7. Click **Save**.

## Extending Your Test Case

In this section, you will extend your TestCase with multiple test steps and assertions.

1. Add a new SOAP request to the existing TestCase with the following details:

    | Field | Value |
    | --- | --- |
    | Specify name for step | Create Customer |
    | Select operation to invoke for request | CustomerWebServiceSoap -> CreateCustomer |

2. Change the request into the following value:

    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:exam="http://www.example.com/">
       <soapenv:Header/>
       <soapenv:Body>
          <exam:CreateCustomer>
             <Name>Donald A. Hylton</Name>
             <Address>784 Better Street</Address>
             <City>Kansas City</City>
             <ZipCode>KS 66102</ZipCode>
          </exam:CreateCustomer>
       </soapenv:Body>
    </soapenv:Envelope>
    ```

3. Add a **Contains Assertion** and enter *true* in the content field.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/contains-assertion.png" class="no-border" >}} 
4. Right-click TestStep **Retrieve Customers**.
5. Select **Clone TestStep**.
6. Change **TestStep Name** to *Retrieve Customers 2*.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/retrieve-customers-2.png" class="no-border" >}}
7. Click **OK**.
8. Open TestStep **Retrieve Customers 2**.
9. Click **Assertions (2)**.
10. Open **XPath Match - Unknown**.
11. Change **Expected result** to *1 + [the number you entered in step 5.6]*.
12. Click **Save**.
13. Add a new SOAP request to the existing TestCase with the following details:
    * In the **Specify name for step**, enter *Delete Customer*.
    * In the **Select operation to invoke for request** field, enter *CustomerWebServiceSoap -> DeleteCustomer*.

14. Change the request into the following value:

    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:exam="http://www.example.com/">
       <soapenv:Header/>
       <soapenv:Body>
          <exam:DeleteCustomer>
             <Name>Donald A. Hylton</Name>
          </exam:DeleteCustomer>
       </soapenv:Body>
    </soapenv:Envelope>
    ```

## Running the TestCase

In this section, you will learn how to run the TestCase you created in previous chapters.

1. Open TestCase **TestCase 1**.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/testcase-2.png" class="no-border" >}}
2. Click the run this TestCase icon.
    {{< figure src="/attachments/refguide/modeling/integration/web-services/testing-web-services-using-soapui/testcase-icon.png" class="no-border" >}}

Congratulations! You have created your first automated test with SoapUI.

## Read More

* [Finding the Root Cause of Runtime Errors](/howto/monitoring-troubleshooting/finding-the-root-cause-of-runtime-errors/)
* [Clearing Warning Messages in Mendix](/howto/monitoring-troubleshooting/clear-warning-messages/)
* [Testing web services using SoapUI](/howto/testing/testing-web-services-using-soapui/)
* [Monitoring Mendix using JMX](/howto/monitoring-troubleshooting/monitoring-mendix-using-jmx/)
* [Debugging Java actions remotely](/howto/monitoring-troubleshooting/debug-java-actions-remotely/)
* [Log Levels](/howto/monitoring-troubleshooting/log-levels/)
* [Debugging Java Actions](/howto/monitoring-troubleshooting/debug-java-actions/)
* [Debugging Microflows and Nanoflows](/refguide/debug-microflows-and-nanoflows/)
* [Debugging Microflows Remotely](/refguide/debug-microflows-remotely/)

Learn more about this topic using the following helpful links:

* [10 tips for the SoapUI beginner](https://www.soapui.org/getting-started/10-tips-for-the-soapui-beginner.html)
