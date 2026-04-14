## Lab 2: Migrate Azure Synapse Analytics SQL objects to Fabric Data Warehouse

### Estimated Duration: 120 Minutes

## Overview

This lab focuses on migrating SQL objects and data from **Azure Synapse Analytics (Dedicated SQL Pool)** to **Microsoft Fabric Data Warehouse**. It demonstrates how to create a warehouse in Fabric, establish a connection to Synapse, and use pipelines to copy data into OneLake. The lab also covers validating migrated data and scheduling pipelines for automated data movement.

## Objectives

By the end of this lab, you will be able to:

- Task 1: Create a Warehouse
- Task 2: Create Linked Connection to Synapse SQL
- Task 3: Schedule the Pipeline

## Task 1: Create a Warehouse

In this task, you will create a new warehouse in Microsoft Fabric to store migrated data.

1. Open your browser and navigate to the following URL to open **Microsoft Fabric** portal: 

    ```
    https://app.fabric.microsoft.com/
    ``` 

1. Select **Workspaces (1)** from the left navigation pane, and then choose **Fabric_Migration<inject key="DeploymentID" enableCopy="false"/> (2)** from the list.

    ![](./media/new0.png)

1. Click **+ New item (1)**, then select **Warehouse (2)** to create a new Fabric Warehouse.

    ![](./media/image1.png)

1. In the **New warehouse** dialog box, enter **sqlpoolmigrate (1)** in the **Name** field, click **Create (2)** and open the new warehouse.

    ![](./media/new1.png)

    ![](./media/image3.png)

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task.
    > - If you receive a success message, you can proceed to the next task.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

    <validation step="1ee57940-621b-4154-aeb5-75fd27b16bd0" />    

## Task 2: Create Linked Connection to Synapse SQL

 In this task, you will establish a connection between Microsoft Fabric and Azure Synapse Analytics to copy and validate data.

1. Select **Workspaces (1)** from the left navigation pane, and then choose **Fabric_Migration<inject key="DeploymentID" enableCopy="false"/> (2)** from the list.

    ![](./media/new0.png)

1. In the **Fabric_Migration<inject key="DeploymentID" enableCopy="false"/>** page, select **+ New item (1)**, then click **Pipeline (2)** to view the full list of available items under **Get data**.

    ![](./media/image5.png)

1. In the **New pipeline** dialog box, enter **sqlpool_migratepipeline (1)** in the **Name** field, then click **Create (2)**.

    ![](./media/new2.png)

1. On newly created pipeline, select **Copy data (1)** dropdown and choose **Add copy data activity (2)** option.

    ![](./media/image7.png)

1. With the **copy data** being selected, navigate to **Source** tab.

    ![](./media/image8.png)

1. Select the **Connection** dropdown **(1)** and select **Browse all (2)** option.

    ![](./media/image9.png)

1. Select **+ New** from the left pane.  

    ![](./media/image10.png)

1. From the data source options, select **Azure Synapse Analytics (SQL DW)** to begin the connection setup.

    ![](./media/image11.png)

1. Select **New Connection → Azure Synapse Analytics (SQL)** and enter the following details, then click **Connect (6)**:

    | Field    | Value                                              |
    |----------|----------------------------------------------------|
    | Server   | Enter Dedicated SQL Endpoint that you pasted into the Notepad in **Task 2 → Step 7** **(1)**   |
    | Database | **sql dedicated pool (2)**                  |
    | Authentication | **Basic (3)**      |
    | Username | `sqladmin`  **(4)**                                        |
    | Password | `password321!`  **(5)**                                     |

     ![](./media/new4.png)

1. Under Source tab, open the **Table** dropdown and select the **fabric_employee** table.

    ![](./media/image13.png)

    ![](./media/image14.png)

1. Now, navigate to **destination** tab.

    ![](./media/image15.png)

1. Select the **Connection** dropdown **(1)** and select **Browse all (2)** option.

    ![](./media/image16.png)

