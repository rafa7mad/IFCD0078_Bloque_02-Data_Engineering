# Get started with Real-Time Intelligence in Microsoft Fabric

Microsoft Fabric provides Real-Time Intelligence, enabling you to create analytical solutions for real-time data streams. In this exercise, you’ll use the Real-Time Intelligence capabilities in Microsoft Fabric to ingest, analyze, and visualize a real-time stream of stock market data.

This lab takes approximately 30 minutes to complete.

>! Tip: For related training content, see Get started with Real-Time Intelligence in Microsoft Fabric.


## Create a workspace

>! Note: You need access to a Fabric paid or trial capacity to complete this exercise. For information about the free Fabric trial, see Fabric trial.

>! Note: You need a Microsoft Fabric tenant to complete this exercise.

1. Navigate to the [Microsoft Fabric home page](https://app.fabric.microsoft.com/home?experience=fabric) at https://app.fabric.microsoft.com/home?experience=fabric in a browser, and sign in with your Fabric credentials.

2. In the menu bar on the left, select Workspaces (the icon looks similar to 🗇).

3. Create a new workspace with a name of your choice, selecting a licensing mode that includes Fabric capacity (Trial, Premium, or Fabric).

4. When your new workspace opens, it should be empty.

![001_workspace](images/001_workspace.JPG)

<br>

---

## Create an eventstream

Now you’re ready to find and ingest real-time data from a streaming source. To do this, you’ll start in the Fabric Real-Time Hub. The real-time hub provides an easy way to find and manage sources of streaming data.

>! Tip: The first time you use the Real-Time Hub, some Getting started tips may be displayed. You can close these.

1. In the menu bar on the left, select the Real-Time hub.

>! Note: If you don’t see the Real-Time hub, select the ellipsis (…) and then pin the Real-Time hub to the menu bar.

![011_Real_time](images/011_Real_time.jpg)

<br>

2. In the Real-Time hub, select Add data.

![012_add_data](images/012_add_data.jpg)

<br>

3. Select the Stock market sample data source.

![013_Stock_market](images/013_Stock_market.jpg)

<br>

4. Configure the data source as follows:
- Source name: stock
- Workspace: Select the workspace you created
- Eventstream name: stock-data

>! The default stream associated with this data will automatically be named *stock-data-stream*.

![014_stock](images/014_stock.jpg)

<br>

5. Select Next, then Connect to create the eventstream.

6. Select Open eventstream. The eventstream will show the stock source and the stock-data-stream on the design canvas:

![016_eventstream](images/016_eventstream.jpg)

<br>

---

## Create an eventhouse

The eventstream ingests the real-time stock data, but doesn’t currently do anything with it. Let’s create an eventhouse where we can store the captured data in a table.

1. On the menu bar on the left, select Create. In the New page, under the Real-Time Intelligence section, select Eventhouse. Give it a unique name of your choice.

>! Note: If the Create option is not pinned to the sidebar, you need to select the ellipsis (…) option first.

    Close any tips or prompts that are displayed until you see your new empty eventhouse.

![021_Eventhouse](images/021_Eventhouse.jpg)

<br>

2. In the pane on the left, note that your eventhouse contains a KQL database with the same name as the eventhouse. You can create tables for your real-time data in this database, or create additional databases as necessary.

3. Select the database, and note that there is an associated queryset. This file contains some sample KQL queries that you can use to get started querying the tables in your database.
However, currently there are no tables to query. Let’s resolve that problem by getting data from the eventstream into a new table.

![023_KQL_database](images/023_KQL_database.jpg)

<br>

4. In the main page of your KQL database, select Get data.

5. For the data source, select Eventstream > Existing eventstream.

![025_Existing_eventstream](images/025_Existing_eventstream.jpg)

<br>

6. In the Select or create a destination table pane, create a new table named stock. Then in the Configure the data source pane, select your workspace and the stock-data eventstream and name the connection stock-table.

![026_stock-table](images/026_stock-table.jpg)

<br>

7. Use the Next button to complete the steps to inspect the data and then finish the configuration. Then close the configuration window to see your eventhouse with the stock table.

![027_connection](images/027_connection.jpg)

<br>

The connection between the stream and the table has been created. Let’s verify that in the eventstream.

8. In the menu bar on the left, select the Real-Time hub. In the … menu for the stock-data-stream stream, select Open eventstream.

The eventstream now shows a destination for the stream:

![028_Open_eventstream](images/028_Open_eventstream.jpg)

<br>

>! Tip: Select the destination on the design canvas, and if no data preview is shown beneath it, select Refresh.

In this exercise, you’ve created a very simple eventstream that captures real-time data and loads it into a table. In a real solution, you’d typically add transformations to aggregate the data over temporal windows (for example, to capture the average price of each stock over five-minute periods).

Now let’s explore how you can query and analyze the captured data.

<br>

---

## Query the captured data

The eventstream captures real-time stock market data and loads it into a table in your KQL database. You can query this table to see the captured data.

1. In the menu bar on the left, select your eventhouse database.
2. Select the queryset for your database.
3. In the query pane, modify the first example query as shown here:

code
 stock
 | take 100

4. Select the query code and run it to see 100 rows of data from the table.

![034_queryset_query](images/034_queryset_query.jpg)

<br>

5. Review the results, then modify the query to retrieve the average price for each stock symbol in the last 5 minutes:

code
 stock
 | where ["time"] > ago(5m)
 | summarize avgPrice = avg(todecimal(bidPrice)) by symbol
 | project symbol, avgPrice

6. Highlight the modified query and run it to see the results.

![036_average](images/036_average.jpg)

7. Wait a few seconds and run it again, noting that the average prices change as new data is added to the table from the real-time stream.

![037_average](images/037_average.JPG)

<br>

---

## Create a real-time dashboard

Now that you have a table that is being populated by stream of data, you can use a real-time dashboard to visualize the data.

1. In the query editor, select the KQL query you used to retrieve the average stock prices for the last five minutes.
2. On the toolbar, select Save to dashboard. Then pin the query in a new dashboard with the following settings:
- Dashboard name: Stock Dashboard
- Tile name: Average Prices

![042_Stock_Dashboard](images/042_Stock_Dashboard.jpg)

<br>

3. Create the dashboard and open it. It should look like this:

![043_Average_Prices](images/043_Average_Prices.jpg)

<br>

4. At the top-right of the dashboard, switch from Viewing mode to Editing mode.
5. Select the Edit (pencil) icon for the Average Prices tile.
6. In the Visual formatting pane, change the Visual from Table to Column chart:

![046_Column_chart](images/046_Column_chart.jpg)

<br>

7. At the top of the dashboard, select Apply changes and view your modified dashboard:

![047_Apply_changes](images/047_Apply_changes.jpg)

<br>

Now you have a live visualization of your real-time stock data.

<br>

---

## Create an alert

Real-Time Intelligence in Microsoft Fabric includes a technology named Activator, which can trigger actions based on real-time events. Let’s use it to alert you when the average stock price increases by a specific amount.

1. In the dashboard window containing your stock price visualization, in the toolbar, select Set alert.
2. In the Set alert pane, create an alert with the following settings:
- Run query every: 5 minutes
- Check: On each event grouped by
- Grouping field: symbol
- When: avgPrice
- Condition: Increases by
- Value: 100
- Action: Send me an email
- Save location:
    - Workspace: Your workspace
    - Item: Create a new item
    - New item name: A unique name of your choice

![images](images)
Screenshot of alert settings.

<br>

3. Create the alert and wait for it to be saved. Then close the pane confirming it has been created.
4. In the menu bar on the left, select the page for your workspace (saving any unsaved changes to your dashboard if prompted).
5. On the workspace page, view the items you have created in this exercise, including the activator for your alert.
6. Open the activator, and in its page, under the avgPrice node, select the unique identifier for your alert. Then view its History tab.

Your alert may not have been triggered, in which case the history will contain no data. If the average stock price ever changes by more than 100, the activator will send you an email and the alert will be recorded in the history.

<br>

---

## Clean up resources

In this exercise, you have create an eventhouse, ingested real-time data using an eventstream, queried the ingested data in a KQL database table, created a real-time dashboard to visualize the real-time data, and configured an alert using Activator.

If you’ve finished exploring Real-Time Intelligence in Fabric, you can delete the workspace you created for this exercise.

1. In the bar on the left, select the icon for your workspace.
2. In the toolbar, select Workspace settings.
3. In the General section, select Remove this workspace.

<br>

---

<br>

[Up](#get-started-with-real-time-intelligence-in-microsoft-fabric)


