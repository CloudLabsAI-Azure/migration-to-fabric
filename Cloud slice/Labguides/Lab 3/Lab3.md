## Lab 3: Migrate Azure Synapse Spark and Lake Database to Microsoft Fabric

### Estimated Duration: 120 Minutes

## Overview

In this lab, you will learn how to migrate data and workloads from Azure Synapse Analytics to Microsoft Fabric. The lab covers creating and configuring a Synapse workspace, uploading and accessing data from Azure Data Lake Storage Gen2, and setting up a Microsoft Fabric workspace with a Lakehouse.

You will also explore how to use shortcuts to connect external storage, transform data using notebooks, and rebuild data pipelines in Microsoft Fabric. This hands-on exercise provides practical experience in modernizing data platforms and implementing a unified analytics solution.

**Objective**

By the end of this lab, you will be able to:

- Create and configure an Azure Synapse workspace and dedicated SQL pool

- Upload and manage data in Azure Data Lake Storage Gen2

- Set up a Microsoft Fabric workspace and Lakehouse

- Create shortcuts to access external data in OneLake

- Transform and load data using notebooks

- Rebuild and execute data pipelines in Microsoft Fabric

- Validate migrated data and resources

## Task 1: Create a Synapse workspace in the Azure portal

1. In the search bar, enter **Synapse** and select **Azure Synapse Analytics**.

    ![](../Lab1/media/image5.png)   

1. Click **Create**.

    ![](../Lab1/media/image6.png)

1. Enter the following details to create the resource group, then click **OK**.

    - **Subscription**: Select the default subscription
    - **Resource Group**: Select the assigned Resource group
    - **Managed Resource group:**  Leave this blank.
    - **Workspace name**: fabric-synapse<inject key="DeploymentID" enableCopy="false"/>
    - **Region**: Select the region of your resource group

        ![](../Lab1/media/image7.png)

    - **Select Data Lake Storage Gen2 account:** From subscription
    - **Account name:** Create
    New: **fabricsynapsegen2<inject key="DeploymentID" enableCopy="false"/>**
    - Click **OK**

        ![](../Lab1/media/image8.png)

    - **File System Name**: Create
    New: **synapsefile<inject key="DeploymentID" enableCopy="false"/>**
1. Enter the following details, then click **Next: Security >**.

        ![](../Lab1/media/image9.png)

1. Configure the **Security** settings by selecting **both Microsoft
    
    - **SQL Server admin login:** sqladmin
    - **SQL Password**: password321!
    - Click **Review + create**

    ![](../Lab1/media/image11.png)

1.  In the **Review + submit** tab, once the validation is passed, click on the **Create** button.

    ![](../Lab1/media/image12.png)

1. This deployment may take a few minutes.

    ![](../Lab1/media/image13.png)

1. Click on **Go to resource group** button.

    ![](../Lab1/media/image14.png)

1. Click on your **Synapse workspace**.

    ![](../Lab1/media/image15.png)

## Task 2: Create a dedicated SQL pool

1. In the **Open Synapse Studio** dialog, click **Open** to launch Azure Synapse Studio.

    ![](../Lab1/media/image16.png)

1. In Synapse Studio, select **Manage** (left pane).

    ![](../Lab1/media/image17.png)

1. In Synapse Studio, on the left-side pane, select **Manage** \> **SQL pools** under **Analytics pools** and then click on **New**

    ![](../Lab1/media/image18.png)

1. **Configure SQL Pool**

    - **SQL pool name:** **sql dedicated pool**
    - **Performance level:** Choose **DW100c**
    - Click **Review + Create → Create**.

        ![](../Lab1/media/image19.png)

1. In the **Review + submit** tab, once the Validation is Passed, click on the **Create** button.

    ![](../Lab1/media/image20.png)

    ![](../Lab1/media/image21.png)

1. Go to **Manage → SQL pools** and confirm the Dedicated SQL Pool shows **Online**

    ![](../Lab1/media/image22.png)

1. Return to the **Azure portal**. In the **Overview** section of the Synapse workspace, copy the **Dedicated SQL endpoint** and **Dedicated SQL pool** details, and save them in a Notepad for later use.

    ![](../Lab1/media/image23.png)

