### PL-300 Microsoft Power-BI Data Analyst

# Clean, transform, and load data in Power BI

## Lab story

In this lab, you’ll use data cleansing and transformation techniques to start shaping your data model. You’ll then apply the queries to load each as a table to the semantic model.

In this lab, you learn how to:

- Apply various data transformations.
- Load queries to the semantic model.

**This lab should take approximately 45 minutes.**

<br>

## Get started

To complete this exercise, first open a web browser and enter the following URL to download the zip folder:

> https://github.com/MicrosoftLearning/PL-300-Microsoft-Power-BI-Data-Analyst/raw/Main/Allfiles/Labs/02-transform-data-power-bi/02-transform-data.zip

Extract the ZIP file to: **C:\Users\Student\Downloads\02-transform-data**

Open the **02-Starter-Sales Analysis.pbix** file.

>*! **Note:** If a sign-in dialog appears while the file is loading, select **Cancel**. Close any other informational windows. If Power BI asks you to apply pending changes, select **Apply Later**.*

<br>

## Configure the Salesperson query

In this task, you will configure the `DimEmployee` query and transform it into the `Salesperson` table.

> **Important:** Rename columns exactly as indicated because later steps depend on these names.

1. In Power BI Desktop, go to the **Home** ribbon and select **Transform data** to open Power Query Editor.

2. In the **Queries** pane, select `DimEmployee`.

> **Note:** If Power BI asks how to connect to the data source, select **Edit Credentials**, use your current Windows credentials, and allow the unencrypted connection if required.

3. In **Query Settings**, rename the query:

   `DimEmployee` → `Salesperson`

4. Use **Home > Choose Columns > Go to Column** to locate a column quickly.

5. Sort the column list alphabetically and locate `SalesPersonFlag`.

6. Filter `SalesPersonFlag` so that only rows with the value `TRUE` remain.

7. Confirm that a **Filtered Rows** step has been added under **Applied Steps**.

8. Select **Choose Columns**.

9. Clear **Select All Columns**.

10. Keep only these columns:

    - `EmployeeKey`
    - `EmployeeNationalIDAlternateKey`
    - `FirstName`
    - `LastName`
    - `Title`
    - `EmailAddress`

11. Confirm that Power Query has added a new step for the removed columns.

12. Select `FirstName`, hold **Ctrl**, and also select `LastName`.

13. Right-click either selected column and choose **Merge Columns**.

14. Use **Space** as the separator.

15. Name the new column:

    `Salesperson`

16. Rename:

    `EmployeeNationalIDAlternateKey` → `EmployeeID`

17. Rename:

    `EmailAddress` → `UPN`

> `UPN` means **User Principal Name**.

Check that the query contains:

- **5 columns**
- **18 rows**

---

## Configure the SalespersonRegion query

1. Select the `DimEmployeeSalesTerritory` query.

2. Rename it:

   `DimEmployeeSalesTerritory` → `SalespersonRegion`

3. Select the `DimEmployee` column.

4. Hold **Ctrl** and also select `DimSalesTerritory`.

5. Right-click one of the selected columns and choose **Remove Columns**.

Check that the query contains:

- **2 columns**
- **39 rows**

---

## Configure the Product query

1. Select `DimProduct` and rename it:

   `DimProduct` → `Product`

2. Filter `FinishedGoodsFlag` so that only `TRUE` values remain.

3. Keep only these columns:

   - `ProductKey`
   - `EnglishProductName`
   - `StandardCost`
   - `Color`
   - `DimProductSubcategory`

4. Expand `DimProductSubcategory`.

5. Clear **Select All Columns**.

6. Select:

   - `EnglishProductSubcategoryName`
   - `DimProductCategory`

7. Clear **Use Original Column Name as Prefix**, and confirm the operation.

8. Expand `DimProductCategory`.

9. Keep only:

   `EnglishProductCategoryName`

10. Rename these columns:

    - `EnglishProductName` → `Product`
    - `StandardCost` → `Standard Cost`
    - `EnglishProductSubcategoryName` → `Subcategory`
    - `EnglishProductCategoryName` → `Category`

Check that the query contains:

- **6 columns**
- **397 rows**

---

## Configure the Reseller query

1. Select `DimReseller` and rename it:

   `DimReseller` → `Reseller`

2. Keep only:

   - `ResellerKey`
   - `BusinessType`
   - `ResellerName`
   - `DimGeography`

3. Expand `DimGeography` and include only:

   - `City`
   - `StateProvinceName`
   - `EnglishCountryRegionName`

4. Review the distinct values in `BusinessType`.

   Notice that both `Warehouse` and `Ware House` are present.

5. Right-click `BusinessType` and select **Replace Values**.

6. Configure the replacement:

   - **Value to Find:** `Ware House`
   - **Replace With:** `Warehouse`

7. Rename:

   - `BusinessType` → `Business Type`
   - `ResellerName` → `Reseller`
   - `StateProvinceName` → `State-Province`
   - `EnglishCountryRegionName` → `Country-Region`

Check that the query contains:

- **6 columns**
- **701 rows**

---

## Configure the Region query

1. Select `DimSalesTerritory` and rename it:

   `DimSalesTerritory` → `Region`

2. Filter `SalesTerritoryAlternateKey` to exclude the value `0`.

3. Keep only:

   - `SalesTerritoryKey`
   - `SalesTerritoryRegion`
   - `SalesTerritoryCountry`
   - `SalesTerritoryGroup`

