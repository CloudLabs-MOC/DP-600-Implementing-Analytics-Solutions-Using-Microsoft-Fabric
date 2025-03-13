# Module 01a: Ingest Data in Data Flows (Gen2) in MS Fabric

## Lab scenario 

In Microsoft Fabric, Dataflows (Gen2) connect to various data sources and perform transformations in Power Query Online. They can then be used in Data Pipelines to ingest data into a lakehouse or other analytical store or to define a dataset for a Power BI report.

This lab is designed to introduce the different elements of Dataflows (Gen2), and not create a complex solution that may exist in an enterprise.

## Lab objectives
In this lab, you will perform the below tasks:

- Task 1: Create a Lakehouse
- Task 2: Create a Dataflow (Gen2) to ingest data
- Task 3: Add data destination for Dataflow
- Task 4: Add a dataflow to a pipeline

## Estimated timing: 30 minutes

## Architecture Diagram

![](Images/Arch-01.png)

### Task 1: Create a Lakehouse

Large-scale data analytics solutions have traditionally been built around a *data warehouse*, in which data is stored in relational tables and queried using SQL. The growth in "big data" (characterized by high *volumes*, *variety*, and *velocity* of new data assets) together with the availability of low-cost storage and cloud-scale distributed computing technologies has led to an alternative approach to analytical data storage; the *data lake*. In a data lake, data is stored as files without imposing a fixed schema for storage. Increasingly, data engineers and analysts seek to benefit from the best features of both of these approaches by combining them in a *data lakehouse*; in which data is stored in files in a data lake and a relational schema is applied to them as a metadata layer so that they can be queried using traditional SQL semantics.

In Microsoft Fabric, a lakehouse provides highly scalable file storage in a *OneLake* store (built on Azure Data Lake Store Gen2) with a metastore for relational objects such as tables and views based on the open source *Delta Lake* table format. Delta Lake enables you to define a schema of tables in your lakehouse that you can query using SQL.


Now that you have created a workspace in the previous step, it's time to switch to the *Data engineering* experience in the portal and create a data lakehouse into which you will ingest data.

1. In the bottom left corner of the Power BI portal, click the **Power BI** icon and select **Fabric** to switch to the Fabric experience.

   ![02](./Images/01/Pg3-T1-S10.png)
   
2. From the left pane, click on **Fabric (1)**, select **+ New item (2)**, then scroll down in the pop-up and choose **Lakehouse** to create a new Lakehouse.

    - **Name:** Enter **Lakehouse<inject key="DeploymentID" enableCopy="false"/>**

    - Click on **Create**.

      ![02](./Images/01/dp-600-newimage40.png)
  
      ![02](./Images/01/dp-600-newimage5.png)

        >**Note:** After a minute or so, a new lakehouse with no **Tables** or **Files** will be created.

3. On the **Lakehouse<inject key="DeploymentID" enableCopy="false"/>** tab in the pane on the left, in the **...** menu for the **Files (1)** node, select **New subfolder (2)** and create a subfolder named **new_data**

   ![02](./Images/01/01.png)
   ![02](./Images/01/dp-600-newimage41.png)

### Task 2: Create a Dataflow (Gen2) to ingest data

Now that you have a lakehouse, you need to ingest some data into it. One way to do this is to define a dataflow that encapsulates an *extract, transform, and load* (ETL) process.

1. In the home page for your **fabric (1)** workspace, select **Dataflow Gen2** to create. After a few seconds, the Power Query editor for your new dataflow opens as shown in second image.

    ![02](./Images/01/dp-600-newimage600.png)

    ![New dataflow.](./Images/new-dataflow.png)

1. Select **Import from a Text/CSV file**, and create a new data source with the following settings and click on **Next**:
    - **Link to file**: *Selected*
    - **File path or URL**: `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv`
    - **Connection**: Create new connection
    - **data gateway**: (none)
    - **Authentication kind**: Anonymous

    ![02](./Images/01/dp-600-newimage70.png)
    ![02](./Images/01/dp-600-newimage7.png)

1. Click on **Create** the data source. The Power Query editor shows the data source and an initial set of query steps to format the data, as shown here:

   ![02](./Images/01/dp-600-newimage8.png)

