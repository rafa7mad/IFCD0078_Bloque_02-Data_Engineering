### PL-300-Microsoft-Power-BI-Data-Analyst

# Setup local lab environment

Ideally, you should complete these labs in a hosted lab environment. If you want to complete them on your own computer, you can do so by installing the following software.

   - All setup and resource files can be [downloaded from GitHub](https://github.com/MicrosoftLearning/PL-300-Microsoft-Power-BI-Data-Analyst/raw/Main/AllfilesDownload.zip).

     - Extract the ‘AllFilesDownload’ folder to D:/ and rename it to ‘D:\Allfiles'.

***You may experience unexpected dialogs and behavior when using your own environment. Due to the wide range of possible local configurations, the course team cannot support issues you may encounter in your own environment.***

![001_folder_Allfiles_0](images/001_folder_Allfiles_0.jpg)

<br>

## Instructions using Windows 11

>! The instructions below are for a Windows 11 computer. Connecting from a different OS may not result in the same experience.

## Power BI Desktop

1. Download and install from the Microsoft store. If you do not have access to the Microsoft store, [download from the web](https://www.microsoft.com/download/details.aspx?id=58494). Power BI Desktop is the primary application for these labs.

   - Use the default options in the installer.

![011_power_bi_desktop_0](images/011_power_bi_desktop_0.jpg)

<br>

### Microsoft 365 Developer account

For some of the exercises, you will need to log into Power BI with an organizational account. You can use your own, but if you don’t have access, you can create a free [Microsoft 365 Developer account](https://developer.microsoft.com/microsoft-365/dev-program).

> Since I have a valid organizational account and can log in to Power BI, I do not need to create a free Microsoft 365 developer account.

### SQL Server Database Engine

The lab connects to a localhost SQL Server instance. The following instructions will help you install SQL Server and configure the default options. You only need to install the Database Engine feature.

   - Download the free [Developer copy of install media](https://www.microsoft.com/sql-server/sql-server-downloads?SilentAuth=1&f=255&MSPPError=-2147217396&rtc=1)
   - [Install SQL Server from the Installation Wizard (Setup)](https://learn.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup)

>! You can use an existing SQL Server instance if you have access, instead of installing a local version. However, you’ll need to modify the connection string from “localhost” to your instance name.

![013_SQL_Server_Database_Engine_0](images/013_SQL_Server_Database_Engine_0.jpg)

### Additional installation: SQL Server Management Studio (SSMS)

We need to install SSMS (SQL Server Management Studio) to connect to the databases on `localhost`.

![014a_SQL_Server_Management_Studio_0](images/014a_SQL_Server_Management_Studio_0.jpg)

<br>

Open SSMS (SQL Server Management Studio)

![014b_SQL_Server_Management_Studio_0](images/014b_SQL_Server_Management_Studio_0.jpg)

<br>

Now we are going to add the two databases available in `Allfiles`, specifically in the `DatabaseBackup` folder.

In `Databases`, select `Restore Database...`

![015a_Restore_Database](images/015a_Restore_Database.jpg)

<br>

Select `Device`, click the three dots (`...`), then click `Add` and locate the `.bak` file of the database you want to restore.

![015b_Restore_Database](images/015b_Restore_Database.jpg)

<br>

Once the backup file has been added, confirm the selection by clicking `OK`.

![015c_Restore_Database_0](images/015c_Restore_Database_0.jpg)

<br>

A confirmation message is displayed when the database has been restored successfully.

![015d_Restore_Database_0](images/015d_Restore_Database_0.jpg)

<br>

Check that the database has been restored and appears under `Databases`.

![015e_Restore_Database_0](images/015e_Restore_Database_0.jpg)

<br>

Repeat the same procedure to restore the other database.

![015f_Restore_Database_0](images/015f_Restore_Database_0.jpg)

<br>

### Microsoft Edge

1. Install the latest version of [Microsoft Edge](https://microsoft.com/edge) to access Power BI service online.

![021_Microsoft_Edge_Power_BI_0](images/021_Microsoft_Edge_Power_BI_0.jpg)

<br>

---

[Up](#pl-300-microsoft-power-bi-data-analyst)

