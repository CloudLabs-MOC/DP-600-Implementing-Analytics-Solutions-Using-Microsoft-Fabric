# Module 02: Get started with lakehouses in Microsoft Fabric

## Lab scenario

A data lakehouse is a common analytical data store for cloud-scale analytics solutions. One of the core tasks of a data engineer is to implement and manage the ingestion of data from multiple operational data sources into the lakehouse. In Microsoft Fabric, you can implement *extract, transform, and load* (ETL) or *extract, load, and transform* (ELT) solutions for data ingestion through the creation of *pipelines*.

Fabric also supports Apache Spark, enabling you to write and run code to process data at scale. By combining the pipeline and Spark capabilities in Fabric, you can implement complex data ingestion logic that copies data from external sources into the OneLake storage on which the lakehouse is based and then uses Spark code to perform custom data transformations before loading it into tables for analysis.

## Lab objectives
In this lab, you will perform:

- Create a lakehouse
- Explore shortcuts
- Create a pipeline
- Create a notebook
- Use SQL to query tables
- Create a visual query
- Create a report

## Estimated timing: 60 minutes

## Architecture Diagram

![](Images/Arch-04.png)

### Task 1: Create a Lakehouse

Large-scale data analytics solutions have traditionally been built around a *data warehouse*, in which data is stored in relational tables and queried using SQL. The growth in "big data" (characterized by high *volumes*, *variety*, and *velocity* of new data assets) together with the availability of low-cost storage and cloud-scale distributed computing technologies has led to an alternative approach to analytical data storage; the *data lake*. In a data lake, data is stored as files without imposing a fixed schema for storage. Increasingly, data engineers and analysts seek to benefit from the best features of both of these approaches by combining them in a *data lakehouse*; in which data is stored in files in a data lake and a relational schema is applied to them as a metadata layer so that they can be queried using traditional SQL semantics.

In Microsoft Fabric, a lakehouse provides highly scalable file storage in a *OneLake* store (built on Azure Data Lake Store Gen2) with a metastore for relational objects such as tables and views based on the open source *Delta Lake* table format. Delta Lake enables you to define a schema of tables in your lakehouse that you can query using SQL.


Now that you have created a workspace in the previous step, it's time to switch to the *Data engineering* experience in the portal and create a data lakehouse into which you will ingest data.

1. At the bottom left of the Power BI portal, select the **Power BI (1)** icon and switch to the **Data Engineering (2)** experience.

   ![02](./Images/dp2-1.1.png)
   
2. In the **Data engineering** home page, click on **Lakehouse** to create a new lakehouse.

    - **Name:** Enter **Lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)**

    - Click on **Create (2)**.

      ![02](./Images/dp-5.png)
  
      ![02](./Images/dp-6.png)

        >**Note:** After a minute or so, a new lakehouse with no **Tables** or **Files** will be created.