1. On the toolbar ribbon, select the **Add column** tab. Then select **Custom column** and create a new column named **MonthNo** that contains a number based on the formula `Date.Month([OrderDate])`, and select **OK** - as shown here:

    ![02](./Images/01/dp-600-newimage9.png)

    ![02](./Images/01/dp-600-newimage10.png)

    >**Note:** The step to add the custom column is added to the query and the resulting column is displayed in the data pane:

    ![Query with a custom column step.](./Images/custom-column-added.png)

    > **Tip:** In the Query Settings pane on the right side, notice the **Applied Steps** include each transformation step. At the bottom, you can also toggle the **Diagram flow** button to turn on the Visual Diagram of the steps. Steps can be moved up or down, edited by selecting the gear icon, and you can select each step to see the transformations applied in the preview pane.

### Task 3: Add data destination for Dataflow

1. On the toolbar ribbon, select the **Home** tab. On the right side under **Query Settings**, click the **+** icon next to **Data Destination**, then choose **Lakehouse** from the dropdown.

   > **Note:** If this option is greyed out, you may already have a data destination set. Check the data destination at the bottom of the Query settings pane on the right side of the Power Query editor. If a destination is already set, you can change it using the gear.

     ![02](./Images/01/dp-600-newimage111.png)

1. Select settings option data destination at the bottom of the Query settings pane. In the **Connect to data destination** dialog box, select **Edit the connection** and sign in using your Power BI organizational account if already signed in click on **Next** to set the identity that the dataflow uses to access the lakehouse.

   ![02](./Images/01/dp-600-newimage15.png)

   ![02](./Images/01/dp-600-newimage16.png)

1. In the list of available workspaces, find your workspace and select the **Lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)** you created in it at the start of this exercise. Then specify a new table named **orders (2)** Select **Next (3)** and :

   ![Data destination configuration page.](./Images/lakehouse.png)

1. If the **Use automatic settings is On**, toggle button to **switch it off**. Select the **OrderDate** and **MonthNo** columns in Power Query online. Now, change the Source type by selecting the drop-down, follow these instructions:

    - OrderDate = Date/Time
    - MonthNo = Whole number

    >**Note**: MonthNo option is greyed out first select  **Source type** by Whole number.

1. On the **Choose destination settings** page, select **Append** under Update method, and then select **Save settings**.

     ![02](./Images/01/dp-600-newimage14.png)

1. Select **Publish** to publish the dataflow. Then wait for the **Dataflow 1** dataflow to be created in your workspace.

    ![Query with a lakehouse destination.](./Images/publish.png)

1. Once published, select the **ellipses (...)**, select **Properties**, and rename your dataflow as **Transform Orders Dataflow**, select **Save**.

     ![02](./Images/01/dp-600-newimage12.png)

     ![02](./Images/01/dp-600-newimage13.png)

### Task 4: Add a dataflow to a pipeline

You can include a dataflow as an activity in a pipeline. Pipelines are used to orchestrate data ingestion and processing activities, enabling you to combine dataflows with other kinds of operations in a single, scheduled process. Pipelines can be created in a few different experiences, including the Data Factory experience.

1. From your Fabric-enabled workspace, select **+ New Item (1)**. Select **Data pipeline (2)** from the pop-up, then when prompted, create a new pipeline named **Load Orders pipeline**.

   The pipeline editor opens.

   ![02](./Images/01/dp-600-newimage170.png)

   ![02](./Images/01/dp-600-newimage18.png)

   >**Note** : If your not able to see **Data pipeline** use > button to see **Data pipeline** option 
   
   > **Tip**: If the Copy Data wizard opens automatically, close it!

1. Select **Pipeline activity**, and add a **Dataflow** activity to the pipeline.

    ![Pipeline with a dataflow activity.](./Images/dp-600-lab01-21.png)

1. With the new **Dataflow1** activity selected, on the **Settings** tab, in the **Dataflow** drop-down list, select **Transform Orders Dataflow** (the data flow you created previously)

   ![02](./Images/01/dp-600-newimage19.png)

1. On the **Home** tab, save the pipeline using the **&#128427;** **Save** icon.

1. Use the **&#9655; Run** button to run the pipeline, and wait for it to complete. It may take a few minutes.

   ![Pipeline with a dataflow that has completed successfully.](./Images/dataflow-pipeline-succeeded.png)

1. In the menu bar on the left edge, select your **lakehouse**.

1. In the **...** menu for **Tables**, select **Refresh**. Then expand **Tables** and select the **orders** table, which has been created by your dataflow.

   ![Table loaded by a dataflow.](./Images/lakehouse(1).png)

### Review
 In this lab, you have completed the following :
- Created a Lakehouse
- Created a Dataflow (Gen2) to ingest data
- Added data destination for Dataflow
- Added a dataflow to a pipeline

## You have successfully completed this lab.
