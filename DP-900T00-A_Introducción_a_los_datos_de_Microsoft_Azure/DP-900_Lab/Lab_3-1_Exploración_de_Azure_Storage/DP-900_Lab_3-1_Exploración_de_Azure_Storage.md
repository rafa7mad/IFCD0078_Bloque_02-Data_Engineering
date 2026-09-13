## DP-900T00A-Azure-Data-Fundamentals

# [Exploración de Azure Storage](https://microsoftlearning.github.io/DP-900T00A-Azure-Data-Fundamentals/Instructions/Labs/dp900-02-storage-lab.html)

In this lab, you’ll create an Azure Storage account, which is a secure place in the cloud to keep different kinds of data. You’ll then explore its four core services and see what each one is for:

- Blob storage, for storing files such as images, documents, and data files.
- Data Lake Storage Gen2, blob storage with real folders, used for big-data analytics.
- Azure Files, cloud file shares that behave like a shared network drive.

This kind of storage is called non-relational because, unlike a relational database, the data doesn’t have to be organized into related tables with a fixed structure. Don’t worry if these terms are new, every step is explained as you go.

<br>

## Provision an Azure Storage account
“Provisioning” just means creating and setting up a new resource. The first step in using Azure Storage is to create a storage account, which acts as a container for everything you store.

