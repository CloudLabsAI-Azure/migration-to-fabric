## Lab 3: Migrate Azure Synapse Spark and Lake Database to Microsoft Fabric

### Estimated Duration: 120 Minutes

## Overview

In this lab, you will learn how to migrate data and workloads from Azure Synapse Analytics to Microsoft Fabric. The lab covers creating and configuring a Synapse workspace, uploading and accessing data from Azure Data Lake Storage Gen2, and setting up a Microsoft Fabric workspace with a Lakehouse.

You will also explore how to use shortcuts to connect external storage, transform data using notebooks, and rebuild data pipelines in Microsoft Fabric. This hands-on exercise provides practical experience in modernizing data platforms and implementing a unified analytics solution.

## Objectives

By the end of this lab, you will be able to:

- Task 1: Create a Synapse workspace in the Azure portal
- Task 2: Create a dedicated SQL pool
- Task 3: Upload Sample Data into the Primary Storage Account
- Task 4: Create a Fabric workspace
- Task 5: Create a Lakehouse
- Task 6: Create Shortcut in the Files Section
- Task 7: Rebuild Synapse Pipelines in Microsoft Fabric

## Task 1: Create a Synapse workspace in the Azure portal

1. In the Azure portal search bar, search for **Synapse Analytics (1)** and select **Azure Synapse Analytics (2).** 

   ![](../Lab1/media/image5.png)   

2. In the Azure Synapse Analytics home page, click **+ Create**.

   ![](../Lab1/media/image6.png)

3. Enter below details to create resource group and then click on **OK**.

   - **Subscription**: Select the default subscription **(1)**
   - **Resource Group**: Select the **fabric-rg (1)** Resource group
   - **Workspace name**: Enter **fabric-synapse<inject key="DeploymentID" enableCopy="false"/> (3)**
   - **Region**: <inject key="Region" enableCopy="false"/> **(4)**

     ![](../Lab1/media/image7.png)

   - **Select Data Lake Storage Gen2 account:** From subscription
   - **Account name:** Create
   - **New:** **fabricsynapsegen2<inject key="DeploymentID" enableCopy="false"/> (1)**
   - Click **OK (2)**

     ![](../Lab1/media/image8.png)

   - **File System Name**: **Create (1)**
   - New: **synapsefile<inject key="DeploymentID" enableCopy="false"/> (2)**
   - Click **OK (3)**.
   - Now, click on **Next: Security > (4)**.

     ![](../Lab1/media/image9.png)

4. Configure the **Security** settings by selecting **Use both local and Microsoft Entra ID** authentication as Authentication method:

   - **SQL Server admin login:** `sqladmin` **(1)**
   - **SQL Password**: `password321!` **(2)**
   - Click **Review + create (3)**

      ![](../Lab1/media/image11.png)

5. In the **Review + submit** tab, once the validation is passed, click on the **Create** button.

   ![](../Lab1/media/image12.png)

6. This deployment may take a few minutes.

   ![](../Lab1/media/image13.png)

7. Click on **Go to resource group** button.

   ![](../Lab1/media/image14.png)

8. Click on your **fabric-synapse<inject key="DeploymentID" enableCopy="false"/>** workspace from the list.

   ![](../Lab1/media/image15.png) 

>**Congratulations** on completing the Task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.

<validation step="c1d778f6-2e99-4a3c-b246-06537302d31c" />     

## Task 2: Create a dedicated SQL pool

1. On the **fabric-synapse<inject key="DeploymentID" enableCopy="false"/>** workspace Overview page, click click **Open** in the **Open Synapse Studio** dialog to launch Azure Synapse Studio.

   ![](../Lab1/media/image16.png)

2. In Synapse Studio, select **Manage** from the left navigation pane.

   ![](../Lab1/media/image17.png)

3. Select **SQL pools (1)** under **Analytics pools** and then click on **+ New (2)** to create a new SQL pool.

   ![](../Lab1/media/new0.png)

4. On the Basics tab of **New dedicated SQL pool** page, add the following details:

    - **Dedicated SQL pool name:** **sql dedicated pool (1)**
    - **Performance level:** Choose **DW100c (2)**
    - Click **Review + create (3)**

      ![](../Lab1/media/image19.png)

5. In the **Review + submit** tab, once the Validation is Passed, click on the **Create** button.

   ![](../Lab1/media/image20.png)

   ![](../Lab1/media/image21.png)

6. Go to **Manage → SQL pools** and confirm the newly created Dedicated SQL Pool **sql dedicated pool** shows status as **Online**

   ![](../Lab1/media/image22.png)

7. Return to the **Azure portal**. In the **Overview** section of the Synapse workspace, copy the **Dedicated SQL endpoint** and **Dedicated SQL pool** details, and save them in a Notepad for later use.

   ![](../Lab1/media/image23.png)

>**Congratulations** on completing the Task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.

<validation step="489fd00e-068a-44e4-a1f4-bde64ed02795" />       

## Task 3: Upload Sample Data into the Primary Storage Account