## Task 3: Upload Sample Data into the Primary Storage Account

Open Synapse Studio

1. In Synapse Studio, navigate to the **Data Hub** and select **Linked**.

    ![](./media/image24.png)

1. Under **Azure Data Lake Storage Gen2**, locate the item with your workspace name, such as **fabric-synapse<inject key="DeploymentID" enableCopy="false"/> (Primary — asastorageaccount01 (your storage account))**.

    ![](./media/image25.png)

1. Click **+ New folder**.

![](./media/image26.png)

1.  Enter the folder name as **FabricMigration** and click the
    **Create** button.
    ![](./media/image27.png)

1.  Select **FabricMigration** folder

![](./media/image28.png)

1.  Click **Upload**.

![](./media/image29.png)

1.  Click on **Folder**.

![](./media/image30.png)

1. Browse to **C:\LabFiles\LabFiles\Lab1** on your VM, then
    select **all** file except DACPAC file and click on **Open** button.

![](./media/image31.png)

1.  After selecting all the required files, click the **Upload** button
    to add them to the destination folder.

![](./media/image32.png)

![](./media/image33.png)

1.  Right-click on the folder and select **Properties.**

    ![](./media/image34.png)

1.  Copy the **URL** path and save it in notepad to use
    it for later use

    ![](./media/image35.png)

1.  Right-click on the **DimCustomer.csv** and select
    **Properties.**

    ![](./media/image36.png)

1. Copy the **ABFSS** path and save it in notepad to use it for later use

    ![](./media/image37.png)

## Task 4: Create a Fabric workspace

In this task, you set up a new Microsoft Fabric workspace to serve as
the central hub for the Medallion Architecture implementation. This
workspace brings together all essential components, including
Lakehouses, Dataflows, Pipelines, Warehouses, Notebooks, and Power BI
assets.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: +++https://app.fabric.microsoft.com/+++ then
    press the **Enter** button.

1.  On the **Fabric Home** page click on **+ New Workspaces** as shown
    in the image below.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image38.png)

| Field                     | Value                                      |
|--------------------------|--------------------------------------------|
| Name                     | +++ FabricMigrationLab@lab.LabInstance.Id+++ |
| Advanced                 | Select Fabric                              |
| Default storage format   | Small dataset storage format               |

1. Enter the following details in the **Create a workspace** pane, then click **Apply**.

    ![](./media/image39.png)

    ![](./media/image40.png)

    ![](./media/image41.png)

## Task 5: Create a Lakehouse

1. Click the **+New item** button.

    ![](./media/image42.png)

1. Click the **Lakehouse** tile.

    ![](./media/image43.png)

1. Enter +++**SynapseMigrationLakehouse+++** in the **Name** field, then click **Create** and open the new lakehouse.

    ![](./media/image44.png)

    ![](./media/image45.png)

## Task 5: Create Shortcut in the Files Section

1.  In the **Lakehouse**, select the **New Shortcut** tile

![](./media/image46.png)

1.  From the list of external sources, select **Azure Data Lake Storage
    Gen2** to create a new shortcut.

![](./media/image47.png)

1.  Select **New connection**, enter the **ADLS Gen2 URI**, and then
    click **Next** to proceed with creating the shortcut.

    ![](./media/image48.png)

1.  Select **synapsefile** and click **Next**

    ![](./media/image49.png)

1.  Click **Create**

    ![](./media/image50.png)

1.  Select the **synapsefile** folder to review the files.

    ![](./media/image51.png)

![](./media/image52.png)

1. In the **Lakehouse** page, click **Open notebook** in the command bar and select **New notebook**.

![](./media/image53.png)

1. Replace the code in the cell with the following code, then click **▷ Run cell** to review the output.

    ```python
    df = spark.read.format("csv").load("Dimension_Customer File ABFSS URI ")
    ```

    Replace **Dimension_Customer** with the **ABFSS URI** from Task 3, Step 12.

    **Example:**

    ```python
    df = spark.read.format("csv").load("abfss://synapsefile@fabricsynapsegen21.dfs.core.windows.net/FabricMigration/Dimension_Customer.csv")
    ```

    ![](./media/image54.png)

