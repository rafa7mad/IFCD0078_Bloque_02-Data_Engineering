# Manage the semantic model lifecycle

<br>

Analytics teams that publish directly to production without validation or structured deployment risk breaking reports, losing change history, and serving incorrect data. A defined lifecycle process prevents these problems by catching issues before content reaches business users.

In this exercise, you create a lakehouse with sample data, then use SemPy in a Fabric notebook to inspect the semantic model’s structure and validate data quality. When validation reveals missing relationships that produce incorrect DAX results, you use SemPy’s read/write TOM connection to fix the model programmatically. After confirming the model is correct, you create a deployment pipeline with Development and Production stages and promote the validated content from Development to Production. These tasks follow the **Validate** → **Fix** → **Deploy** stages of the semantic model lifecycle.

This lab takes approximately 45 minutes to complete.

>! **Tip**: For related training content, see [Manage the semantic model development lifecycle](https://learn.microsoft.com/training/modules/manage-semantic-model-lifecycle/).

<br>

## Set up the environment

<br>

>! **Note**: You need access to a Fabric paid or trial capacity to complete this exercise. Paid capacities must include Power BI capabilities, or you need a separate Power BI Pro or Premium Per User license. For information about the free Fabric trial, see [Fabric trial](https://aka.ms/fabrictrial).

### Create workspaces

In this task, you create two workspaces for the deployment pipeline stages: Development and Production.

1. Navigate to the [Microsoft Fabric home page](https://app.fabric.microsoft.com/home?experience=fabric) at <br>
    https://app.fabric.microsoft.com/home?experience=fabric in a browser, and sign in with your Fabric credentials.
2. In the menu bar on the left, select **Workspaces** (the icon looks similar to 🗇).
3. Create a new workspace with a name of your choice followed by `-dev` (for example, `SalesLifecycle-dev`), selecting a licensing mode that includes Fabric capacity (*Trial*, *Premium*, or *Fabric*). Remember this base name because you use it for the production workspace and the deployment pipeline.
4. When your new workspace opens, it should be empty.

![011_workspace_0](images/011_workspace_0.JPG)

<br>

5. Repeat the process to create a second workspace with the same base name followed by `-prod` (for example, `SalesLifecycle-prod`).

>! **Note**: Both workspaces must be on Fabric or Premium capacity for deployment pipelines to work.

6. Navigate back to your **-dev** workspace to continue with the exercise.

<br>

### Import a notebook

In this task, you download the lab notebook that contains all the Python code for this exercise.

1. Open a web browser and enter the following URL to download the [21b-manage-semantic-model-lifecycle.ipynb](https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/21b/21b-manage-semantic-model-lifecycle.ipynb) notebook:

> https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/21b/21b-manage-semantic-model-lifecycle.ipynb

2. Save the file to your local **Downloads** folder (or to the VM desktop if you’re in a hosted lab environment).

3. In your workspace, select **Import** and then select **Notebook**.

4. Select **Upload** and browse to the **21b-manage-semantic-model-lifecycle.ipynb** file you downloaded. Select **Open**, then select **Upload**.

![012_import_notebook_0](images/012_import_notebook_0.JPG)

<br>

## Create a lakehouse and load data

In this task, you create a lakehouse and generate the sample data.

1. From the workspace toolbar, select **+ New item** and select **Lakehouse**.

2. Name the lakehouse `SalesLakehouse`. It may take a minute for the lakehouse to create.

3. Once the lakehouse opens, select **Open notebook** > **Existing notebook** from the toolbar.

4. Select the notebook you just uploaded — `21b-manage-semantic-model-lifecycle` — and select **Open**.

![021_open_notebook_0](images/021_open_notebook_0.JPG)

<br>

5. Once in the notebook, run the first code cell under the `Generate sample data` heading.

    >! Do **not** run any cells below the `Generate sample data` section yet. You need to create the semantic model first.

![022_generate_sample_data_0](images/022_generate_sample_data_0.JPG)

<br>

6. In the lakehouse explorer on the left, select the ellipsis … next to **Tables** and **Refresh** to confirm that `products`, `customers`, and `sales` tables appear.

![023_tables](images/023_tables.jpg)

<br>

## Create a semantic model

In this task, you create a Power BI semantic model from the lakehouse tables so you can validate it with SemPy.

1. Navigate back to the workspace and select the **SalesLakehouse** lakehouse.

2. Switch to the **SQL analytics endpoint** in the top-right corner.

![031_lakehouse-endpoint-switch](images/031_lakehouse-endpoint-switch.png)

<br>

3. From the toolbar, select **New semantic model** and configure as follows:

    - **Name**: `SalesData`
    - **Workspace**: Your `-dev` workspace
    - **Storage mode**: Direct Lake on SQL
    - **Tables**: Select all

![033_new_semantic_model_0](images/033_new_semantic_model_0.JPG)

<br>

4. Confirm and wait for the model to create. *It can take a minute or two for the semantic model to be fully available*.

You now have a semantic model built upon a lakehouse that can be managed using SemPy in notebooks.

<br>

## Validate the semantic model with SemPy

SemPy is a Python library in Fabric notebooks that connects to semantic models through the XMLA endpoint. In this task, you use SemPy to inspect model structure, check data quality, and verify relationships before deployment.

1. Navigate back to the notebook and scroll down to the **Validate the semantic model with SemPy** heading in the notebook.
   Run each code cell in this section one at a time and review the output:

   >! **Note**: The code for `Validate the semantic model with SemPy` and `Fix the semantic model with SemPy` is generating an error. 
   It is necessary to specify the workspace where the semantic model is located in each Fabric  function. <br>
   workspace="SalesLifecycle-dev"

<br>

![041_validate_sempy](images/041_validate_sempy.jpg)

<br>

2. Lists all tables in the `SalesData` semantic model to confirm it’s accessible.

    * The output shows three tables: `products`, `customers`, and `sales`.

![042_tables_0](images/042_tables_0.JPG)

<br>

3. Lists every column across all tables, showing name, data type, and parent table. This helps you understand an unfamiliar model without opening Power BI Desktop.
    
    * The output shows a table with each column’s name, data type, and which table it belongs to.

![043_columns_0](images/043_columns_0.JPG)

<br>

4. Checks for null values across all columns and duplicate primary keys. Nulls in foreign key columns mean rows can’t join to dimension tables, causing blanks in reports. Duplicates would inflate aggregations.

    * The output shows three null `CustomerKey` values and zero duplicate `SalesKey` values.

![044_null_duplicate_0](images/044_null_duplicate_0.JPG)

<br>

5. Uses SemPy to discover potential relationships between tables by matching column name patterns (like `Key` suffixes) and checking value overlap.

    * The output shows one many-to-one relationship on `ProductKey` between the `sales` and `products` tables.

![045_relationships_0](images/045_relationships_0.JPG)

<br>

6. Checks for orphaned foreign keys — values in the fact table that have no matching row in the dimension table. Orphaned keys cause blank rows in reports.

    * The output shows violations for `CustomerKey` value 99, which has no match in the `customers` table — meaning 10 sales records produce blank customer names.

![046_violations_0](images/046_violations_0.JPG)

<br>

7. Evaluates a DAX query against the semantic model to verify calculations without opening Power BI Desktop.

    * The output shows the same total for every product category. **This is incorrect** — the totals should differ because each category contains different products at different prices. The identical values occur because the semantic model has no relationships, so the DAX engine can’t filter sales by category.

![047_dax_query_0](images/047_dax_query_0.JPG)

<br>

## Fix the semantic model with SemPy

The validation revealed that the DAX query returns identical totals for every category — a clear sign that the semantic model is missing relationships. In this task, you use SemPy’s `connect_semantic_model` function to open a read/write connection to the model’s Tabular Object Model (TOM) and programmatically add the missing relationships.

1. In the notebook, scroll down to the **Fix the semantic model with SemPy** heading. Run each code cell in this section one at a time and review the output:

2. Opens a read/write connection to the `SalesData` semantic model and creates two many-to-one relationships using the TOM API: `sales[ProductKey]` to `products[ProductKey]` and `sales[CustomerKey]` to `customers[CustomerKey]`. Changes save automatically when the connection closes, and the cell then refreshes the semantic model so the new relationships hold data.

    * The output confirms two relationships were added and the semantic model was refreshed.

![048_s2_relationships_0](images/048_s2_relationships_0.JPG)

<br>

3. Re-runs the same DAX query from the validation step. Now that relationships exist, the DAX engine filters sales by product category and returns correct per-category totals.
    
    * The output shows different totals for each product category (Accessories, Bikes, Clothing), confirming the relationships are working.

>! **Note**: The `connect_semantic_model` function requires ReadWrite permissions on the semantic model and uses the XMLA read/write endpoint. Fabric Trial, Premium, and Fabric capacity workspaces have this endpoint enabled by default.

![049_dax_query_0](images/049_dax_query_0.JPG)

<br>

You can now close the notebook and any other items you may still have open.

<br>

## Create a deployment pipeline

Deployment pipelines promote validated content from development to production through defined stages. In this task, you create a pipeline with two stages and assign the workspaces you created earlier.

1. Navigate to your `-dev` workspace.

2. In the workspace toolbar, select **Create deployment pipeline**.

3. In the **Add a new deployment pipeline** dialog, enter a name for the pipeline (for example, `SalesData Deployment Pipeline`) and select **Next**.

4. In the pipeline structure step, you see three default stages: `Development`, `Test`, and `Production`. Delete the `Test` stage by selecting its delete icon so that only `Development` and `Production` remain. Select **Create and continue**.

![061_21b-configure-pipeline](images/061_21b-configure-pipeline.png)

<br>

5. In the **Development** stage, select your `-dev` workspace and select the check to save the setting.

6. In the **Production** stage, select your **-prod** workspace and save.

The pipeline shows two stages. The `Development` stage contains the `SalesLakehouse` lakehouse, notebook, and semantic model. The `Production` stage shows the assigned workspace with no content yet.

<br>

## Deploy content across stages

With both stages configured, you can compare and promote content. In this task, you deploy the validated content from Development to Production and verify the results.

1. In the pipeline view, review the comparison between stages. Items in Development should show an indicator that they exist only in the source stage.

2. Select the **Production** card to select all items for staging.

![071_21b-staged-items](images/071_21b-staged-items.png)

<br>

3. Select **Deploy**. In the deployment dialog, optionally add a note (for example, `Initial deployment - validated with SemPy`) and confirm the deployment.

4. Wait for the deployment to complete. The pipeline view should indicate that both stages are in sync.

5. Navigate to your **-prod** workspace to verify the deployed items. You should see the lakehouse, semantic model, and other items from the development workspace.

>! **Note**: After deployment, the production workspace contains copies of the items from development. Subsequent changes in development don’t appear in production until you deploy again, which gives you control over what reaches end users.

![072_21b-final-state](images/072_21b-final-state.png)

<br>

### Try it with Copilot (optional)

In a notebook in your development workspace, ask Copilot:

> `Write a Python script using the Fabric REST API to automate a deployment pipeline deployment and send a notification on completion.`

Review the generated code. This shows how teams automate deployments without manual steps. The generated code doesn’t trigger an actual deployment.

> `Write Python code to generate a data quality summary report for a FabricDataFrame. Include checks for null values, duplicate keys, and value distribution statistics.`

Review the generated code. Copilot produces a reusable quality check script that you can adapt to validate other semantic models without modifying the tables you already created.

**Unfortunately, Copilot isn't available with Fabric trial account.**

<br>

## Clean up resources

In this exercise, you validated a semantic model with SemPy, created a deployment pipeline, and deployed content from development to production.

If you’ve finished exploring, delete the resources you created for this exercise.

1. Navigate to the deployment pipeline. In the pipeline settings, select **Delete pipeline**.

2. In the menu bar on the left, select **Workspaces**.

3. Open your **-dev** workspace. In the toolbar, select **Workspace settings**, then in the **General** section, select **Remove this workspace**.

4. Open your **-prod** workspace. In the toolbar, select **Workspace settings**, then in the **General** section, select **Remove this workspace**.

![imagen](images)

<br>

---

[Up](#manage-the-semantic-model-lifecycle)