1. Navigate to **Synapse Studio**, then open the **Data (1)** from the left navigation pane and select the **Linked (2)** section.
 
   ![](./media/image24.png)

2. Under the **Linked** tab in the Data pane and expand **Azure Data Lake Storage Gen2** to locate and expand your workspace name, such as **fabric-synapse<inject key="DeploymentID" enableCopy="false"/> (Primary — asastorageaccount01 (your storage account))** to view the container.

   ![](../Lab1/media/image25.png)

3. Select the container named **synapsefile<inject key="DeploymentID" enableCopy="false"/> (Primary)**.

   ![](../Lab1/media/image26.png)

4. Click **+ New folder**.

   ![](./media/image26.png)

5. Enter the folder name as **FabricMigration (1)** and click the **Create (2)** button.

   ![](./media/image27.png)

6. Select **FabricMigration** folder.

   ![](./media/image28.png)

7. Click **Upload** to upload the files in the **FabricMigration** folder.

   ![](./media/image29.png)

8. Click on **Folder** icon next to File Upload.

   ![](./media/image30.png)

9. Browse to **C:\LabFiles\lab file (1)** path, then, select **all (2)** file except DACPAC file and click on **Open (3)** button.

   ![](./media/image31.png)

10. After selecting all the required files, click the **Upload** button to add them to the destination folder.

    ![](./media/new1.png)

11. Right-click the **DimCustomer.csv (1)** file and select **Properties (2)** to view its details and metadata.

    ![](./media/image32.png)

12. Copy the **ABFSS** path and save it in notepad to use it for later use

    ![](./media/image37.png)

13. Right-click the **FabricMigration** folder and select **Properties** to view its details and storage information.

    ![](./media/image34.png)

14. Copy the **URL** path and save it in notepad to use it for later use.

    ![](./media/image35.png)

15. Right-click the **synapsefile<inject key="DeploymentID" enableCopy="false"/> (Primary) (1)** container and select **Properties (2)** to view its details and configuration.

    ![](./media/new5.png)

16. Click the **copy icon (1)** next to the **URL** to copy the storage path and paste it in the Notepad, then click **Close (2)** to exit the properties window.

    ![](./media/new6.png)    

## Task 4: Create a Fabric workspace

1. Open your browser and navigate to the following URL to open **Microsoft Fabric** portal: 

   ```
   https://app.fabric.microsoft.com/
   ```

2. Enter the following credentials to login to the Fabric portal:  

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>    

3. On the **Fabric Home** page click on **+ New Workspaces** as shown in the image below.

    ![](../Lab1/media/image76.png)

4. On the **Create a workspace** pane that appears to the right, enter the following details, and then click **Apply (4)**.

   | Field | Value |
   |------------------------|------------------------------|
   | Name                   | **FabricMigrationLab<inject key="DeploymentID"    enableCopy="false"/> (1)**  |
   | Advanced               | Select **Fabric (2)**                                                        |
   | Default storage format | **Small semantic model storage format (3)**                                           |
   
   ![](./media/new3.png)

   ![](./media/new4.png)

5. The Workspace is now created.

6. Select **Manage access** from the workspace menu.

   ![](./media/new7.png)

7. In the Manage access pane, select **+ Add people or groups**.

   ![](./media/new8.png)

8. In the Add people pane, enter below **URL (1)** in the search box, then select the **Admin (3)** role from the dropdown next to **Viewer (2)** role, and click **Add (4)** to assign permissions.

   ```
   https://sandboxailabs1012.onmicrosoft.com/cloudlabs.ai
   ```

   OR

   ```
   https://sandboxailabs1013.onmicrosoft.com/cloudlabs.ai
   ```

   ![](./media/new9.png)  

>**Congratulations** on completing the Task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.

<validation step="816628f0-c104-4a40-ae80-e93914f85b5d" />

## Task 5: Create a Lakehouse

1. Go to **Workspaces (1)** from the left navigation and select the **FabricMigrationLab<inject key="DeploymentID" enableCopy="false"/> (2)** workspace to open and access its resources.

   ![](./media/new10.png)

2. Click the **+ New item** button.

   ![](./media/image42.png)

3. Click the **Lakehouse** tile.

   ![](./media/image43.png)

4. Enter a name for the Lakehouse **SynapseMigrationLakehouse (1)**, optionally leave **Lakehouse schemas (2)** unchecked, and click **Create (3)** to provision the Lakehouse.

   ![](./media/image44.png)

   ![](./media/image45.png)

## Task 6: Create Shortcut in the Files Section

1. In the **Lakehouse**, click on **ellipsis (...)** next to Files, then select the **New Shortcut (2)** from the list menu.

   ![](./media/new29.png)

2. From the list of external sources, select **Azure Data Lake Storage Gen2** to create a new shortcut.

   ![](./media/image47.png)

3. Select **New connection (1)**, enter the **ADLS Gen2 URL (2)** that you copied in **Task 3 step 16**, and then click **Next (3)** to proceed with creating the shortcut.

   ![](./media/new11.png)

4. Select **synapsefile<inject key="DeploymentID" enableCopy="false"/> (1)** and click **Next (2).**

   ![](./media/new12.png)