3. On the **Lakehouse<inject key="DeploymentID" enableCopy="false"/>** tab in the pane on the left, in the **...** menu for the **Files (1)** node, select **New subfolder (2)** and create a subfolder named **new_data**

   ![02](./Images/01/01.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

<validation step="e57fa5a2-8d9b-4b74-822b-7f3775d3fa13" />

### Task 2: Explore shortcuts

In many scenarios, the data you need to work within your lakehouse may be stored in some other location. While there are many ways to ingest data into the OneLake storage for your lakehouse, another option is to instead create a *shortcut*. Shortcuts enable you to include externally sourced data in your analytics solution without the overhead and risk of data inconsistency associated with copying it.

1. In the **... (1)** menu for the **Files** folder, select **New shortcut (2)**.

   ![02](./Images/dp2-2.png)

3. View the available data source types for shortcuts. Then close the **New shortcut** dialog box without creating a shortcut.

   ![02](./Images/dp2-3.png)
   
### Task 3: Create a pipeline

A simple way to ingest data is to use a **Copy data** activity in a pipeline to extract the data from a source and copy it to a file in the lakehouse.

1. Select **Data Engineering (1)**, and select **Data Engineering (2)** again.

   ![03](./Images/dp2-4.4.png)

1. Navigate to next page on the **Recommended items to create.**

   ![03](./Images/dp2-5.png)
  
3. Now, On the **Synapse Data Engineering**, select **Data pipeline**.

    ![03](./Images/dp2-6.png)

4. Create a new data pipeline named **Ingest Sales Data Pipeline (1)** and click on **Create (2)**. 
   
   ![03](./Images/01/Pg3-TCreatePipeline-S1.1.png)
   
5. If the **Copy data** wizard doesn't open automatically, select **Copy data assistant** in the pipeline editor page.

   ![03](./Images/dp2-copy.png)

6. In the **Copy Data** wizard, on the **Choose a data source** page, search and then select **Http**.

   ![Screenshot of the Choose data source page.](./Images/dp-600-lab03-2.png)

7. One the **Connect to data source** section, enter the following settings for the connection to your data source:
    - **URL (1)**: `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv`
    - **Connection (2)**: Create new connection
    - **Connection name (3)**: *Specify a unique name*
    - **Data Gateway (4)**: Leave it to **(none)**
    - **Authentication kind (5)**: Basic (*Leave password blank*)
    - **Username (6)**: Provide the username 
    - Click on **Next (7)**
  
        ![04](./Images/dp2-7.png)
    
8. Select **Next**. Make sure the following settings are selected:
    - **Relative URL**: *Leave blank*
    - **Request method**: GET
    - **Additional headers**: *Leave blank*
    - **Binary copy**: Unselected
    - **Request timeout**: *Leave blank*
    - **Max concurrent connections**: *Leave blank*
  
        ![05](./Images/dp2-8.8.png)
   
9. Wait for the data to be sampled and then ensure that the following settings are selected:
    - **File format (1)**: DelimitedText
    - **Column delimiter (2)**: Comma (,)
    - **Row delimiter (3)**: Line feed (\n)
    - Select **Preview data (4)** to see a sample of the data that will be ingested.

      ![05](./Images/fabric5.png)

10. Select **Preview data** to see a sample of the data that will be ingested. Then close the data preview and select **Next**.

     ![06](./Images/fabric6.png)

1. On the **Choose data destination** page, search **(1)** and select your lakehouse **Lakehouse<inject key="DeploymentID" enableCopy="false"/> (2)**.

   ![05](./Images/dp2-lk.png)

1. Set the following data destination options, and then select **Next (4)**:
    - **Root folder (1)**: Files
    - **Folder path (2)**: new_data
    - **File name (3)**: sales.csv
   
        ![08](./Images/dp2-9.png)

1. Set the following file format options and then select **Next (4)**:
    - **File format (1)**: DelimitedText
    - **Column delimiter (2)**: Comma (,)
    - **Row delimiter (3)**: Line feed (\n)
   
      ![09](./Images/fabric10.png)

1. On the **Copy summary** page, review the details of your copy operation and then select **Save + Run**.

    ![09](./Images/dp2-10.png)

    A new pipeline containing a **Copy data** activity is created, as shown here:

    ![Screenshot of a pipeline with a Copy Data activity.](./Images/dp2-11.png)

1. When the pipeline starts to run, you can monitor its status in the **Output** pane under the pipeline designer. Use the **&#8635;** (*Refresh*) icon to refresh the status, and wait until it has succeeded.

    ![Screenshot of a pipeline with a Copy Data activity.](./Images/dp2-12.png)

1. In the menu bar on the left, select your lakehouse i.e., **Lakehouse<inject key="DeploymentID" enableCopy="false"/>**.

   ![Screenshot of a pipeline with a Copy Data activity.](./Images/dp2-13.png)

1. On the **Home** page, in the **Lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)** pane, expand **Files** and select the **new_data (2)** folder to verify that the **sales.csv (3)** file has been copied.

    ![10](./Images/01/10.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

<validation step="b849afaa-9add-4584-b93b-8c8bbce53379" />

### Task 4: Create a notebook

1. Navigate to the **Home (1)** page, then select **Notebook (2)**.

   ![11](./Images/dp10.1.png)

     >**Note:** If a pop up appears New data sources and languages now available click on skip tour.

    After a few seconds, a new notebook containing a single *cell* will open. Notebooks are made up of one or more cells that can contain *code* or *markdown* (formatted text).
   
1. In the **Explorer** pane on the left, expand **Lakehouses** and click on  **Add** to add the existing Lakehouse.

   ![02](./Images/dp12.1.png)

   ![02](./Images/dp13.1.png)

1. A pompt appears, make sure to select **Existing Lakehouse (1)** and then click on **Add (2)**.

   ![02](./Images/dp14.png)

1. On the **Discover data from your org and beyond and use it to create reports** page , select the **Lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)** and then click on **Add (2).**

   ![02](./Images/dp15.1.png)
   