4. Rename:

   - `SalesTerritoryRegion` → `Region`
   - `SalesTerritoryCountry` → `Country`
   - `SalesTerritoryGroup` → `Group`

Check that the query contains:

- **4 columns**
- **10 rows**

---

## Configure the Sales query

1. Select `FactResellerSales` and rename it:

   `FactResellerSales` → `Sales`

2. Keep only these columns:

   - `SalesOrderNumber`
   - `OrderDate`
   - `ProductKey`
   - `ResellerKey`
   - `EmployeeKey`
   - `SalesTerritoryKey`
   - `OrderQuantity`
   - `UnitPrice`
   - `TotalProductCost`
   - `SalesAmount`
   - `DimProduct`

> `DimProduct` is retained temporarily so that `StandardCost` can be used when `TotalProductCost` is missing.

3. Expand `DimProduct` and include only:

   `StandardCost`

4. On the **Add Column** ribbon, select **Custom Column**.

5. Name the new column:

   `Cost`

6. Enter this formula:

```powerquery
if [TotalProductCost] = null then [OrderQuantity] * [StandardCost] else [TotalProductCost]
```

This calculates the cost from quantity and standard cost when `TotalProductCost` is null; otherwise, it keeps the existing value.

7. Remove:

   - `TotalProductCost`
   - `StandardCost`

8. Rename:

   - `OrderQuantity` → `Quantity`
   - `UnitPrice` → `Unit Price`
   - `SalesAmount` → `Sales`

9. Change `Quantity` to **Whole Number**.

10. Change these columns to **Fixed Decimal Number**:

    - `Unit Price`
    - `Sales`
    - `Cost`

Check that the query contains:

- **10 columns**
- **999+ rows**

> Power Query displays a maximum of 1,000 rows in the preview for each query.

---

## Configure the Targets query

1. Select `ResellerSalesTargets` and rename it:

   `ResellerSalesTargets` → `Targets`

> **Note:** If Power BI asks for credentials for this source, select **Edit Credentials** and use **Anonymous** access.

2. Select `Year` and `EmployeeID`.

3. Right-click either selected column and choose **Unpivot Other Columns**.

4. The former month column names (`M01` to `M12`) now appear in `Attribute`, and their contents appear in `Value`.

5. Filter `Value` to remove rows containing a hyphen (`-`).

6. Rename:

   - `Attribute` → `MonthNumber`
   - `Value` → `Target`

7. In `MonthNumber`, use **Replace Values**.

8. Replace:

   `M` → *(empty value)*

9. Change `MonthNumber` to **Whole Number**.

10. Go to **Add Column > Column From Examples**.

11. For the first row, corresponding to year 2017 and month 7, enter a date representing July 1, 2017.

> **Regional settings:** The Microsoft-hosted VM uses U.S. date formatting (`7/1/2017`). With other regional settings, enter the equivalent valid date for your system.

12. Confirm that Power Query predicts the remaining date values.

13. Rename the generated column:

   `Merged` → `TargetMonth`

14. Remove:

   - `Year`
   - `MonthNumber`

15. Change the data types:

   - `Target` → **Fixed Decimal Number**
   - `TargetMonth` → **Date**

16. Select `Target`.

17. Go to **Transform > Standard > Multiply**.

18. Multiply the values by:

   `1000`

> The source target values are stored in thousands.

Check that the query contains:

- **3 columns**
- **809 rows**

---

## Configure the ColorFormats query

1. Select `ColorFormats`.

2. Notice that the first row contains the column names.

3. Go to **Home > Use First Row as Headers**.

Check that the query contains:

- **3 columns**
- **10 rows**

---

## Update the Product query

In this task, you will merge `ColorFormats` into `Product`.

1. Select `Product`.

2. Go to **Home > Merge Queries**.

3. In the `Product` table, select the `Color` column.

4. In the second table list, select `ColorFormats`.

5. In `ColorFormats`, select the `Color` column.

6. If the **Privacy Levels** dialog appears, set both data sources to:

   `Organizational`

   Then save the settings.

7. Keep the default join type:

   `Left Outer`

8. Expand the resulting `ColorFormats` column and include:

   - `Background Color Format`
   - `Font Color Format`

Check that the `Product` query now contains:

- **8 columns**
- **397 rows**

---

## Update the ColorFormats query

1. Select `ColorFormats`.

2. In **Query Settings**, select **All Properties**.

3. In **Query Properties**, clear:

   **Enable Load To Report**

> `ColorFormats` is only used as a supporting query for the merge with `Product`, so it does not need to be loaded as a separate model table.

---

## Review final product

In Power Query Editor, verify that the eight queries have these names:

- `Salesperson`
- `SalespersonRegion`
- `Product`
- `Reseller`
- `Region`
- `Sales`
- `Targets`
- `ColorFormats`

`ColorFormats` should have loading disabled.

1. Select **Close & Apply**.

2. Return to Power BI Desktop.

3. In the **Data** pane, verify that **7 tables** have been loaded into the semantic model.

---

## Lab complete

Saving the Power BI file is optional for this lab.

If you want to save it:

1. Go to **File > Save As**.
2. Select **Browse this device**.
3. Choose a destination folder and enter a descriptive file name.
4. Save the report as a `.pbix` file.
5. If prompted to apply pending query changes, select **Apply**.
6. Close Power BI Desktop.

---

[Up](#pl-300-microsoft-power-bi-data-analyst)
