---
title: Power BI tutorial for Azure Cosmos DB connector | Microsoft Docs
description: Use this Power BI tutorial to import JSON, create insightful reports, and visualize data using the Azure Cosmos DB and Power BI connector.
keywords: power bi tutorial, visualize data, power bi connector
services: cosmos-db
author: SnehaGunda
manager: kfile

ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: sngun

---
# Power BI tutorial for Azure Cosmos DB: Visualize data using the Power BI connector
[PowerBI.com](https://powerbi.microsoft.com/) is an online service where you can create and share dashboards and reports with data that's important to you and your organization.  Power BI Desktop is a dedicated report authoring tool that enables you to retrieve data from various data sources, merge and transform the data, create powerful reports and visualizations, and publish the reports to Power BI.  With the latest version of Power BI Desktop, you can now connect to your Azure Cosmos DB account via the Azure Cosmos DB connector for Power BI.   

In this Power BI tutorial, we walk through the steps to connect to an Azure Cosmos DB account in Power BI Desktop, navigate to a collection where we want to extract the data using the Navigator, transform JSON data into tabular format using Power BI Desktop Query Editor, and build and publish a report to PowerBI.com.

After completing this Power BI tutorial, you'll be able to answer the following questions:  

* How can I build reports with data from Azure Cosmos DB using Power BI Desktop?
* How can I connect to an Azure Cosmos DB account in Power BI Desktop?
* How can I retrieve data from a collection in Power BI Desktop?
* How can I transform nested JSON data in Power BI Desktop?
* How can I publish and share my reports in PowerBI.com?

> [!NOTE]
> The Power BI connector for Azure Cosmos DB connects to Power BI Desktop for extraction and transformation of data. Reports created in Power BI Desktop can then be published to PowerBI.com. Direct extraction and transformation of Azure Cosmos DB data cannot be performed in PowerBI.com. 

> [!NOTE]
> To connect Azure Cosmos DB to Power BI using the MongoDB API, you must use the [Simba MongoDB ODBC Driver](http://www.simba.com/drivers/mongodb-odbc-jdbc/).

## Prerequisites
Before following the instructions in this Power BI tutorial, ensure that you have access to the following resources:

* [The latest version of Power BI Desktop](https://powerbi.microsoft.com/desktop).
* Access to our demo account or data in your Azure Cosmos DB account.
  * The demo account is populated with the volcano data shown in this tutorial. This demo account is not bound by any SLAs and is meant for demonstration purposes only.  We reserve the right to make modifications to this demo account including but not limited to, terminating the account, changing the key, restricting access, changing, and delete the data, at any time without advance notice or reason.
    * URL: https://analytics.documents.azure.com
    * Read-only key: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==
  * Or, to create your own account, see [Create an Azure Cosmos DB database account using the Azure portal](https://azure.microsoft.com/documentation/articles/create-account/). Then, to get sample volcano data that's similar to what's used in this tutorial (but does not contain the GeoJSON blocks), see the [NOAA site](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) and then import the data using the [Azure Cosmos DB data migration tool](import-data.md).

To share your reports in PowerBI.com, you must have an account in PowerBI.com.  To learn more about Power BI for Free and Power BI Pro, visit [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## Let's get started
In this tutorial, let's imagine that you are a geologist studying volcanoes around the world.  The volcano data is stored in an Azure Cosmos DB account and the JSON documents look like the following sample document.

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

You want to retrieve the volcano data from the Azure Cosmos DB account and visualize data in an interactive Power BI report like the following report.

![By completing this Power BI tutorial with the Power BI connector, you'll be able to visualize data with the Power BI Desktop volcano report](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Ready to give it a try? Let's get started.

1. Run Power BI Desktop on your workstation.
2. Once Power BI Desktop is launched, a *Welcome* screen is displayed.
   
    ![Power BI Desktop Welcome screen - Power BI connector](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. You can **Get Data**, see **Recent Sources**, or **Open Other Reports** directly from the *Welcome* screen.  Click the X at the top right corner to close the screen. The **Report** view of Power BI Desktop is displayed.
   
    ![Power BI Desktop Report View - Power BI connector](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Select the **Home** ribbon, then click on **Get Data**.  The **Get Data** window should appear.
5. Click on **Azure**, select **Azure Cosmos DB (Beta)**, and then click **Connect**. 

    ![Power BI Desktop Get Data - Power BI connector](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. On the **Preview Connector** page, click **Continue**. The **Azure Cosmos DB** window appears.
7. Specify the Azure Cosmos DB account endpoint URL you would like to retrieve the data from as shown below, and then click **OK**. To use your own account, you can retrieve the URL from the URI box in the **[Keys](manage-account.md#keys)** blade of the Azure portal. To use the demo account, enter `https://analytics.documents.azure.com` for the URL. 
   
    Leave the database name, collection name, and SQL statement blank as these fields are optional.  Instead, we will use the Navigator to select the Database and Collection to identify where the data comes from.
   
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Desktop Connect Window](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. If you are connecting to this endpoint for the first time, you are prompted for the account key. For your own account, retrieve the key from the **Primary Key** box in the **[Read-only Keys](manage-account.md#keys)** blade of the Azure portal. For the demo account, the key is `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Enter the appropriate key and then click **Connect**.
   
    We recommend that you use the read-only key when building reports.  This prevents unnecessary exposure of the master key to potential security risks. The read-only key is available from the [Keys](manage-account.md#keys) blade of the Azure portal or you can use the demo account information provided above.
   
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Account Key](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > If you get an error that says "The specified database was not found." see the workaround steps in this [Power BI issue](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. When the account is successfully connected, the **Navigator** pane appears.  The **Navigator** shows a list of databases under the account.
10. Click and expand on the database where the data for the report comes from, if you're using the demo account, select **volcanodb**.   
11. Now, select a collection that contains the data to retrieve. If you're using the demo account, select **volcano1**.
    
    The Preview pane shows a list of **Record** items.  A Document is represented as a **Record** type in Power BI. Similarly, a nested JSON block inside a document is also a **Record**.
    
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Navigator window](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Click **Edit** to launch the Query Editor in a new window to transform the data.

## Flattening and transforming JSON documents
1. Switch to the Power BI Query Editor window, where the **Document** column in the center pane.
   ![Power BI Desktop Query Editor](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Click on the expander at the right side of the **Document** column header.  The context menu with a list of fields will appear.  Select the fields you need for your report, for instance,  Volcano Name, Country, Region, Location, Elevation, Type, Status and Last Know Eruption. Uncheck the **Use original column name as prefix** box, and then click **OK**.
   
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Expand documents](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. The center pane displays a preview of the result with the fields selected.
   
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Flatten results](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. In our example, the Location property is a GeoJSON block in a document.  As you can see, Location is represented as a **Record** type in Power BI Desktop.  
5. Click on the expander at the right side of the Document.Location column header.  The context menu with type and coordinates fields appear.  Let's select the coordinates field, ensure **Use original column name as prefix** is not selected, and click **OK**.
   
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Location record](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. The center pane now shows a coordinates column of **List** type.  As shown at the beginning of the tutorial, the GeoJSON data in this tutorial is of Point type with Latitude and Longitude values recorded in the coordinates array.
   
    The coordinates[0] element represents Longitude while coordinates[1] represents Latitude.
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Coordinates list](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. To flatten the coordinates array, create a **Custom Column** called LatLong.  Select the **Add Column** ribbon and click on **Custom Column**.  The **Custom Column** window appears.
8. Provide a name for the new column, e.g. LatLong.
9. Next, specify the custom formula for the new column.  For our example, we will concatenate the Latitude and Longitude values separated by a comma as shown below using the following formula: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Click **OK**.
   
    For more information on Data Analysis Expressions (DAX) including DAX functions, please visit [DAX Basic in Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Add Custom Column](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Now, the center pane shows the new LatLong columns populated with the values.
    
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Custom LatLong column](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    If you receive an Error in the new column, make sure that the applied steps under Query Settings match the following figure:
    
    ![Applied steps should be Source, Navigation, Expanded Document, Expanded Document.Location, Added Custom](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    If your steps are different, delete the extra steps and try adding the custom column again. 

11. Click **Close and Apply** to save the data model.
    
    ![Power BI tutorial for Azure Cosmos DB Power BI connector - Close & Apply](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## Build the reports
Power BI Desktop Report view is where you can start creating reports to visualize data.  You can create reports by dragging and dropping fields into the **Report** canvas.

![Power BI Desktop Report View - Power BI connector](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

In the Report view, you should find:

1. The **Fields** pane, this is where you can see a list of data models with fields you can use for your reports.
2. The **Visualizations** pane. A report can contain a single or multiple visualizations.  Pick the visual types fitting your needs from the **Visualizations** pane.
3. The **Report** canvas, this is where you build the visuals for your report.
4. The **Report** page. You can add multiple report pages in Power BI Desktop.

The following shows the basic steps of creating a simple interactive Map view report.

1. For our example, we will create a map view showing the location of each volcano.  In the **Visualizations** pane, click on the Map visual type as highlighted in the screenshot above.  You should see the Map visual type painted on the **Report** canvas.  The **Visualization** pane should also display a set of properties related to the Map visual type.
2. Now, drag and drop the LatLong field from the **Fields** pane to the **Location** property in **Visualizations** pane.
3. Next, drag and drop the Volcano Name field to the **Legend** property.  
4. Then, drag and drop the Elevation field to the **Size** property.  
5. You should now see the Map visual showing a set of bubbles indicating the location of each volcano with the size of the bubble correlating to the elevation of the volcano.
6. You now have created a basic report.  You can further customize the report by adding more visualizations.  In our case, we added a Volcano Type slicer to make the report interactive.  
   
    ![Screenshot of the final Power BI Desktop report upon completion of the Power BI tutorial for Azure Cosmos DB](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)
7. On the File menu, click **Save** and save the file as PowerBITutorial.pbix.

## Publish and share your report
To share your report, you must have an account in PowerBI.com.

1. In the Power BI Desktop, click on the **Home** ribbon.
2. Click **Publish**.  You are be prompted to enter the user name and password for your PowerBI.com account.
3. Once the credential has been authenticated, the report is published to your destination you selected.
4. Click **Open 'PowerBITutorial.pbix' in Power BI** to see and share your report on PowerBI.com.
   
    ![Publishing to Power BI Success! Open tutorial in Power BI](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## Create a dashboard in PowerBI.com
Now that you have a report, lets share it on PowerBI.com

When you publish your report from Power BI Desktop to PowerBI.com, it generates a **Report** and a **Dataset** in your PowerBI.com tenant. For example, after you published a report called **PowerBITutorial** to PowerBI.com, you will see PowerBITutorial in both the **Reports** and **Datasets** sections on PowerBI.com.

   ![Screenshot of the new Report and Dataset in PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

To create a sharable dashboard, click the **Pin Live Page** button on your PowerBI.com report.

   ![Screenshot of the new Report and Dataset in PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Then follow the instructions in [Pin a tile from a report](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) to create a new dashboard. 

You can also do ad hoc modifications to report before creating a dashboard. However, it's recommended that you use Power BI Desktop to perform the modifications and republish the report to PowerBI.com.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

![Screenshot of Refresh Now in PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    ![Screenshot of the Schedule Refresh in PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## Next steps
* To learn more about Power BI, see [Get started with Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* To learn more about Azure Cosmos DB, see the [Azure Cosmos DB documentation landing page](https://azure.microsoft.com/documentation/services/cosmos-db/).