3. Select the existing cell in the notebook, which contains some simple code, and then replace the default code with the following variable declaration and click on **&#9655; Run cell**.

    ```python
   table_name = "sales"
    ```

   ![11](./Images/dp2-fl.png) 

4. In the **... (1)** menu for the cell (at its top-right) select **Toggle parameter cell (2)**. This configures the cell so that the variables declared in it are treated as parameters when running the notebook from a pipeline.

     ![12](./Images/01/12.png)

5. Under the parameters cell, use the **+ Code** button to add a new code cell. Then add the following code to it:

    ```python
   from pyspark.sql.functions import *

   # Read the new sales data
   df = spark.read.format("csv").option("header","true").option("inferSchema","true").load("Files/new_data/*.csv")

   ## Add month and year columns
   df = df.withColumn("Year", year(col("OrderDate"))).withColumn("Month", month(col("OrderDate")))

   # Derive FirstName and LastName columns
   df = df.withColumn("FirstName", split(col("CustomerName"), " ").getItem(0)).withColumn("LastName", split(col("CustomerName"), " ").getItem(1))

   # Filter and reorder columns
   df = df["SalesOrderNumber", "SalesOrderLineNumber", "OrderDate", "Year", "Month", "FirstName", "LastName", "EmailAddress", "Item", "Quantity", "UnitPrice", "TaxAmount"]

   # Load the data into a managed table
   #Managed tables are tables for which both the schema metadata and the data files are managed by Fabric. The data files for the table are created in the Tables folder.
   df.write.format("delta").mode("append").saveAsTable(table_name)
    ```

    This code loads the data from the sales.csv file that was ingested by the **Copy Data** activity, applies some transformation logic, and saves the transformed data as a **managed table** - appending the data if the table already exists.

6. Verify that your notebooks look similar to this, and then use the **&#9655; Run all** button on the toolbar to run all of the cells it contains.

    ![Screenshot of a notebook with a parameters cell and code to transform data.](./Images/notebook.png)

    > **Note**: Since this is the first time you've run any Spark code in this session, the Spark pool must be started. This means that the first cell can take a minute or so to complete.

7. (Optional) You can also create **external tables** for which the schema metadata is defined in the metastore for the lakehouse, but the data files are stored in an external location.

    ```python
    df.write.format("delta").saveAsTable("external_sales", path="<abfs_path>/external_sales")

    #In the Lakehouse explorer pane, in the ... menu for the Files folder, select Copy ABFS path.

    #The ABFS path is the fully qualified path to the Files folder in the OneLake storage for your lakehouse - similar to this:

    #abfss://workspace@tenant-onelake.dfs.fabric.microsoft.com/lakehousename.Lakehouse/Files
    ```
    > **Note**: To run the above code, you need to replace the <abfs_path> with your abfs path

1. In the hub menu bar on the left, select your lakehouse.

1. When the notebook run has completed, in the **Lakehouse explorer** pane on the left, in the **...** menu for **Tables** select **Refresh** and verify that a **sales** table has been created.

   ![.](./Images/dp2-14.png)

1. In the notebook menu bar, use the ⚙️ **Settings (1)** icon to view the notebook settings. Then set the **Name** of the notebook to **Load Sales Notebook (2)** and close the settings pane.

   ![.](./Images/dp2-15.png)

1. In the **Explorer** pane, refresh the view. Then expand **Tables**, and select the **sales** table to see a preview of the data it contains.

   ![.](./Images/dp2-16.png)

### Task 5: Use SQL to query tables

When you create a lakehouse and define tables in it, an SQL analytics endpoint is automatically created through which the tables can be queried using SQL `SELECT` statements. 

1. Navigate to the **Lakehouse<inject key="DeploymentID" enableCopy="false"/>** on the left pane.

   ![.](./Images/dp2-17.png)

1. At the top-right of the Lakehouse page, switch from **Lakehouse** to **SQL analytics endpoint**. Then wait a short time until the SQL query endpoint for your lakehouse opens in a visual interface from which you can query its tables, as shown here:

    ![Screenshot of the SQL endpoint page.](./Images/sqlanalytics.png)

