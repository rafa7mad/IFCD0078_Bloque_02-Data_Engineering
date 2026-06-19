## DP-900T00A-Azure-Data-Fundamentals

# [Explore Azure SQL Database]([https://](https://microsoftlearning.github.io/DP-900T00A-Azure-Data-Fundamentals/Instructions/Labs/dp900-01-sql-lab.html))

## Provision an Azure SQL Database resource

1. Sign in to the [portal de Azure](https://portal.azure.com/?azure-portal=true) using your Azure account.

![Suscripción Azure](images/010_azure_portal.JPG)

2. At the top left of the page, select ＋ Create a resource. In the Search the Marketplace box, type Azure SQL and press Enter. In the search results, select Azure SQL (published by Microsoft).

 ![Azure SQL](images/020_azure_sql.JPG)
 
 3. On the Azure SQL page, select Create. On the Find the right Azure SQL solution for your workload page, in the Create a database tile, select More details, and then select Create SQL Database.

![Create a database](images/031_create_database.JPG)

![Create a database](images/032_create_sql_database.JPG)

4. Enter the following values on the Create SQL Database page, and leave all other properties with their default setting:

- Subscription: Select your Azure subscription.
- Resource group: Create a new resource group with a name of your choice.

> What is a resource group? It’s just a folder that holds related Azure resources together. When you’re finished, you can delete the folder to remove everything in one click.

- Database name: Dealership

![Create sql database](images/041_create_sql_database.JPG)

- Server: Select Create new and create a new server with a unique name in any available location. Use SQL authentication and specify your name as the server admin login and a suitably complex password (remember the password - you’ll need it later!)

Select OK to close the server form.

![Create server](images/042_create_server.JPG)

- Want to use SQL elastic pool?: No
- Workload environment: Development
- Compute + storage: Leave unchanged.
- Backup storage redundancy: Locally-redundant backup storage

![Create sql database](images/043_create_sql_database.JPG)

5. Select Next: Networking >. On the Networking page, in the Network connectivity section, select Public endpoint. Then, in the Firewall rules section, set both Allow Azure services and resources to access this server and Add current client IP address to Yes.

![051_networking](images/051_networking.JPG) 

Others options: Leave unchanged.

![052_networking](images/052_networking.JPG) 

6. Select Next: Security > and make sure the Enable Microsoft Defender for SQL option is set to Not now.

![060_security](images/060_security.JPG)

7. Select Next: Additional settings >. On the Additional settings tab, make sure the Use existing data option is set to None.

![070_additional](images/070_additional.JPG)

8. Select Review + create, review the settings, and then select Create.

![080_review_create](images/080_review_create.JPG)

9. Wait a few minutes for the deployment to complete. When it’s finished, select Go to resource.

![090_sql_database](images/090_sql_database.JPG)

![100_database](images/100_database.JPG)


## Create the database tables and add sample data

Your database is created, but it’s empty. In a relational database, data is stored in tables, which are like spreadsheets made of rows and columns. You’ll now create two tables and fill them with a small amount of sample data.

1. In the menu on the left side of the database page, select Query editor (preview). On the sign-in pane, select the SQL authentication tab, enter the server admin login and password you created earlier, and then select Connect.

![110_query_editor](images/110_query_editor.JPG)

The query editor is where you’ll type and run SQL commands. Select ＋ New query to open a blank query tab.

![111_new_query](images/111_new_query.JPG)

2. In the query tab, paste the following SQL code. This creates a Manufacturer table (the companies that build vehicles) and a Vehicle table (the cars the dealership sells).

![120_create_table](images/120_create_table.JPG)

3. Select ▷ Run above the query. You should see a message confirming the query succeeded. Your two tables now exist, but they’re empty.

![121_create_table_c](images/121_create_table_c.JPG)

4. Replace all the SQL in the query tab with the following code, which adds sample manufacturers and vehicles. Then select ▷ Run.

![130_insert_into](images/130_insert_into.JPG)

![131_insert_into](images/131_insert_into.JPG)


## Query the data

Now that your database has data in it, you can use SQL SELECT statements to retrieve and explore it.

1. Replace all the SQL in the query tab with the following code, and select ▷ Run. This returns every column and every row from the Vehicle table.

![140_select](images/140_select.JPG)

2. Replace the query with the following code and select ▷ Run. This returns only specific columns, so the results are easier to read.

![141_select_2](images/141_select_2.JPG)

3. Now try filtering the data. Replace the query with the following code and select ▷ Run. The WHERE clause returns only the vehicles that cost less than $30,000, and ORDER BY sorts them from cheapest to most expensive.

![142_select_3](images/142_select_3.JPG)

4. Finally, try a query that combines data from both tables. Replace the query with the following code and select ▷ Run.

![143_select_inner](images/143_select_inner.JPG)

5. Take a moment to experiment. Try changing the price in the WHERE clause, or sorting by a different column, then run the query again to see how the results change.

![144_select_inner_where](images/144_select_inner_where.JPG)

6. When you’re done, close the query editor pane, discarding your edits if prompted.

![150_close_query_editor](images/150_close_query_editor.JPG)


## Clean up

When you’ve finished exploring, you should delete the resources you created so you don’t incur any further costs.

1. In the Azure portal, navigate to the resource group you created at the start of the lab (for example, dp900-lab-rg).

![200_rgbd](images/200_rgbd.JPG)

2. Select Delete resource group, confirm the deletion by entering the resource group name, and select Delete.

![210_rgbd_delete](images/210_rgbd_delete.JPG)

Finish!

![220_rg](images/220_rg.JPG)








