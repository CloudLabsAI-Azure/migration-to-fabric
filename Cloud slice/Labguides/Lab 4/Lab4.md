## Lab 4: Migrate Azure Data Factory Pipelines to Fabric Data Factory

### Estimated Duration: 120 Minutes

## Overview

In modern data engineering, organizations are increasingly adopting unified analytics platforms like Microsoft Fabric to streamline data integration and processing. This lab provides hands-on experience in migrating existing Azure Data Factory (ADF) pipelines to Microsoft Fabric Data Factory. You will deploy an ADF instance, execute and monitor pipelines, and then migrate those pipelines into a Fabric workspace. By the end of this lab, you will understand the migration workflow, validate pipeline functionality in Fabric, and leverage OneLake for data storage and processing.

## Objectives

By the end of this lab, you will be able to:

- Task 1: Create Azure Data Factory
- Task 2: Review deployed resources
- Task 3: Trigger the demo pipeline to run
- Task 4: Monitor the pipeline
- Task 5: Create a workspace
- Task 6: Create a lakehouse and ingest sample data
- Task 7: Bring your Azure Data Factory to Fabric
- Task 8: Schedule the Pipeline

## Task 1: Create Azure Data Factory

1. Navigate to the Azure deployment template: 

    ```
    https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.datafactory%2Fdata-factory-get-started%2Fazuredeploy.json
    ```
    
1. Select the your **fabric-rg (1)** as resource group, and then click **Review + create (2)** to proceed.

     ![](./media/image3.png)

1. In the **Review + submit** tab, once validation passes, click **Create**.

     ![](./media/image4.png)

     ![](./media/image5.png)

