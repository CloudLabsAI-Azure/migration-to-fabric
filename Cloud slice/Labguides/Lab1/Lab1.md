## Lab 1: Migrate data from Azure Synapse Analytics to Fabric Data Warehouse

### Estimated Duration: 120 Minutes

## Overview

This lab provides a comprehensive, hands‑on experience to help you
understand and execute the end‑to‑end process of migrating data and
metadata from **Azure Synapse Analytics dedicated SQL pools** to the
**Microsoft Fabric Data Warehouse**. As organizations modernize their
analytics platforms, Microsoft Fabric offers a unified, scalable, and
fully integrated environment that simplifies data engineering,
warehousing, real‑time analytics, and business intelligence.

Throughout this lab, you will create and configure Azure Synapse
components, ingest sample datasets, integrate them through pipelines,
set up a new Fabric workspace, and use the **Fabric Migration
Assistant** to seamlessly migrate metadata and data into the Fabric Data
Warehouse. You will also learn to reroute dependent analytical tools and
processes to the migrated environment, ensuring business continuity.

## Objectives

By the end of this lab, you will be able to:

- Task 1: Create a Synapse workspace in the Azure portal
- Task 2: Create a dedicated SQL pool
- Task 3: Place sample data into the primary storage account
- Task 4: Integrate with pipelines
- Task 5: Create a Fabric workspace
- Task 6: Copy metadata
- Task 7: Fix problems using Migration Assistant
- Task 8: Copy data using Migration Assistant
- Task 9: Reroute connections

## Task 1: Create a Synapse workspace in the Azure portal

1. In the search bar enter **Synapse** and select **Azure Synapse Analytics.** Search for **Synapse Analytics**.

    ![](./media/image5.png)   

1. Click **Create**.

    ![](./media/image6.png)

1. Enter below details to create resource group and then click
    on **OK**.

    - **Subscription**: Select the default subscription
    - **Resource Group**: Select the assigned Resource group
    - **Managed Resource group:**  Leave this blank.
    - **Workspace name**: fabric-synapse<inject key="DeploymentID" enableCopy="false"/>
    - **Region**: Select the region of your resource group

        ![](./media/image7.png)

    - **Select Data Lake Storage Gen2 account:** From subscription
    - **Account name:** Create
    New: **fabricsynapsegen2<inject key="DeploymentID" enableCopy="false"/>**
    - Click **OK**

        ![](./media/image8.png)

    - **File System Name**: Create
    New: **synapsefile<inject key="DeploymentID" enableCopy="false"/>**
    - Click **OK**.
    - Enter below details and then click on **Next: Security >**.

        ![](./media/image9.png)

1. Configure the **Security** settings by selecting **both Microsoft
    
    - **SQL Server admin login:** sqladmin
    - **SQL Password**: password321!
    - Click **Review + create**

    ![](./media/image11.png)

1.  In the **Review + submit** tab, once the validation is passed, click on the **Create** button.

    ![](./media/image12.png)

1. This deployment may take a few minutes.

    ![](./media/image13.png)

1. Click on **Go to resource group** button.

    ![](./media/image14.png)

1. Click on your **Synapse workspace**.

    ![](./media/image15.png)

## Task 2: Create a dedicated SQL pool

1. In the **Open Synapse Studio** dialog, click **Open** to launch Azure Synapse Studio.

    ![](./media/image16.png)

1. In Synapse Studio, select **Manage** (left pane).

    ![](./media/image17.png)

1. In Synapse Studio, on the left-side pane, select **Manage** \> **SQL pools** under **Analytics pools** and then click on **New**

    ![](./media/image18.png)

1. **Configure SQL Pool**

    - **SQL pool name:** **sql dedicated pool**
    - **Performance level:** Choose **DW100c**
    - Click **Review + Create → Create**.

        ![](./media/image19.png)

1. In the **Review + submit** tab, once the Validation is Passed, click on the **Create** button.

    ![](./media/image20.png)

    ![](./media/image21.png)

1. Go to **Manage → SQL pools** and confirm the Dedicated SQL Pool shows **Online**

    ![](./media/image22.png)

1. Return to the **Azure portal**. In the **Overview** section of the Synapse workspace, copy the **Dedicated SQL endpoint** and **Dedicated SQL pool** details, and save them in a Notepad for later use.

    ![](./media/image23.png)

## Task 3: Place sample data into the primary storage account

1. Navigate to **Synapse Studio**, then open the **Data Hub** and select the **Linked** section.

    ![](./media/image24.png)

1. Under the **Azure Data Lake Storage Gen2** category, locate the item with your workspace name, such as **fabric-synapse<inject key="DeploymentID" enableCopy="false"/> (Primary — asastorageaccount01 (your storage account))**.

    ![](./media/image25.png)