1. Click the **+ Code** icon below the cell output to add a new code cell, enter the following code, and click **▷ Run cell**.

    ```python
    df.write.format("delta").mode("overwrite").save("Tables/Customer")
    ```

    ![](./media/image55.png)

1. To validate the created tables, refresh the **Tables** section in the **Explorer** panel until all tables appear.

    ![](./media/image56.png)

    ![](./media/image57.png)

## Task 7: Rebuild Synapse Pipelines in Microsoft Fabric

1. From the left menu, click the workspace icon and select your workspace name.

    ![](./media/image58.png)

1. Click **+ New item** and select **Pipeline**.

    ![](./media/image59.png)

1. Enter +++**Migrate_Pipeline+++** as the pipeline name and click **Create**.

    ![](./media/image60.png)

    ![](./media/image61.png)

1.  On newly created pipeline, select **Copy data** dropdown and
    choose **Add copy data activity** option.

    ![](./media/image62.png)

1.  With the **copy data** being selected, navigate to **Source** tab.

    ![](./media/image63.png)

1.  Select the **Connection** dropdown and select **Browse all** option.

    ![](./media/image64.png)

1.  Select **+ New** from the left pane

    ![](./media/image65.png)

1.  From the data source options, select **Azure Data Lake Storage
    Gen2** to begin the connection setup.

    ![](./media/image66.png)

1.  Select **New connection**, enter the **ADLS Gen2 URI**, and then
    click **Connect** to proceed with creating the shortcut.

    ![](./media/image67.png)

1. In the **Source** tab of the Copy Data activity, click the
    **Browse** button to select the folder containing the source files.

    ![](./media/image68.png)

1. Select the **Fact_Order.csv** file and click **OK**.

    ![](./media/image69.png)

1. Navigate to the **Destination** tab and click **Browse all**.

    ![](./media/image70.png)

1. In the destination selection window, select **OneLake catalog** from the left pane and then select **synapseMigrationLakehouse**.

![](./media/image71.png)

1. In the **Source** tab, select **DelimitedText** as the file format.

    ![](./media/image72.png)

1. Under the **Destination** tab, click **+ New**.

![](./media/image73.png)

1. Enter the table name as +++**dim_Date+++** and click **Create**.

    ![](./media/image74.png)

1. Click **Run** to execute the copy activity.

    ![](./media/image75.png)

1. Click **Save and run** to save and execute the pipeline.

    ![](./media/image76.png)

    ![](./media/image77.png)

1. After the pipeline completes successfully, navigate to your workspace and click **Lakehouse**.

    ![](./media/image78.png)

    ![](./media/image79.png)

## Task 8: Delete the Resources

1. Search for **Resource groups** in the Azure portal and select it under **Services**.

![A screenshot of a computer Description automatically
generated](./media/image80.png)

1.  In the Resource groups page, select your resource group.

1.  On the **Resource Group** home page, select all resources and then
    click **Delete**.

![](./media/image81.png)

1. In the **Delete Resources** pane, type "delete" in the confirmation field and click **Delete**.

    ![](./media/image82.png)

![](./media/image83.png)

1. Go to the Microsoft Fabric workspace at +++https://app.fabric.microsoft.com/+++.

1. Select the **...** option under the workspace name and choose **Workspace settings**.

    ![](./media/image84.png)

1. Select **General** and choose **Remove this workspace**.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image85.png)

1. Click **Delete** in the confirmation dialog.

    ![](./media/image86.png)

1.  Wait for a notification that the Workspace has been deleted, before
    proceeding to the next lab.

    ![](./media/image87.png)

**Summary**

In this lab, you successfully migrated data and basic pipeline
functionality from Azure Synapse Analytics to Microsoft Fabric. You
created and configured necessary resources in both platforms, accessed
and transformed data using Lakehouse and notebooks, and rebuilt
pipelines to enable data movement within Fabric.

This lab demonstrates how Microsoft Fabric simplifies data integration
and analytics by providing a unified platform, making it easier to
modernize existing Synapse-based solutions.