1. If you haven’t already done so, sign into the [Azure portal](https://portal.azure.com/?azure-portal=true#home).

2. On the Azure portal home page, select **＋ Create a resource** from the upper left-hand corner and search for $Storage$ $account$. Then in the resulting **Storage account** page, select **Create**.

![01_azure_portal](images/01_azure_portal.JPG)

![02_create_storage_account](images/02_create_storage_account.jpg)

<br>

3. Enter the following values on the Basics tab of the Create a storage account page:

- Subscription: Select your Azure subscription.
- Resource group: Select Create new and enter a name of your choice, such as dp900-lab-rg.
- Storage account name: Enter a unique name for your storage account using lower-case letters and numbers only (this name must not already be in use by anyone else).
- Region: Select any available location near you.
- Performance: Standard
- Redundancy: Locally-redundant storage (LRS)

![03_stac_basics](images/03_stac_basics.jpg)

<br>

4. Select Next: Advanced > and view the advanced configuration options. In particular, note that this is where you can enable hierarchical namespace to support Azure Data Lake Storage Gen2. Leave the Enable hierarchical namespace option cleared (you’ll enable it later), and then select Next: Networking > to view the networking options for your storage account.

![![alt text](04_stac_advanced.JPG)](images/04_stac_advanced.JPG)

<br>

<u>Networking</u> Default options, no changes.

![04_stac_networking](images/04_stac_networking.JPG)

<br>

5. Select Next: Data protection > and then in the Recovery section, deselect all of the Enable soft delete… options. These options retain deleted files for subsequent recovery, but can cause issues later when you enable hierarchical namespace.

![05_stac_dataprotection](images/05_stac_dataprotection.JPG)

<br>

6. Continue through the remaining Next > pages without changing any of the default settings, and then on the Review page, wait for your selections to be validated and select Create to create your Azure Storage account.

![06_stac_reviewcreate](images/06_stac_reviewcreate.jpg)

<br>

7. Wait for deployment to complete. Then select Go to resource to open the storage account that was deployed.

![07a_stac_deployment_progress](images/07b_stac_deployment_complete.JPG)

![07b_stac_deployment_complete](images/07b_stac_deployment_complete.JPG)

<br>

## Explore blob storage

Now that you have an Azure Storage account, you can create a container for blob data.

> ! **Tip**: A container groups blobs and is the first scoping level for access control. Starting with plain blob storage (no hierarchical namespace) shows virtual folder behavior you’ll compare to Data Lake Gen2 later.

1. Download the [product1.json](https://aka.ms/product1.json) JSON file from https://aka.ms/product1.json and save it on your computer (you can save it in any folder - you’ll upload it to blob storage later).

If the JSON file is displayed in your browser, right click the page, and select Save As. Name the file product1.json and store it in your downloads folder.

2. In the Azure portal page for your storage container, on the left side, in the Data storage section, select Containers.

![20_stac_overwiew](images/20_stac_overwiew.JPG)

<br>

3. In the Containers page, select ＋ Add container. In the New container pane, enter the name $data$.

Note that the Anonymous access level is automatically set to Private (no anonymous access) and can’t be changed, because anonymous access is disabled by default on the storage account. Select Create.

![23_stac_containers](images/23_stac_containers.jpg)

<br>

4. When the data container has been created, verify that it’s listed in the Containers page.

![24_stac_data](images/24_stac_data.jpg)

<br>

5. In the pane on the left side, in the top section, select Storage browser. This page provides a browser-based interface that you can use to work with the data in your storage account.

6. In the storage browser page, select Blob containers and verify that your data container is listed.

7. Select the data container, and note that it’s empty.

![27_stac_databrowser](images/27_stac_databrowser.jpg)

<br>

8. Select ＋ Add Directory and read the information about folders before creating a new directory named $products$.

![28_stac_datadirectory](images/28_stac_datadirectory.jpg)

<br>

9. In storage browser, verify that the current view shows the contents of the products folder you just created - observe that the “breadcrumbs” at the top of the page reflect the path Blob containers > data > products.

![29_stac_datadirectory](images/29_stac_datadirectory.jpg)

<br>

10. In the breadcrumbs, select data to switch to the data container, and note that it does not contain a folder named products.

Folders in blob storage are virtual, and only exist as part of the path of a blob. Since the products folder contained no blobs, it isn’t really there!

<br>

11. Use the ⤒ Upload button to open the Upload blob panel.

![31_stac_datadirectoryupload](images/31_stac_datadirectoryupload.jpg)

12. In the Upload blob panel, select the product1.json file you saved on your local computer previously. Then in the Advanced section, in the Upload to folder box, enter product_data and select the Upload button.

![32_stac_datadirectoryupload](images/32_stac_datadirectoryupload.jpg)

<br>

13. Close the Upload blob panel if it’s still open, and verify that a product_data virtual folder has been created in the data container.

![33_stac_data_product_data](images/33_stac_data_product_data.jpg)

<br>

14. Select the product_data folder and verify that it contains the product1.json blob you uploaded.

![34_stac_data_product1json](images/34_stac_data_product1json.jpg)

<br>

15. On the left side, in the Data storage section, select Containers.

![35_stac_data_storage](images/35_stac_data_storage.jpg)

<br>

16. Open the data container, and verify that the product_data folder you created is listed.

![36a_stac_data_product_data](images/36a_stac_data_product_data.jpg)

![36b_stac_data_product_data](images/36b_stac_data_product_data.jpg)

<br>

17. Select the ‧‧‧ icon at the right-end of the folder, and note that the menu doesn’t display any options. Folders in a flat namespace blob container are virtual, and can’t be managed.

> ! **Tip**: No real directory object exists, so there are no rename/permission operations — those require hierarchical namespace.

![37_stac_data_product_data](images/37_stac_data_product_data.jpg)

<br>

18. Use the X icon at the top right in the data page to close the page and return to the Containers page.

![38_stac_containers](images/38_stac_containers.JPG)

<br>

# Explore Azure Data Lake Storage Gen2

El soporte para Azure Data Lake Store Gen2 te permite usar carpetas jerárquicas para organizar y gestionar el acceso a blobs. También te permite usar almacenamiento en blob de Azure para alojar sistemas de archivos distribuidos para plataformas comunes de análisis de big data.

> ! **Tip**: Turning on hierarchical namespace makes folders behave like real directories. It also lets you do folder actions safely (all at once, without errors) and gives you file-permission controls similar to those in Linux. This is especially helpful when working with big data tools like Spark or Hadoop, or when managing large, organized data lakes.

<br>

1. Download the product2.json JSON file from https://aka.ms/product2.json and save it on your computer in the same folder where you downloaded product1.json previously - you’ll upload it to blob storage later.

<br>

2. In the Azure portal page for your storage account, on the left side, scroll down to the Settings section, and select Data Lake Gen2 upgrade.

<br>

3. In the Data Lake Gen2 upgrade page, expand and complete each step to upgrade your storage account to enable hierarchical namespace and support Azure Data Lake Storage Gen. This may take some time.

![40_data_lake_gen2](images/40_data_lake_gen2.jpg)

![40b_data_lake_gen2](images/40a_data_lake_gen2.jpg)

> If we have problems in step 2, we need to check that all the "Recovery" options in "Data Protection" are not checked, that is, they are all disabled.

![40b2_data_lake_gen2](images/40b2_data_lake_gen2.jpg)

![40b3_data_lake_gen2](images/40b3_data_lake_gen2.jpg)

![40c_data_lake_gen2](images/40c_data_lake_gen2.jpg)

<br>

4. When the upgrade is complete, in the pane on the left side, in the top section, select Storage browser and navigate back to the root of your data blob container, which still contains the product_data folder.

![44_data_product_data](images/44_data_product_data.jpg)

<br>

5. Select the product_data folder, and verify it still contains the product1.json file you uploaded previously.

![45_product1json](images/45_product1json.jpg)

<br>

6.Use the ⤒ Upload button to open the Upload blob panel.

![46_product1json](images/46_product1json.jpg)

<br>

7. In the Upload blob panel, select the product2.json file you saved on your local computer. Then select the Upload button.

![47_product2json](images/47_product2json.jpg)

<br>

8. Close the Upload blob panel if it’s still open, and verify that a product_data folder now contains the product2.json file.

![48_product2json](images/48_product2json.jpg)

<br>

> ! Tip: Adding a second file post-upgrade confirms seamless continuity: existing blobs still work, and new ones gain hierarchical benefits such as directory ACLs (Access Control Lists).

9. On the left side, in the Data storage section, select Containers.

![49_data_storage](images/49_data_storage.jpg)

<br>

10. Open the data container, and verify that the product_data folder you created is listed.

![50_product_data.jpg](images/50_product_data.jpg)

<br>

11. Select the ‧‧‧ icon at the right-end of the folder, and note that with hierarchical namespace enabled, you can perform configuration tasks at the folder-level; including renaming folders and setting permissions (Manage ACL).

![51_product_data](images/51_product_data.jpg)

<br>

> ! Tip: Real folders let you apply least-privilege security at folder granularity, rename safely, and speed recursive listings versus scanning thousands of prefixed blob names.

<br>

12. Use the X icon at the top right in the data page to close the page and return to the Containers page.

![52_containers](images/52_containers.jpg)

![52b_containers](images/52b_containers.jpg)

<br>

## Explore Azure Files

Azure Files provides a way to create cloud-based file shares.

> ! Tip: Azure Files offers SMB/NFS endpoints for lift‑and‑shift scenarios where apps expect a traditional file system. It complements (not replaces) blob storage by supporting file locks and OS-native tooling.

> ! Note: Because you enabled hierarchical namespace (Azure Data Lake Storage Gen2) earlier, file shares for this account are managed under Classic file shares. On a storage account without hierarchical namespace, this menu item is simply named File shares, but the steps to create and connect to a share are the same.

<br>

1. In the Azure portal page for your storage account, on the left side, in the Data storage section, select Classic file shares.

![61_classic_flie_shares](images/61_classic_flie_shares.jpg)

<br>

2. In the Classic file shares page, select ＋ Classic file share. On the Basics tab, enter the name files and leave the Access tier set to Transaction optimized.

![62_classic_flie_shares](images/62_classic_flie_shares.jpg)

<br>

3. Select Next: Backup > and clear the Enable backup checkbox to disable backup. Then select Review + create, and on the Review + create tab, select Create.

![63_classic_flie_shares_backup](images/63_classic_flie_shares_backup.jpg)

![63b_classic_flie_shares_backup](images/63b_classic_flie_shares_backup.jpg)

<br>

> ! Tip: Disabling backup keeps costs down for a short-lived lab environment — you would enable it for production resilience.

<br>

4. When the files share has been created, return to the Classic file shares page and open your new files share.

![64_files_share](images/64_files_share.jpg)

<br>

5. At the top of the page, select Connect. Then in the Connect pane, note that there are tabs for common operating systems (Windows, Linux, and macOS) that contain scripts you can run to connect to the shared folder from a client computer.

![65_files_share_connect](images/65_files_share_connect.jpg)

![65b_files_share_connect](images/65b_files_share_connect.jpg)

<br>

> !Tip: The generated scripts show exactly how to mount the share using platform-native commands, illustrating hybrid access patterns from virtual machines, containers, or on-prem servers.

6. Close the Connect pane and then close the files page to return to the Classic file shares page for your Azure storage account.

![66_files_share](images/66_files_share.jpg)

<br>

## Clean up

When you’ve finished exploring Azure Storage, you should delete the resources you created so you don’t incur any further costs.

<br>

1. In the Azure portal, navigate to the resource group you created at the start of the lab (for example, dp900-lab-rg).

![71_resource_ group](images/71_resource_%20group.jpg)

<br>

2. Select Delete resource group, confirm the deletion by entering the resource group name, and select Delete.

![72_resource_ group_dp900](images/72_resource_%20group_dp900.jpg)

![72b_resource_ group_dp900](images/72_resource_%20group_dp900.jpg)

<br>

We verified that the resource group had been removed.

![72c_resource_ group_cleanup](images/72c_resource_%20group_cleanup.jpg)

<br>

> !Tip: Deleting the resource group removes the storage account and everything inside it in a single step. This is the quickest way to make sure nothing is left running and costing money.

<br>

In this lab, you created an Azure Storage account and explored blob storage, Data Lake Storage Gen2, and Azure Files. You’ve now seen the main ways Azure stores non-relational data!