1. Select the container named **synapsefile<inject key="DeploymentID" enableCopy="false"/> (Primary)**.

    ![](./media/image26.png)

1. Select **Upload**, browse to `C:\LabFiles\lab file`, choose **NYCTripSmall.parquet**, and then click **Upload** to complete the process.

    ![](./media/image27.png)

    ![](./media/image28.png)

    ![](./media/image29.png)

1. Click on **Upload** button.

    ![](./media/image30.png)

1. The **paraquest** file uploaded successfully.

1. Repeat the same steps to upload the remaining **DimDate.csv,
    Dimension_Employee.csv** files
    
    ![](./media/image32.png)

    ![](./media/image33.png)

1. Right-click on **NYCTripSmall.parquet** and select **Properties**.

    ![](./media/image40.png)

1. In the **Properties** page, copy the URL and ABFSS path, then save them in Notepad for later use.

    ![](./media/image41.png)

## Task 4: Integrate with pipelines

1. **In Synapse Studio,** click on **Integrate hub**

    ![](./media/image42.png)

1. Select **+** and select **Pipeline** to create a new pipeline.

    ![](./media/image43.png)

1. Under **Activities**, expand the **Move and transform** category, then drag a **Copy data** activity onto the canvas.

    ![](./media/image44.png)

    ![](./media/image45.png)

1. On the **Source** page, select **+** **New**

    ![](./media/image46.png)

    ![](./media/image47.png)

1. In **New integration dataset**, select the **Azure Data Lake Storage
    Gen2** tile.

    ![](./media/image48.png)

1. Select the **DelimitedText** tile and click **Continue**.

    ![](./media/image49.png)

1. In **Set properties**, click **+ New** under **Linked service** to
    create a new connection to the storage account

    ![](./media/image50.png)

1.  In **New linked service**, provide a name, select your **Azure
    subscription** and **Storage account**, then click **Test
    connection** and **Create** after a successful validation.

    ![](./media/image51.png)

1. Browse and select the **folder**, then choose the required file.

    ![](./media/image52.png)

1. Select **synapsefile<inject key="DeploymentID" enableCopy="false"/>**, choose the **DimDate.csv** file, and then click **OK**.

    ![](./media/image53a.png)

    ![](./media/image53.png)

1. In the Synapse pipeline, go to the **Sink** tab within the **Copy Data** activity to configure the destination settings for the dataset.

    ![](./media/image54.png)

1. On the **Source** page, select **+** **New**

    ![](./media/image55.png)

1. In **New integration dataset**, select the **Azure Synapse
    Analytics** tile.

    ![](./media/image56.png)

1. In the **New Linked Service** setup, configure the connection to the Synapse SQL dedicated pool by selecting the **Azure subscription**, **server name**, and **database name**. Enter the **User name** as **sqladmin** and the **password** as **password321!**, then click **Test connection**. If the test is successful, click **Create**.

    ![](./media/image58.png)

1. In the **Set Properties** window, specify the table name as **dbo.fabric**, set the **Import schema** option to **None**, and then click **OK** to confirm the configuration.

    ![](./media/image59.png)

1. Select the **Auto create table** option in the Sink settings and then click on the **Mapping** tab to proceed.

    ![](./media/image60.png)

1. Click **Import schemas** to automatically detect and load the file structure.

    ![](./media/image61.png)

1. Click **Debug**

    ![](./media/image62.png)

    ![](./media/image63.png)

1. Navigate back to the **Copy data** activity, click the **Source** tab, and select **Open** to view the source dataset configuration.

    ![](./media/image64.png)

1. Click **Browse** folder

    ![](./media/image65.png)

1. Select the **Dimension_Employee.csv** file and click **OK**.

    ![](./media/image66.png)

1. Click on **Pipeline 1** tab.

    ![](./media/image67.png)

1. Click on the **Sink** tab and then select **Open** to view the sink dataset configuration.

    ![](./media/image68.png)

1. In the **Sink** dataset configuration, enter the schema as **dbo** and specify the table name as **fabric_employee**, then return to **Pipeline 1** to continue the setup.

    ![](./media/image69.png)

1. Go to the **Mapping** tab and click **Import schema**.

    ![](./media/image70.png)

1. Click **Debug** to test and validate the pipeline execution.

    ![](./media/image72.png)

    ![](./media/image73.png)

1. **In Synapse Studio,** click on the **Data hub**.

    ![](./media/image74.png)

1. Verify that the **files** deployed successfully.

    ![](./media/image75.png)

## Task 5: Create a Fabric workspace

1. Open your browser and navigate to: **https://app.fabric.microsoft.com/** then press **Enter**.