1. On choose a destination window, select **OneLake catalog** from the left pane and select the **sqlpoolmigrate**.

    ![](./media/image17.png)

    ![](./media/image18.png)

1. In the **Destination** tab, select **Auto create table (1)**, specify the schema as **dbo (2)**, and enter the table name as **employee (3)** to automatically create and load data into the table.

    ![](./media/image19.png)

1. Click on **Run** to run the copy data.

    ![](./media/image20.png)

1. Click on **Save and run** button so that pipeline will be save and
    run.

    ![](./media/image21.png)

    ![](./media/image22.png)

    ![](./media/image23.png)

1. After the successful execution of the pipeline, go to your SQL analytics endpoint Lakehouse and open the explorer to see the imported data.

1. Select **Fabric_Migration<inject key="DeploymentID" enableCopy="false"/> workspace (1)** from the left navigation, and select **sqlpoolmigrate (2)** to view and access the created warehouse.

    ![](./media/image24.png)

1. Expand **sqlpoolmigrate**, navigate to **Schemas → dbo → Tables**, and select the **employee** table to verify that it has been successfully created and loaded.

    ![](./media/image25.png)

1. Select **New SQL query** to write your SQL statements.

    ![](./media/image26.png)

1. Enter the following query **(1)**, then click the **Run (2)** icon to run it and observe the counts in the **output (3)**.

    ```
    SELECT COUNT(*) FROM dbo.employee;
    ```

    ![](./media/image27.png)

1. Enter the following query **(1)**, then click the **Run (2)** icon to run it and observe the results.

    ```
    SELECT TOP 10 * FROM dbo.employee;
    ```
    ![](./media/image28.png)

1. Compare row counts with Synapse: return to the Synapse workspace, navigate to the **Develop (1)** section, click the **+ (2)** button, and select **SQL script (3)** to create a new SQL script.

    ![](./media/image29.png)

1. Ensure that the SQL script is connected to the **SQL dedicated pool** by selecting it from both the **Connect to** dropdown and the **Use database** dropdown, as highlighted in the image

    ![](./media/image30.png)

1. Enter the following code **(1)** into the editor and click **Run (2)** to execute it.

    ```
    SELECT COUNT(*) FROM dbo.fabric_employee;
    ```

    ![](./media/image31.png)

    ![](./media/image32.png)

    > **Note:** we had renamed *dbo.fabric_employee* to *dbo.employee* for migration

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task.
    > - If you receive a success message, you can proceed to the next task.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

    <validation step="01133add-9237-42c6-b39e-0dd25c717758" />    

## Task 3: Schedule the Pipeline

In this task, you will configure a schedule to automate pipeline execution for recurring data migration.

1. Navigate back to the Fabric portal, click **Fabric_migration<inject key="DeploymentID" enableCopy="false"/>** in the left navigation pane, and then select **sql_migratepipeline**.

    ![](./media/image33.png)

1. Click **Schedule**.

    ![](./media/image34.png)

1. Click **+ Add schedule** to configure a new schedule for automatically running the pipeline.

    ![](./media/image35.png)

1. Configure the schedule as required. The example here schedules the pipeline to execute daily at 8:00 PM until the end of the year.

1. In Failure notification, enter the **<inject key="AzureAdUserEmail"></inject> (1)** . Select **Daily (2)** under Repeat, set Time of day, and click Save **(3)** to schedule the pipeline execution.

    ![](./media/image36.png)

## Review

In this lab, you successfully migrated data from Azure Synapse Analytics to Microsoft Fabric Data Warehouse. You created a new warehouse, configured a pipeline with a Synapse SQL source, and loaded data into Fabric using OneLake. After executing the pipeline, you validated the data by comparing row counts between Synapse and Fabric. Finally, you scheduled the pipeline to automate recurring data transfers, ensuring a streamlined and scalable migration process.

## Congratulation! You have successfully completed this lab.