**Introduction**

In modern data engineering solutions, organizations are increasingly
adopting unified analytics platforms like Microsoft Fabric to streamline
data integration and processing. This lab provides hands-on experience
in migrating existing Azure Data Factory (ADF) pipelines to Microsoft
Fabric Data Factory. You will explore how to deploy an ADF instance, run
and monitor pipelines, and then seamlessly migrate those pipelines into
a Fabric workspace. By the end of this lab, you will understand the
migration workflow, validate pipeline functionality in Fabric, and
leverage OneLake for data storage and processing.

**Objectives**

By completing this lab, you will be able to:

- Create and configure an Azure Data Factory instance

- Review and execute an existing pipeline in ADF

- Monitor pipeline execution and validate data movement

- Set up a Microsoft Fabric workspace and Lakehouse

- Migrate Azure Data Factory pipelines to Fabric Data Factory

- Configure source and destination connections in Fabric

- Run and validate the migrated pipeline in Fabric

- Schedule pipelines for automated execution

- Clean up resources after completion

## Task 1: Create Azure Data Factory

1.  Open a browser go to +++ sign in with your cloud slice account
    below.

> Username: <+++@lab.CloudPortalCredential>(User1).Username+++
>
> Password: \<<+++@lab.CloudPortalCredential>(User1).Password\>+++

![A screenshot of a computer Description automatically
generated](./media/image1.png)

![A screenshot of a login box Description automatically
generated](./media/image2.png)

2.  Open a browser go to
    +++https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.datafactory%2Fdata-factory-get-started%2Fazuredeploy.json
    +++

3.  Select the appropriate **subscription** and **resource group**,
    choose the region as *(US) **Central US***, and then click
    ***Review + create*** to proceed with deploying Azure Data Factory.

![](./media/image3.png)

4.  In the **Review + submit** tab, once the Validation is Passed, click
    on the **Create** button.

> ![](./media/image4.png)

![](./media/image5.png)

5.  After the deployment is completed, click on **Go to resource
    group** button.

![](./media/image6.png)

![](./media/image7.png)

## Task 2: Review deployed resources

1.  Make sure the below resource got deployed successfully

- Data factory

- Storage account

![](./media/image8.png)

2.  Select **Azure Data factory**

![](./media/image9.png)

3.  Click on **Launch studio** to launch your Azure Data factory.

![](./media/image10.png)

> ![](./media/image11.png)

4.  In Azure Data Factory Studio, select the **Author** tab ![Author
    tab](./media/image12.png).

> ![](./media/image13.png)

5.  Select the pipeline that the template created.

> ![](./media/image14.png)

6.  Check the source data by selecting **Open**.

> ![](./media/image15.png)

![](./media/image16.png)

7.  In the source dataset, select **Open** to view the input file
    created for the demo.

![](./media/image17.png)

8.  In the source dataset, select **Browse** to view the input file
    created

> ![](./media/image18.png)

9.  Note the moviesDB2.csv file, which was already uploaded into the
    input folder.

> ![Screenshot of the contents of the input folder, showing the input
> file used in the demo.](./media/image19.png)

## Task 3: Trigger the demo pipeline to run

1.  Once the Copy Data activity is configured, click **Add trigger** at
    the top to set up a trigger for running the pipeline

> ![](./media/image20.png)

2.  Select **Trigger now**

![](./media/image21.png)

3.  Click on **OK**

![](./media/image22.png)

![](./media/image23.png)

![](./media/image24.png)

4.  After the pipeline completes successfully, click **View pipeline
    run** in the notification panel to review the execution details

![](./media/image25.png)

![](./media/image26.png)

## Task 4 :Monitor the pipeline

1.  Select the **Monitor** tab ![Monitor tab](./media/image27.png). This
    tab provides an overview of your pipeline runs, including the start
    time and status.

2.  In this quickstart, the pipeline has only one activity type: **Copy
    data**. Select the pipeline name to view the details of the copy
    activity's run results.

> ![](./media/image28.png)

3.  Select the **Details** icon to display the detailed copy process. In
    the results, the **Data read** and **Data written** sizes are the
    same, and one file was read and written. This information proves
    that all the data was successfully copied to the destination.

> ![](./media/image29.png)

## Task 5: Create a workspace

Before working with data in Fabric, create a workspace with the Fabric
trial enabled.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: +++<https://app.fabric.microsoft.com/+++> then
    press the **Enter** button.

**Note**: If you are directed to Microsoft Fabric Home page, then skip
steps from \#2 to \#4.

2.  In the **Microsoft Fabric** window, enter your credentials, and
    click on the **Submit** button.

![](./media/image30.png)

3.  Then, In the **Microsoft** window enter the password and click on
    the **Sign in** button.

![A login screen with a red box and blue text AI-generated content may
be incorrect.](./media/image31.png)

4.  In **Stay signed in?** window, click on the **Yes** button.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image32.png)

![](./media/image33.png)

5.  On the Microsoft **Fabric Home Page**, select **New
    workspace** option.

![](./media/image34.png)

6.  In the **Create a workspace** tab, enter the following details and
    click on the **Apply** button.

[TABLE]

> ![](./media/image35.png)
>
> ![](./media/image36.png)

5.  Wait for the deployment to complete. It’ll take approximately 2-3
    minutes.

![](./media/image37.png)