1. On the **Fabric Home** page click on **+ New Workspaces** as shown in the image below.

    ![](./media/image76.png)

1. On the **Create a workspace** pane that appears to the right, enter the following details, and then click **Apply**.

    | Field                   | Value                                                                 |
    |------------------------|-----------------------------------------------------------------------|
    | Name                   | **Fabric_Migration<inject key="DeploymentID" enableCopy="false"/>**  |
    | Advanced               | Select Fabric                                                         |
    | Default storage format | Small dataset storage format                                           |

    ![](./media/image77.png)

    ![](./media/image78.png)

1. The Workspace is now created.

    ![](./media/image79.png)

## Task 6: Copy metadata

1. In your Fabric workspace, select the **Migrate** button on the item
    action deck.

    ![](./media/image80.png)

1. In the **Migrate to Fabric** source menu, under **Migrate to
    Fabric**, select the **Azure Synapse Analytics dedicated SQL
    pool** tile.

    ![](./media/image81.png)

1. On the **Choose your method** screen, keep the default option **Upload a file with the source metadata**, then click **Next**.

    ![](./media/image82.png)

1. Click **Choose file**

    ![](./media/image83.png)

1. Browse to `C:\LabFiles\lab file`, select the **AdventureWorks.DACPAC** file, and click **Open**.

    ![](./media/image84.png)

1. When the upload is complete, select **Next**.

    ![](./media/image85.png)

1. On the **Set the destination** page, enter the **Fabric workspace name** and specify a new warehouse name as **Migration_Warehouse**, then click **Next**.

    ![](./media/image86.png)