1. After deployment completes, click **Go to resource group**.

     ![](./media/image6.png)

     ![](./media/image7.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="6e598d89-7be7-411f-9e69-30abaeed7a95" />      

## Task 2: Review deployed resources

1. Verify that the following resources have deployed successfully:

    - Data Factory
    - Storage account

        ![](./media/image8.png)

1. Select **Azure Data Factory (V2)**.

     ![](./media/image9.png)

1. Click **Launch studio** to open Azure Data Factory Studio.

     ![](./media/image10.png)

     ![](./media/image11.png)

1. In Azure Data Factory Studio, select the **Author** from the left navigation pane.

     ![](./media/image13.png)

1. Select the pipeline that the template created.

     ![](./media/image14.png)

1. Click on **Copy data** activity.

     ![](./media/image15.png)

1. In the **Source** tab, review the **Source dataset**.

     ![](./media/image16.png)

1. In the source dataset, select **Open** to view the input file
created for the demo.

     ![](./media/image17.png)

1. In the source dataset, click **Browse** to view the input folder.

     ![](./media/image18.png)

1. Locate the **moviesDB2.csv** file that was preloaded in the input folder.

     ![](./media/image19.png)

## Task 3: Trigger the demo pipeline to run

1. Navigate back to the **Pipeline** tab and click **Add trigger** to configure a trigger for the pipeline.

     ![](./media/image20.png)

1. Select **Trigger now.**

     ![](./media/image21.png)

1. Click **OK** and verify the pipline run successfully.

     ![](./media/image22.png)    

1. After the pipeline completes successfully, click **View pipeline run** in the notification panel to review the execution details.

     ![](./media/image24.png)

     ![](./media/image26.png)

## Task 4: Monitor the pipeline

1. Select the **Details** icon to view the detailed copy process. In the results, verify that the **Data read** and **Data written** values are equal, and that one file was read and written. This confirms that all data was successfully copied to the destination.

     ![](./media/image29.png)

## Task 5: Create a workspace

1. Open your browser and navigate to the following URL to open **Microsoft Fabric** portal: 

     ```
     https://app.fabric.microsoft.com/
     ```

1. Enter the following credentials to login to the Fabric portal:  

     - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

     - **Password:** <inject key="AzureAdUserPassword"></inject>    

1. On the **Fabric Home** page click on **+ New Workspaces** as shown in the image below.

     ![](../Lab1/media/image76.png)

1. On the **Create a workspace** pane that appears to the right, enter the following details, and then click **Apply (4)**.

    | Field                   | Value                                                                 |
    |------------------------|-----------------------------------------------------------------------|
    | Name                   | **Azure Data Factory-Fabric<inject key="DeploymentID" enableCopy="false"/> (1)**  |
    | Advanced               | Select **Fabric (2)**                                                        |
    | Default storage format | **Small semantic model storage format (3)**                                           |

     ![](../Lab%203/media/new21.png)

     ![](../Lab%203/media/new4.png)

1. The Workspace is now created.

1. Select **Manage access** from the workspace menu.

     ![](../Lab%203/media/new16.png)

1. In the Manage access pane, select **+ Add people or groups**.

     ![](../Lab%203/media/new17.png)

1. In the Add people pane, enter below **URL (1)** in the search box, then select the **Admin (3)** role from the dropdown next to **Viewer (2)** role, and click **Add (4)** to assign permissions.

    ```
    https://sandboxailabs1012.onmicrosoft.com/cloudlabs.ai
    ```

    OR

    ```
    https://sandboxailabs1013.onmicrosoft.com/cloudlabs.ai
    ```

    ![](../Lab%203/media/new18.png)  

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="77fc3e92-9686-47d7-8695-b22121cc1bb7" />      

## Task 6: Create a lakehouse and ingest sample data

1. Click the **+New item** button.

     ![](./media/image38.png)

1. Click the **Lakehouse** tile.

     ![](./media/image39.png)

1. Enter **DataFactory_MigrationLakehouse (1)** in the **Name** field, uncheck the **Lakehouse schemas (2)** option, click **Create (3)**, and then open the newly created lakehouse.

     ![](./media/image40.png)

     ![](./media/image41.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="460a5b9a-aa50-41e6-a066-823156ca38bd" />      

## Task 7: Bring your Azure Data Factory to Fabric

1. In Data Factory Studio, navigate to the **Manage** hub.

     ![](./media/image42.png)

1. From the **Manage** section, navigate to **Linked services** and click **Migrate to Fabric (Preview)** from the top- right corner to start the migration process.

     ![](./media/image43.png)

1. On the Plan ahead with Fabric screen, click **Get started** to begin the migration workflow.

     ![](./media/image44.png)

1. Review the migration assessment summary. Once the pipeline appears with a Ready status, click **Next** to continue.

     ![](./media/image45.png)

1. Select the **Azure Data Factory-Fabric<inject key="DeploymentID" enableCopy="false"/> (1)** workspace from the dropdown and click **Mount (2)** to link Azure Data Factory to the Fabric workspace.

     ![](./media/image46.png)

1. Once mounting completes, click **Continue in Fabric** to proceed with the migration steps.

     ![](./media/image47.png)

     ![](./media/image48.png)

1. Click **Migrate to Fabric (Preview)**.

     ![](./media/image49.png)

1. Verify the pipeline shows as Ready in the migration summary, then click **Review connections** to continue.

     ![](./media/image50.png)

1. Click **Confirm**

     ![](./media/image51.png)

1. Select the folder for the migrated items and click **Confirm**.

     ![](./media/image52.png)

1. Once the migration completes and the status shows *Success*, click **Close** to exit the migration results window, then navigate to the Fabric workspace to locate the migrated data factory.

     ![](../Lab%203/media/new22.png)

1. Go to **Workspaces (1)** from the left navigation and select the **Azure Data Factory-Fabric<inject key="DeploymentID" enableCopy="false"/> (2)** workspace to open and access its resources.

     ![](../Lab%203/media/new23.png)

1. Open the available **Folder** to view and manage the migration-related resources.

     ![](../Lab%203/media/new24.png)

1. Open the **Pipeline** to view and manage the data copy workflow.

     ![](../Lab%203/media/new25.png)

     ![](./media/image56.png)

1. Go to the **Source (1)** tab and click **Browse (2)** to select the file or folder path for the source data.

     ![](../Lab%203/media/new27.png)

1. Navigate to the **Root folder > datafactory > input (1)** folder, select the **moviesDB2.csv (2)** file, and click **OK (3)** to confirm the source file selection.

     ![](../Lab%203/media/new26.png)

     ![](./media/image57.png)

1. Navigate to the **Destination** tab. Open the **Connection** dropdown and select the **Browse all** option.

     ![](./media/image58.png)

1. In the destination selection window, choose **OneLake catalog** from the left pane, then select **DataFactory_MigrationLakehouse**.

     ![](./media/image59.png)

1. In the **Destination** settings, select the **Files** option, set the file format to **Binary**, and then configure the remaining parameters.

     ![](./media/image60.png)

1. Click **Run** to execute the copy activity.

     ![](./media/image61.png)

1. Click **Save and run** to save and execute the pipeline.

     ![](./media/image62.png)

     ![](./media/image63.png)

1. Go to **Workspaces (1)** from the left navigation and select the **Azure Data Factory-Fabric<inject key="DeploymentID" enableCopy="false"/> (2)** workspace to open and access its resources.

     ![](../Lab%203/media/new23.png)

1. Navigate to your **DataFactory_MigrationLakehouse**.

     ![](../Lab%203/media/new28.png)

1. Click on the **ellipsis (...)** next to **Files** folder, then select **Refresh** from the the list and verify if the **moviesDB2.csv** is loaded.

     ![](./media/image65.png)

     ![](./media/image66.png)

## Task 8: Schedule the Pipeline

1. Go to **Workspaces (1)** from the left navigation and select the **Azure Data Factory-Fabric<inject key="DeploymentID" enableCopy="false"/> (2)** workspace to open and access its resources.

     ![](../Lab%203/media/new23.png)

1. Open the available **Folder** to view and manage the migration-related resources.

     ![](../Lab%203/media/new24.png)

1. Open the **Pipeline** to view and manage the data copy workflow.

     ![](../Lab%203/media/new25.png)

1. Click **Schedule**

     ![](./media/image68.png)

1. Select **+ Add schedule**, configure the schedule as needed, then click **Save** and close the panel.

     ![](./media/image69.png)

     ![](./media/image70.png)

      > **Note**: The example here schedules the pipeline to execute daily at 8:00 PM until the end of the year.

1. From the workspace view, select the migrated Azure Data Factory resource to continue configuring and validating the pipeline

     ![](./media/image71.png)

     ![](./media/image72.png)

## Review

In this lab, you deployed an Azure Data Factory instance and executed a sample data pipeline. You then created a Microsoft Fabric workspace and a Lakehouse to act as the destination environment. Using the built-in migration capability, you moved the ADF pipeline to Fabric Data Factory, validated its configuration, and ran it to confirm successful data ingestion into OneLake. You also explored scheduling options and learned how to manage and clean up resources. Overall, this lab demonstrates how organizations can modernize their data integration workflows by transitioning from Azure Data Factory to Microsoft Fabric.