## Task 6: Create a lakehouse and Ingest sample data

1.  In the **Data-FactoryXX** workspace page, navigate and click
    on **+New item**  button

> ![](./media/image38.png)

2.  Click on the "**Lakehouse**" tile.

> ![](./media/image39.png)

3.  In the **New lakehouse** dialog box, enter
    +++**DataFactory_MigrationLakehouse+++** in the **Name** field,
    click on the **Create** button and open the new lakehouse.

> ![](./media/image40.png)

![](./media/image41.png)

## Task 7: Bring your Azure Data Factory to Fabric

1.  In Data Factory Studio, navigate to the **Manage** hub

![](./media/image42.png)

2.  From the **Manage** section, navigate to **Linked services** and
    then click on **Migrate to Fabric (Preview)** to begin the migration
    process

![](./media/image43.png)

3.  On the Plan ahead with Fabric screen, click **Get started** to begin
    the migration workflow.

> ![](./media/image44.png)

4.  Review the migration assessment summary, and once the pipeline
    appears with a Ready status, click **Next** to proceed with the
    migration

> ![](./media/image45.png)

5.  Select the Fabric workspace name from the dropdown menu and click
    *Mount* to attach the Azure Data Factory to the chosen Fabric
    workspace

> ![](./media/image46.png)

6.  Once the mounting process completes successfully, click **Continue
    in Fabric** to proceed with the migration steps in the Fabric
    workspace.

![](./media/image47.png)

> ![](./media/image48.png)

7.  Click **Migrate to Fabric (Preview)**

> ![](./media/image49.png)

8.  Verify that the pipeline is marked as Ready in the migration
    summary, then click **Review connections** to continue with the
    migration process

> ![](./media/image50.png)

9.  Click **Confirm**

> ![](./media/image51.png)

10. Select the folder where you want the migrated items to be stored,
    and then click **Confirm** to proceed with the migration

> ![](./media/image52.png)

11. After the migration completes and the status shows *Success*, click
    **Close** to exit the migration results window.

> ![](./media/image53.png)
>
> ![](./media/image54.png)

12. Select **Pipeline**

![](./media/image55.png)

![](./media/image56.png)

13. In the Source settings of the pipeline, review that the **Movies**
    file is correctly referenced in the file path.

> ![](./media/image57.png)

14. Navigate to **Source** tab. Select the **Connection** dropdown and
    select **Browse all** option.

> ![](./media/image58.png)

15. On choose a destination window, select **OneLake catalog** from the
    left pane and select the **DataFactory_MigrationLakehouse**.

> ![](./media/image59.png)

16. In the Destination settings, select the **Files** option and set the
    file format to **Binary** before configuring the remaining
    parameters.

> ![](./media/image60.png)

8.  Click on **Run** to run the copy data.

> ![](./media/image61.png)

17. Click on **Save and run** button so that pipeline will be save and
    run.

> ![](./media/image62.png)

![](./media/image63.png)

18. After the successful execution of the pipeline, go to your Lakehouse

![](./media/image64.png)

19. Click and select refresh on the **Files**. The file appears.

![](./media/image65.png)

![](./media/image66.png)

## Task 3: Schedule the Pipeline

1.  Now, click on **workspace** on the left-sided navigation pane and
    select **pipeline**

![](./media/image67.png)

2.  Click **Schedule**

> ![](./media/image68.png)

3.  Select **+ Add schedule** and configure the schedule as required
    then select **Save** and close the **Schedule** panel.

> **Note**: The example here schedules the pipeline to execute daily at
> 8:00 PM until the end of the year.

![](./media/image69.png)

> ![](./media/image70.png)

4.  From the workspace view, select the migrated Azure Data Factory
    resource to continue configuring and validating the pipeline

![](./media/image71.png)

![](./media/image72.png)

![](./media/image73.png)

## Task 4: Delete the Resources

1.  To delete resources , type **Resource groups** in the Azure portal
    search bar, navigate and click on **Resource
    groups** under **Services**.

> ![](./media/image74.png)

2.  In the Resource groups page, select your resource group.

3.  On the **Resource Group** home page, select all resources and then
    click **Delete**.

![](./media/image75.png)

4.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “delete” to confirm deletion** field, then click
    on the **Delete** button

![](./media/image76.png)

![](./media/image77.png)

5.  Go to your +++ https://app.fabric.microsoft.com/+++ Microsoft Fabric
    workspace

6.  Select the **...** option under the workspace name and
    select **Workspace settings**.

> ![](./media/image78.png)

7.  Select **General** and **Remove this workspace.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image79.png)

20. Click on **Delete** in the warning that pops up.

> ![](./media/image80.png)

![](./media/image81.png)

![](./media/image82.png)

![](./media/image83.png)

**Summary**

In this lab, you successfully deployed an Azure Data Factory instance
and executed a sample data pipeline. You then created a Microsoft Fabric
workspace and Lakehouse to serve as the destination environment. Using
the built-in migration feature, you transferred the ADF pipeline to
Fabric Data Factory, validated its configuration, and executed it to
ensure successful data ingestion into OneLake. Finally, you explored
scheduling capabilities and learned how to manage and clean up
resources. This lab demonstrates how organizations can modernize their
data integration workflows by transitioning from Azure Data Factory to
Microsoft Fabric.
