---
title: "Configure the External Database Connector for Databricks"
linktitle: "External Database Connector"
url: /appstore/modules/databricks/external-database-connector/
description: "Describes the steps required to use the Mendix External Database connector with Databricks."
weight: 10
---

## Introduction

The [External Database connector](/appstore/modules/external-database-connector/) allows you to connect to databases and select data to use in your app. You can use it to directly test connections and queries during configuration in Studio Pro (design time). For Mendix apps that use Databricks as their database, the External Database connector is the recommended integration option for Mendix 10.19.0 and up.

This how-to describes the steps required to enable your app to use the External Database connector with Databricks, and to model several common use cases.

## Configuring the Connection Between Your Mendix App and Databricks

To connect your Mendix application to Databricks with the External Database connector, follow these steps:

1. [Install the External Database connector](/appstore/modules/external-database-connector/#installation). If you are using Studio Pro 10.19, please make sure to use the latest version 5.2.0 [External Database Connector](https://marketplace.mendix.com/link/component/219862).
2. Download the latest [JDBC driver](https://www.databricks.com/spark/jdbc-drivers-archive) that Databricks provides and put the .jar file in to the userlib of your Mendix project.
3. Run the [Connect to Database wizard](/appstore/modules/external-database-connector/#configuration) and select **Other** as the database type.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/DatabricksConfig.png" >}}

4. Provide a name for the database connection document.
5. As a username use "token" and as your password use the personal access token (PAT) you have created in your Databricks account.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/PAT.png" >}}
   
7. In your Databricks account find the JDBC URL related to the SQL Warehouse or Cluster you are using and copy it into the JDBC URL field and add UID=token;PWD=<PAT> where <PAT> should be replaced with your actual PAT.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/JDBC_URL.png" >}}
  
9. Click **Test Connection** to verify the connection details, and then click **Save**.

Your Mendix app now connects to Databricks with the provided connection details. When the connection is successful, you can see your Databricks tables in your Mendix app.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/Database_structure.png" >}}

You can now configure the queries that you need to run on your Databricks database. The following sections of this document provide examples of some common queries. For general information about creating queries, see [External Database Connector: Querying a Database](/appstore/modules/external-database-connector/#query-database) and [External Database Connector: Using Query Response](/appstore/modules/external-database-connector/#use-query-response).

## Configuring a Basic Query

This section provides an example of a query that determines the average minimum, maximum, and average temperature for a given postal code for the next 10 calendar days, based on the climate data in the **CLIMATOLOGY_DAY** view.

To execute and test the query in Studio Pro, follow these steps:

1. In your Mendix app, in the **App Explorer**, find and open the external connection document that you created with the Connect to Database wizard.
2. In the **Name** field, enter a name for your query, for example, *ClimateForecastNext10DaysQuery*.
3. Enter the following **SQL Query**:

    ```sql
    select POSTAL_CODE                                                   as "PostalCode"
        , COUNTRY                                                        as "Country"
        , doy_std                                                        as "DayOfYearClimate"
        , dayofyear(CURRENT_DATE)                                        as "DayOfYearToday"
        , current_date + doy_std - dayofyear(CURRENT_DATE)               as "ClimateDate"
        , round((AVG_OF__DAILY_AVG_TEMPERATURE_AIR_F - 32) * (5 / 9), 1) as "AvgAvgTempCelsius"
        , round((AVG_OF__DAILY_MIN_TEMPERATURE_AIR_F - 32) * (5 / 9), 1) as "AvgMinTempCelsius"
        , round((AVG_OF__DAILY_MAX_TEMPERATURE_AIR_F - 32) * (5 / 9), 1) as "AvgMaxTempCelsius"
    from  CLIMATOLOGY_DAY
    where postal_code = {postal_code} 
    and   ((doy_std + 365) - dayofyear (current_date)) % 365 <=10
    order by doy_std asc
    limit 10
    ```

4. Click **Run Query**.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/external-database-connector/sample-snowflake-query-basic.png" >}}

    {{% alert color="info" %}}As shown in the above example, if your input parameters do no exactly match what the database needs, or if the output of the query does not match what you need in Mendix, you can cast or transform your data in your query. You can also use column aliases to help generate entities with the required names.{{% /alert %}}

5. Verify that the results are correct, and then generate the required entity to collect the data in your Mendix application. For more information, see [External Database Connector: Creating an Entity from the Response](/appstore/modules/external-database-connector/#create-entity).
6. Create a page with a gallery widget to show the results. Above the gallery widget you need form to allow the user to specify a postalcode. For this you need to create an NPE, e.g. name Filter, with one field, postalcode. The gallery widget will get its data from the Microflow in the next step. You can refresh this widget by using a nanoflow to trigger refresh of the entity shown in the Gallery widget.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/external-database-connector/sample-snowflake-gallery-page.png" >}}

7. Create a microflow that will run the query by doing the following steps:
    1. In the **App Explorer**, right-click on the name of your module, and then click **Add microflow**.
    2. Enter a name for your microflow, for example, *ACT_RetrieveWeatherData*, and then click **OK**.
    3. Set the Filter NPE as input parameter for your microflow.
    4. In your **Toolbox**, find the **Query External Database** activity and drag it onto the work area of your microflow.
    5. Position the **Query External Database** activity between the start and end event of your microflow.
    6. Double-click the **Query External Database** microflow activity to configure the required parameters.
    7. In the **Database** section, select your Snowflake database.
    8. In the **Query** list, select the query name that you entered in step 2.
    9. In the **Parameters** section, add the following parameter:
        * **Name** - *postal_code*
        * **Type** - **String**
        * **Value** - *$Filter/PostalCode*
    10. In the **Output** section, provide the following values:
        * **Return type** - **List of *{your module name}*.CLIMATOLOGY_FORECAST**
        * **Use return value** - set to **Yes**
        * **List name** - enter *CLIMATOLOGY_DAY*
    11. Click **OK**.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/external-database-connector/sample-snowflake-query-basic-flow.png" >}}

8. Specify the microflow as the datasource for the gallery widget.
9. Run the page, provide a valid postalcode, and validate the result of the page.

7. Configure a method for triggering the **ACT_RetrieveSentiment** microflow. For example, you can trigger a microflow by associating it with a custom button on a page in your app. For an example of how this can be implemented, see [Creating a Custom Save Button with a Microflow](/refguide/creating-a-custom-save-button/).
8. Run the **ACT_RetrieveSentiment** microflow and verify the results.