1.  Review your inputs and select **Migrate**. A new warehouse item is
    created and the metadata migration begins.

    ![](./media/image87.png)

    > **Note:** When using the Migration Assistant, the new warehouse has **case insensitive collation**, regardless of the [**default warehouse collation setting**](https://learn.microsoft.com/en-us/fabric/data-warehouse/collation).

    ![](./media/image88.png)

1. The Migration Assistant translates the T-SQL metadata into a format supported by the Fabric data warehouse. Once the metadata migration is complete, the Migration Assistant opens automatically. You can access it at any time by selecting the **Migration** button on the **Home** tab of the warehouse ribbon.

    ![](./media/image89.png)

1.  Review the metadata migration summary in the Migration Assistant.
    You'll see the count of migrated objects and the objects that need
    to be fixed before they can be migrated.

    ![](./media/image90.png)

1.  Select **Show migrated objects** to expand the section and see a
    list of objects that have been successfully migrated to your Fabric
    warehouse.

    ![](./media/image90.png)

    ![](./media/image91.png)

    > The **State** column indicates whether an object’s metadata was modified during translation to ensure compatibility with the Fabric Warehouse. For example, certain column data types or T-SQL constructs may be automatically converted to supported equivalents. The **Details** column provides additional information about the specific adjustments made to each object.

1. Select any object to see the adjustments that were made during
    migration.

1. Open the metadata migration summary in full screen view for better
    readability. Apply filters to view specific object types.

    ![](./media/image92.png)

    ![](./media/image93.png)

    ![](./media/image94.png)

1. Optionally, select the **Export** menu to download a migration summary as an Excel file or a CSV.

    - The downloaded Excel file is a well-structured workbook containing two worksheets: **Migrated Objects** and **Objects To Fix**. It is MIP-compliant and adheres to your organization’s sensitivity labeling policies.
    - The CSV is lightweight and tool-friendly.

    ![](./media/image95.png)

1. Select the exported file to review the **CSV** data.

    ![](./media/image96.png)

## Task 7: Fix problems using Migration Assistant

1. Select the **Fix problems** step in the Migration Assistant to see the scripts that failed to migrate.

    ![](./media/image98.png)

1. Review the comments at the beginning of the script to understand the adjustments that were made.

    ![](./media/image100.png)

1. Review and fix the broken scripts using the error information and documentation.

1. To use Copilot for AI-powered assistance in resolving errors, select **Fix query errors** in the **Suggested action** section. Copilot will update the script with recommended changes. Since it is AI-driven, review the suggestions carefully and make any necessary adjustments.

    ![](./media/image101.png)

1. Select **Run** to validate and create the object.

    ![](./media/image102.png)

1. The next script to be fixed opens.

    ![](./media/image103.png)

1. Continue to fix the rest of the scripts. You can choose to skip fixing scripts that you don't need during this step.

1. Once all required metadata is ready for migration, click the **Back** button in the **Fix problems** pane to return to the top-level view of the Migration Assistant. Then, mark the **2. Fix problems** step as complete in the Migration Assistant.

    ![](./media/image104.png)

## Task 8: Copy data using Migration Assistant

1. Select the **Copy data** step in the Migration Assistant.

    ![](./media/image105.png)

1. Select the **Use a copy job** button.

    ![](./media/image106.png)

1. Assign the name **migrate_copyjob** to the new job, then click **Create**.

    ![](./media/image107.png)

1. On the **Connect to data source** page, provide the connection credentials for the source **Azure Synapse Analytics (SQL DW)** warehouse, then click **Next**.

    ![](./media/image108.png)

1. On the **Connection settings** tab, enter the required details below and click **Next**.

    | Field    | Value                                              |
    |----------|----------------------------------------------------|
    | Server   | Dedicated SQL server URL (from Task 2 → Step 7)    |
    | Database | fabric-synapse<inject key="DeploymentID" enableCopy="false"/>                  |
    | Username | sqladmin                                           |
    | Password | Password321!                                       |

    ![](./media/image109.png)

1. On the **Choose data** page, select the tables you want to migrate. The object metadata should already exist in the target warehouse, then click **Next**.

    ![](./media/image110.png)

1. Click **Next**.

    ![](./media/image111.png)

1. On the **Map to destination** page, configure each table's column mappings, then click **Next**.

    ![](./media/image112.png)

1. Review the job summary and click **Save + Run**.

    ![](./media/image113.png)

1. Once the copy job completes, check the **Copy data** step in the Migration Assistant. Select the back button at the top to return to the top-level view of the Migration Assistant.

    ![](./media/image114.png)

    ![](./media/image115.png)

## Task 9: Reroute connections

In the final step, the data loading/reporting platforms that are
connected to your source need to be reconnected to your new Fabric
warehouse.

1. Identify connections on your existing source warehouse.

    - For example, in Azure Synapse Analytics dedicated SQL pools, you can find session information including the source application, connected user, connection source, and whether it's using Microsoft Entra ID or SQL authentication:

1. Navigate to your Synapse workspace.

1. In Synapse Studio, navigate to the **Develop** hub, click the **+** button to add a new resource, then select **SQL script**.

    ![](./media/image116.png)

1. Ensure that the SQL script is connected to the **SQL dedicated pool** by selecting it from both the **Connect to** dropdown and the **Use database** dropdown, as shown in the image.

    ![](./media/image117.png)

1. Enter the following code into the editor and click **Run** to execute it:

    **SQL**
    ```
    SELECT DISTINCT CASE 
            WHEN len(tt) = 0
                THEN app_name
            ELSE tt
            END AS application_name
        ,login_name
        ,ip_address
    FROM (
        SELECT DISTINCT app_name
            ,substring(client_id, 0, CHARINDEX(':', ISNULL(client_id, '0.0.0.0:123'))) AS ip_address
            ,login_name
            ,isnull(substring(app_name, 0, CHARINDEX('-', ISNULL(app_name, '-'))), 'h') AS tt
        FROM sys.dm_pdw_exec_sessions
        ) AS a;
    ```

    ![](./media/image118.png)

1. After running the SQL script, the query results are displayed in table view, showing the application name, login name, and IP address returned by the query.

    ![](./media/image119.png)

1. Update the connections for data loading (ETL/ELT) platforms to point to your Fabric warehouse:

    - For Power BI and Fabric pipelines, use the [List Connections REST API](https://learn.microsoft.com/en-us/rest/api/fabric/core/connections/list-connections?tabs=HTTP) to find connections to your old data source, the Azure Synapse Analytics dedicated SQL pool.

    - Update the connections to the new Fabric data warehouse using **Manage Connections and Gateways** under the **Settings** gear.

1. Once complete, check the **Reroute connections** step in the Migration Assistant.

    ![](./media/image120.png)

    Congratulations! You're now ready to start using the warehouse.

    ![](./media/image121.png)

    ![](./media/image122.png)

    > **Important**: A dedicated SQL pool consumes billable resources as long
    as it's active. Please ensure that you pause the pool even if you are
    taking a small break from the lab execution. If the pool is in active
    state for more than 40 minutes and is not being used, your account might
    get disabled

## Summary

In this lab, you successfully performed a full migration journey from
**Azure Synapse Analytics dedicated SQL pools** to **Microsoft Fabric
Data Warehouse**. You began by configuring Azure and Synapse resources,
uploading datasets, and creating data pipelines. You then created a
Microsoft Fabric workspace and used the **Migration Assistant** to
translate and migrate metadata, fix compatibility issues, and copy data
into the new warehouse.

Finally, you learned how to reroute external applications—such as Power
BI, pipelines, and ETL tools—to the Fabric environment to ensure smooth
operational transition. Completion of this lab equips you with essential
skills needed for modernizing analytical workloads using Microsoft
Fabric.