2. Use the **New SQL query** button to open a new query editor, and enter the following SQL query:

    ```SQL
   SELECT Item, SUM(Quantity * UnitPrice) AS Revenue
   FROM sales
   GROUP BY Item
   ORDER BY Revenue DESC;
    ```

3. Use the **&#9655; Run** button to run the query and view the results, which should show the total revenue for each product.

    ![Screenshot of a SQL query with results.](./Images/sql-query.png)

### Task 6: Create a visual query

While many data professionals are familiar with SQL, data analysts with Power BI experience can apply their Power Query skills to create visual queries.

1. On the toolbar, select **New visual query**.

    ![Screenshot of a SQL query with results.](./Images/dp2-18.png)

3. Drag the **sales** table to the new visual query editor pane that opens to create a Power Query as shown here: 

    ![Screenshot of a Visual query.](./Images/visual-query.png)

4. In the **Manage columns** menu, select **Choose columns**. Then select only the **SalesOrderNumber and SalesOrderLineNumber (1)** columns and click on **OK (2)**.

     ![Screenshot of a Choose columns dialog box.](./Images/dp2-19.png)

    ![Screenshot of a Choose columns dialog box.](./Images/choose-columns.png)

6. Click on **+ (1)**, in the **Transform table** menu, select **Group by (2)**.

    ![Screenshot of a Visual query with results.](./Images/01/Pg3-VisQuery-S4.0.png)

7. Then group the data by using the following **Basic** settings and click on **OK (5)**.

    - **Group by (1)**: SalesOrderNumber
    - **New column name (2)**: LineItems
    - **Operation (3)**: Count distinct values
    - **Column(4)**: SalesOrderLineNumber

        ![Screenshot of a Visual query with results.](./Images/dp2-gb.png)

8. When you're done, the results pane under the visual query shows the number of line items for each sales order.

    ![Screenshot of a Visual query with results.](./Images/visual-query-results.png)

### Task 7: Create a report

The tables in your lakehouse are automatically added to a default dataset that defines a data model for reporting with Power BI.

1. At the bottom of the SQL analytics endpoint page, select the **Model layouts** tab. The data model schema for the dataset is shown.

    ![Screenshot of a data model.](./Images/dp2-20.png)

    > **Note**: In this exercise, the data model consists of a single table. In a real-world scenario, you would likely create multiple tables in your lakehouse, each of which would be included in the model. You could then define relationships between these tables in the model.

2. In the menu ribbon, select the **Reporting (1)** tab. Then select **New report (2)**. A new browser tab opens in which you can design your report.

    ![Screenshot of a data model.](./Images/dp2-21.png)

    >**Note:** On the **New report with all available data** pop-up, select **Continue**.

4. In the **Data** pane on the right, expand the **sales (1)** table. Then select the following fields:
    - **Item (2)**
    - **Quantity (3)**

    A table visualization is added to the report:

    ![Screenshot of a report containing a table.](./Images/newsales.png)

5. Hide the **Data** and **Filters** panes to create more space. Then ensure the table visualization is selected and in the **Visualizations** pane, change the visualization to a **Clustered bar chart** and resize it as shown here.

    ![Screenshot of a report containing a clustered bar chart.](./Images/clustered-bar-chart.png)

6. On the **File (1)** menu, select **Save (2)**. 

    ![Screenshot of a report containing a table.](./Images/dp2-22.png)

1. Then **Save** the report as **Item Sales Report** in the workspace you created previously.

   ![Screenshot of a report containing a table.](./Images/dp2-23.png)

8. Close the browser tab containing the report to return to the SQL analytics endpoint for your lakehouse. Then, in the hub menu bar on the left, select your workspace to verify that it contains the following items:
    - Your lakehouse.
    - The SQL analytics endpoint for your lakehouse.
    - A default dataset for the tables in your lakehouse.
    - The **Item Sales Report** report.
  
      ![Screenshot of a report containing a table.](./Images/dp2-24.png)  

### Review
 In this lab, you have completed the following :
- Created a lakehouse
- Explored shortcuts
- Created a pipeline
- Created a notebook
- Used SQL to query tables
- Created a visual query
- Created a report

## You have successfully completed this lab, please proceed with the upcoming modules.

