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

1. [Install the External Database connector](/appstore/modules/external-database-connector/#installation). Please make sure to use the latest version 5.2.0 or higher [External Database Connector](https://marketplace.mendix.com/link/component/219862). As an additional resource on how to use bring your own JDBC driver with the external database connector you can view [this documentation](https://docs.mendix.com/appstore/modules/external-database-connector/#byod) although this explanation is more generic than what will follow in this documentation.
2. Download the latest [JDBC driver](https://www.databricks.com/spark/jdbc-drivers-archive) that Databricks provides and put the .jar file in to the userlib of your Mendix project.
3. Run the [Connect to Database wizard](/appstore/modules/external-database-connector/#configuration) and select **Other** as the database type.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/DatabricksConfig.png" >}}

4. Provide a name for the database connection document.
5. As a username use "token" and as your password use the personal access token (PAT) you have created in your Databricks account.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/PAT.png" >}}
   
7. In your Databricks account find the JDBC URL related to the SQL Warehouse or Cluster you are using and copy it into the JDBC URL field and add UID=token;PWD=**PAT** where **PAT** should be replaced with your actual PAT.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/JDBC_URL.png" >}}
  
9. Click **Test Connection** to verify the connection details, and then click **Save**.

Your Mendix app now connects to Databricks with the provided connection details. When the connection is successful, you can see your Databricks tables in your Mendix app.

{{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/Database_structure.png" >}}

You can now configure the queries that you need to run on your Databricks database. The following section of this document provides an example of a common query. For general information about creating queries, see [External Database Connector: Querying a Database](/appstore/modules/external-database-connector/#query-database) and [External Database Connector: Using Query Response](/appstore/modules/external-database-connector/#use-query-response).

## Configuring a Basic Query

In this section we will show you a very basic example of how to use the database connector to execute a query on your Databricks data. To do this you can create some data in your Databricks workspace. Navigate to the SQL Editor in your workspace and run the folowing sql command:

```sql
CREATE TABLE customerData (
    name varchar(64),
    address varchar(64),
    postal_code varchar(6),
    gender varchar(64)
); 

INSERT INTO customerData (name, address, postal_code, gender) VALUES 
    ('Henk de Vries', 'Klaprooslaan 5, Bloemenstad', '1234AB', 'Male'),
    ('Sanne Verbeek', 'Molendam 8, Waterveen', '2345CD', 'Female'),
    ('Jan-Willem Bos', 'Zonstraat 22, Zomerhoven', '3456EF', 'Male'),
    ('Marieke de Groot', 'Windmolenweg 33, Korenveld', '4567GH', 'Female'),
    ('Bert van Dijk', 'Vlinderplein 15, Lenteveen', '5678JK', 'Male'),
    ('Lotte van Dam', 'Regenboogpad 10, Kleurenburg', '6789LM', 'Female'),
    ('Koen Smits', 'Eikenlaan 2, Bosrijk', '7890NO', 'Male'),
    ('Emma Visser', 'Druppelweg 45, Regenstad', '8901QP', 'Female'),
    ('Thomas Mulder', 'Sterrenhof 7, Hemelrijk', '9012RS', 'Male'),
    ('Sophie Jansen', 'Kersenstraat 12, Fruitdorp', '0123TU', 'Female');
```

This should have created a table with some entries that can now be queried from your Mendix application.

1. In your Mendix app, in the **App Explorer**, find and open the external connection document that you created with the Connect to Database wizard.
2. In the **Name** field, enter a name for your query, for example, *CustomerData_Get*.
3. Enter the following **SQL Query**:

    ```sql
    SELECT * FROM customerData;
    ```

4. Click **Run Query**.

    {{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/Query.png" >}}

5. Verify that the results are correct, and then generate the required entity to collect the data in your Mendix application. For more information, see [External Database Connector: Creating an Entity from the Response](/appstore/modules/external-database-connector/#create-entity).
6. Create a microflow that will run the query by doing the following steps:
    1. In the **App Explorer**, right-click on the name of your module, and then click **Add microflow**.
    2. Enter a name for your microflow, for example, *ACT_Customerdata_Get*, and then click **OK**.
    3. In your **Toolbox**, find the **Query External Database** activity and drag it onto the work area of your microflow.
    4. Position the **Query External Database** activity between the start and end event of your microflow.
    5. Double-click the **Query External Database** microflow activity to configure the required parameters.
    6. In the **Database** section, select your Databricks database.
    7. In the **Query** list, select the query name that you entered in step 2.
    10. In the **Output** section, provide the following values:
        * **Return type** - **List of *{your module name}*.customerdata**
        * **Use return value** - set to **Yes**
        * **List name** - enter *Customerdata_list*
    11. Click **OK**.

    {{< figure src="static/attachments/appstore/platform-supported-content/modules/databricks/JA_Query.png" >}}

7. Use the microflow behind a microflow button and use the debugger to test if the objects are properly returned (or build a quick interface).
