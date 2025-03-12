# Module 04a: Get started with data warehouses in Microsoft Fabric

## Lab scenario

In Microsoft Fabric, a data warehouse provides a relational database for large-scale analytics. Unlike the default read-only SQL endpoint for tables defined in a lakehouse, a data warehouse provides full SQL semantics; including the ability to insert, update, and delete data in the tables.

## Lab objectives
In this lab, you will perform:

- Create a data warehouse
- Create tables and insert data
- Define a data model
- Query data warehouse tables
- Query fact and dimension tables
- Create a view
- Create a visual query
- Visualize your data

## Estimated timing: 30 minutes

## Architecture Diagram

![](Images/Arch-07.png)

### Task 1: Create a data warehouse

Now that you have a workspace, it’s time to create a data warehouse. The Synapse Data Warehouse home page includes a shortcut to create a new warehouse:

1. In the bottom left corner of the Power BI portal, click the **Power BI** icon and select **Fabric** to switch to the Fabric experience.

   ![02](./Images/01/Pg3-T1-S10.png)

1. From the left pane, click on **Fabric (1)**, select **+ New item (2)**, then scroll down in the pop-up and choose **Warehouse** to create a new Warehouse with a name **Warehouse<inject key="DeploymentID" enableCopy="false"/>**.

    >**Note:** After a minute or so, a new warehouse will be created:

    ![Screenshot of uploaded files in a lakehouse.](./Images/create-warehouse-1203.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

<validation step="35e684af-d817-4bb9-96e5-1f76992b4473" />

### Task 2: Create tables and insert data

A warehouse is a relational database in which you can define tables and other objects.

1. In your new warehouse, select the **Create tables with T-SQL** tile, and replace the default SQL code with the following CREATE TABLE statement:

   ![02](./Images/t-sql-1203.png)

    ```Sql
    CREATE TABLE dbo.DimProduct
    (
        ProductKey INTEGER NOT NULL,
        ProductAltKey VARCHAR(25) NULL,
        ProductName VARCHAR(50) NOT NULL,
        Category VARCHAR(50) NULL,
        ListPrice DECIMAL(5,2) NULL
    );
    GO
    ```

1. Use the **▷ Run** button to run the SQL script, which creates a new table named **DimProduct** in the **dbo** schema of the data warehouse.

1. Use the **Refresh** button on the toolbar to refresh the view. Then, in the **Explorer** pane, expand **Schemas** > **dbo** > **Tables** and verify that the **DimProduct** table has been created.

    ![Screenshot of uploaded files in a lakehouse.](./Images/dbowarehouse.png)

1. On the **Home** menu tab, use the **New SQL Query** button to create a new query, and enter the following INSERT statement:

    ![](./Images/new-sql-query-1203.png)

    ```Sql
    INSERT INTO dbo.DimProduct
    VALUES
    (1, 'RING1', 'Bicycle bell', 'Accessories', 5.99),
    (2, 'BRITE1', 'Front light', 'Accessories', 15.49),
    (3, 'BRITE2', 'Rear light', 'Accessories', 15.49);
    GO
    ```

1. Run the new query to insert three rows into the **DimProduct** table.

1. When the query has finished, in the **Explorer** pane, select the **DimProduct (1)** table and verify that the **three rows (2)** have been added to the table.

    ![Screenshot of uploaded files in a lakehouse.](./Images/dimproduct.png)

1. On the Home menu tab, use the New SQL Query button to create a new query for table. Open the first text file, from **C:\LabFiles\Files\create-dw-01.txt**, and copy the Transact-SQL code related to the 'DimProduct' and paste it inside the table. 

1. Open the second text file, from **C:\LabFiles\Files\create-dw-02.txt**, copy the code and paste the code into the same query pane, where you pasted the above code. 

1. Now, open the third text file, from **C:\LabFiles\Files\create-dw-03.txt**, copy the code and paste the code into the query pane, where you pasted the above codes. Please ensure that each query pasted in a sequence.

    ![01](./Images/02/Pg4-T2-S7.png)

    >**Important**: Please ensure that each query pasted in a sequence.

1. Run the query, which creates a simple data warehouse schema and loads some data. The script should take around 30 seconds to run.

1. Use the **Refresh** button on the toolbar to refresh the view. Then in the **Explorer** pane, verify that the **dbo** schema in the data warehouse now contains the following four tables:

    - **DimCustomer**
    - **DimDate**
    - **DimProduct**
    - **FactSalesOrder**

        >**Tip:** If the schema takes a while to load, just refresh the browser page.

### Task 3: Define a data model

A relational data warehouse typically consists of fact and dimension tables. The fact tables contain numeric measures you can aggregate to analyze business performance (for example, sales revenue), and the dimension tables contain attributes of the entities by which you can aggregate the data (for example, product, customer, or time). In a Microsoft Fabric data warehouse, you can use these keys to define a data model that encapsulates the relationships between the tables.

1. From the left pane at the bottom, select the **Model layouts** tab.

1. In the model pane, rearrange the tables in your data warehouse so that the **FactSalesOrder** table is in the middle, like this:

    ![02](./Images/select-model-1203.png)

1. Drag the **ProductKey** field from the **FactSalesOrder** table and drop it on the **ProductKey** field in the **DimProduct** table. Then confirm the following relationship details and click on **Save**.

    - **Table 1**: FactSalesOrder
    - **Column**: ProductKey
    - **Table 2**: DimProduct
    - **Column**: ProductKey
    - **Cardinality**: Many to one (*:1)
    - **Cross filter direction**: Single
    - **Make this relationship active**: Selected
    - **Assume referential integrity**: Unselected

1. Repeat the process to create many-to-one relationships between the following tables and click on **Save**.

    - **FactSalesOrder.CustomerKey** &#8594; **DimCustomer.CustomerKey**

      ![Screenshot of the data warehouse model page.](./Images/save.png)

    - **FactOrderSales.SalesOrderDateKey** &#8594; **DimDate.DateKey**

1. When all of the relationships have been defined, the model should look like this:

    ![02](./Images/model-layout-1203.png)

### Task 4: Query data warehouse tables

Since the data warehouse is a relational database, you can use SQL to query its tables.

#### Task 4.1: Query fact and dimension tables

Most queries in a relational data warehouse involve aggregating and grouping data (using aggregate functions and GROUP BY clauses) across related tables (using JOIN clauses).

1. Create a new SQL Query, and run the following code:

    ```Sql
    SELECT  d.[Year] AS CalendarYear,
            d.[Month] AS MonthOfYear,
            d.MonthName AS MonthName,
            SUM(so.SalesTotal) AS SalesRevenue
    FROM FactSalesOrder AS so
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    GROUP BY d.[Year], d.[Month], d.MonthName
    ORDER BY CalendarYear, MonthOfYear;
    ```

    >**Note:** that the attributes in the time dimension enable you to aggregate the measures in the fact table at multiple hierarchical levels - in this case, year and month. This is a common pattern in data warehouses.

1. Modify the query as follows to add a second dimension to the aggregation.

    ```Sql
    SELECT  d.[Year] AS CalendarYear,
            d.[Month] AS MonthOfYear,
            d.MonthName AS MonthName,
            c.CountryRegion AS SalesRegion,
        SUM(so.SalesTotal) AS SalesRevenue
    FROM FactSalesOrder AS so
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON so.CustomerKey = c.CustomerKey
    GROUP BY d.[Year], d.[Month], d.MonthName, c.CountryRegion
    ORDER BY CalendarYear, MonthOfYear, SalesRegion;
    ```

1. Run the modified query and review the results, which now include sales revenue aggregated by year, month, and sales region.

### Task 5: Create a view

A data warehouse in Microsoft Fabric has many of the same capabilities you may be used to in relational databases. For example, you can create database objects like views and stored procedures to encapsulate SQL logic.

1. Modify the query you created previously as follows to create a view (note that you need to remove the ORDER BY clause to create a view).

    ```Sql
    CREATE VIEW vSalesByRegion
    AS
    SELECT  d.[Year] AS CalendarYear,
            d.[Month] AS MonthOfYear,
            d.MonthName AS MonthName,
            c.CountryRegion AS SalesRegion,
        SUM(so.SalesTotal) AS SalesRevenue
    FROM FactSalesOrder AS so
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON so.CustomerKey = c.CustomerKey
    GROUP BY d.[Year], d.[Month], d.MonthName, c.CountryRegion;
    ```

1. Run the query to create the view. Then refresh the data warehouse schema and verify that the new view is listed in the **Explorer** pane.

1. Create a new SQL query and run the following SELECT statement:

    ```Sql
    SELECT CalendarYear, MonthName, SalesRegion, SalesRevenue
    FROM vSalesByRegion
    ORDER BY CalendarYear, MonthOfYear, SalesRegion;
    ```

#### Task 5.1 Create a visual query

Instead of writing SQL code, you can use the graphical query designer to query the tables in your data warehouse. This experience is similar to Power Query online, where you can create data transformation steps with no code. For more complex tasks, you can use Power Query’s M (Mashup) language.

1. On the **Home** menu, click on **v** next to New SQL Query and select **New visual query**.

    ![02](./Images/new-visual-query-1203.png)

1. Drag **FactSalesOrder** onto the **canvas**. Notice that a **preview** of the table is displayed in the Preview pane below.

1. Drag **DimProduct** onto the **canvas**. We now have two tables in our query.

1. Use the **(+)** button on the **FactSalesOrder** table on the canvas to **Merge queries**. Screenshot of the canvas with the FactSalesOrder table selected.

    ![02](./Images/visualquerymerge(1).png)

1. In the **Merge queries** window, select **DimProduct** as the right table for merge. Select **ProductKey** in both queries, leave the default **Left outer** join type, and click **OK**.

1. In the **Preview**, note that the new **DimProduct** column has been added to the FactSalesOrder table. Expand the column by clicking the arrow to the right of the column name. Select **ProductName** and click **OK**.

    ![02](./Images/visualquerypreview(1).png)

1. If you’re interested in looking at data for a single product, per a manager request, you can now use the **ProductName** column to filter the data in the query. Filter the **ProductName** column to look at **Cable Lock** data only.

1. From here, you can analyze the results of this single query by selecting **Visualize results** or **Open in Excel**. You can now see exactly what the manager was asking for, so we don’t need to analyze the results further.

#### Task 5.2: Visualize your data

You can easily visualize the data in either a single query, or in your data warehouse. Before you visualize, hide columns and/or tables that aren’t friendly to report designers.

1. In the **Explorer** pane, select the **Model layouts**.

1. Hide the following columns in your Fact and Dimension tables that are not necessary to create a report. Note that this does not remove the columns from the model, it simply hides them from view on the report canvas.
    - FactSalesOrder
        - SalesOrderDateKey
        - CustomerKey
        - ProductKey
    - DimCustomer
        - CustomerKey
        - CustomerAltKey
    - DimDate
        - DateKey
        - DateAltKey
    - DimProduct
        - ProductKey
        - ProductAltKey

1. Now you’re ready to build a report and make this dataset available to others. On the **Reporting** tab, select **New report**. On the **New report with all available data** select **Continue**. This will open a new window, where you can create a Power BI report.
    ![02](./Images/new-report-1203.png)

1. In the **Data** pane, expand **FactSalesOrder**. Note that the columns you hide are no longer visible.

1. Select **SalesTotal**. This will add the column to the **Report canvas**. Because the column is a numeric value, the default visual is a **column chart**.

1. Ensure that the column chart on the canvas is active (with a gray border and handles), and then select **Category** from the **DimProduct** table to add a category to your column chart.

1. In the **Visualizations** pane, change the chart type from a column chart to a **clustered bar chart**. Then resize the chart as necessary to ensure that the categories are readable.

    ![02](./Images/visualizationspane(1).png)

1. In the **Visualizations** pane, select the **Format your visual** tab and in the **General** sub-tab, in the **Title** section, change the **Text** to **Total Sales by Category**.

1. In the **File** menu, select **Save**. Then save the report as **Sales Report** in the workspace you created previously.

1. In the menu hub on the left, navigate back to the workspace. Notice that you now have three items saved in your workspace: your data warehouse, its default dataset, and the report you created.

    ![02](./Images/fabricworkspace.png)

### Review
 In this lab, you have completed the following :
- Created a data warehouse
- Created tables and insert data
- Defined a data model
- Queried data warehouse tables
- Queried fact and dimension tables
- Created a view
- Created a visual query
- Visualized your data

## You have successfully completed this lab, please proceed with the upcoming modules.