5. Click **Create.**

   ![](./media/image50.png)

6. Expand the **Files (1)** section, click on the **ellipsis (...) (2)**, select Refresh . Now  you will be able to see **synapsefile<inject key="DeploymentID" enableCopy="false"/> (3)** folder created.

   ![](./media/new30.png)

7. Select the **synapsefile** folder to review the files.

   ![](./media/image51.png)

   ![](./media/image52.png)

8. In the **Lakehouse** page, click **Open notebook** in the command bar and select **New notebook**.

   ![](./media/image53.png)

9. Replace the code in the cell with the following code, then click **▷ Run cell (2)** to review the output. Replace **DimCustomer** with the **ABFSS URL (1)** that you copied from **Task 3, Step 12**.

   ```python
   df = spark.read.format("csv").load("abfss://synapsefile<inject key="DeploymentID" enableCopy="false"/>@fabricsynapsegen2<inject key="DeploymentID" enableCopy="false"/>.dfs.core.windows.net/FabricMigration/DimCustomer.csv")
   ```

   ![](./media/image54.png)

10. Click the **+ Code** icon below the cell output to add a new code cell, enter the following code, and click **▷ Run cell**.
    
    ```python
    df.write.format("delta").mode("overwrite").save("Tables/Customer")
    ```

    ![](./media/image55.png)

11. To validate the created tables, refresh the **Tables (1)** section in the **Explorer (2)** panel until all tables appear.

    ![](./media/image56.png)

    ![](./media/image57.png)

>**Congratulations** on completing the Task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.

<validation step="d7f320f0-b962-4df4-8192-dd9e354ddbf6" />  

## Task 7: Rebuild Synapse Pipelines in Microsoft Fabric

1. Go to **Workspaces (1)** from the left navigation and select the **FabricMigrationLab<inject key="DeploymentID" enableCopy="false"/> (2)** workspace to open and access its resources.

   ![](./media/new10.png)

2. Click **+ New item (1)** and select **Pipeline (2)**.

   ![](./media/image59.png)

3. Enter **Migrate_Pipeline (1)** as the pipeline name and click **Create (2)**.

   ![](./media/image60.png)

   ![](./media/image61.png)

4. On newly created pipeline, select **Copy data (1)** dropdown and choose **Add copy data activity (2)** option.

   ![](./media/image62.png)

5. With the **copy data** being selected, navigate to **Source** tab.

   ![](./media/image63.png)

6. Select the **Connection** dropdown **(1)** and select **Browse all (2)** option.

   ![](./media/image64.png)

7. Select **+ New** from the left pane.

   ![](./media/image65.png)

8. From the data source options, select **Azure Data Lake Storage Gen2** to begin the connection setup.

   ![](./media/image66.png)

9. Select **New connection**, enter the **ADLS Gen2 URL (1)**, and then click **Connect (2)** to proceed with creating the shortcut.

   ![](./media/image67.png)

   > **Note:** To get the URL, navigate to **Synapse workspace Overview (1)** page, copy **Primary ADLS Gen2 account URL (2)**.
    
   ![](./media/new13.png)

10. In the **Source** tab of the Copy Data activity, click the **Browse** button to select the folder containing the source files.

    ![](./media/image68.png)

11. On the Browse pane, navigate to the **Root folder > synapsefile2181425 > FabricMigration (1)** path and select the **DimDate.csv (2)** file and click **OK (3)**.

    ![](./media/new14.png)

12. In the **Source** tab, select **DelimitedText** as the File format.

    ![](./media/new15.png)    

13. Navigate to the **Destination (1)** tab, for Connection, select **Browse all (2)** from the drop-down.

    ![](./media/image70.png)

14. In the destination selection window, select **OneLake catalog (1)** from the left pane and then select **synapseMigrationLakehouse (2)**.

    ![](./media/image71.png)

15. Under the **Destination** tab, Click on **+ New** for **Table**.

    ![](./media/new16.png)

16. Enter the table name as **dim_Date (1)** and click **Create (2)**.

    ![](./media/image74.png)

17. Click **Run** to execute the copy activity.

    ![](./media/image75.png)

18. Click **Save and run** to save and execute the pipeline.

    ![](./media/image76.png)

    ![](./media/image77.png)

19. After the pipeline completion is successful. Open the **FabricMigrationLab<inject key="DeploymentID" enableCopy="false"/> (1)** workspace and select the **SynapseMigrationLakehouse (2)** to access and work with the created Lakehouse.

    ![](./media/image78.png)

20. Expand **Tables (1)** in the Lakehouse and verify the **dim_Date (1)** table is loaded.

    ![](./media/new17.png)

## Review

In this lab, you successfully migrated data and basic pipeline functionality from Azure Synapse Analytics to Microsoft Fabric. You created and configured necessary resources in both platforms, accessed and transformed data using Lakehouse and notebooks, and rebuilt pipelines to enable data movement within Fabric.

This lab demonstrates how Microsoft Fabric simplifies data integration and analytics by providing a unified platform, making it easier to modernize existing Synapse-based solutions.
