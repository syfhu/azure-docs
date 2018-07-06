---
title: Connected Factory solution overview - Azure | Microsoft Docs
description: A description of the Azure IoT Connected Factory solution accelerator.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
---
# Get started with the Connected Factory solution accelerator

Azure [IoT solution accelerators][lnk-preconfigured-solutions] combine multiple Azure IoT services to deliver end-to-end solutions that implement common IoT business scenarios. The *Connected Factory* solution accelerator connects to and monitors your industrial devices. You can use the solution to analyze the stream of data from your devices and to drive operational productivity and profitability.

This tutorial shows you how to provision the Connected Factory solution accelerator. It also walks you through the basic features of the solution accelerator. You can access many of these features from the solution *dashboard* that deploys as part of the solution accelerator:

![Connected Factory solution accelerator dashboard][img-cf-home]

To complete this tutorial, you need an active Azure subscription.

> [!NOTE]
> If you don’t have an account, you can create a free trial account in just a couple of minutes. For details, see [Azure Free Trial][lnk_free_trial].

## Provision the solution

1. Log on to azureiotsuite.com using your Azure account credentials, and click "**+**" to create a solution.
2. Click **Select** on the **Connected Factory** tile.
3. Enter a **Solution name** for your Connected Factory solution accelerator.
4. Select the **Subscription** and **Region** you want to use to provision the solution.
5. Click **Create Solution** to begin the provisioning process. This process typically takes several minutes to run.

### While you wait for the provisioning process to complete

1. Click the tile for your solution with **Provisioning** status.
2. Notice the **Provisioning states** as Azure services are deployed in your Azure subscription.
3. Once provisioning completes, the status changes to **Ready**.
4. Click the tile to see the details of your solution in the right-hand pane.

> [!NOTE]
> If you encounter issues deploying the solution accelerator, review [Permissions on the azureiotsuite.com site][lnk-permissions] and the [Connected Factory FAQ](iot-accelerators-faq-cf.md). If the issues persist, create a service ticket on the [portal][lnk-portal].

Are there details you'd expect to see that aren't listed for your solution? Make feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## Scenario overview

When you deploy the Connected Factory solution accelerator, it is prepopulated with resources that enable you to step through a common industrial scenario. In this scenario, several factories connected to the solution report the data values required to compute overall equipment efficiency (OEE) and key performance indicators (KPIs). The following sections show you how to:

* Monitor factory, production lines, station OEE, and KPI values
* Analyze the telemetry data generated from these devices using Azure Time Series Insights
* Act on alarms to fix issues

A key feature of this scenario is that you can perform all these actions remotely from the solution dashboard. You do not need physical access to the devices.

## View the solution dashboard

The solution dashboard enables you to manage the deployed solution. It is a hierarchical representation of a global factory configuration. For example, you can view OEE and KPIs, publish new nodes for telemetry and action alarms.

1. When the provisioning is complete and the tile for your solution accelerator indicates **Ready**, choose **Launch** to open your Connected Factory solution portal in a new tab.

    ![Launch the solution accelerator][img-launch-solution]

1. By default, the solution portal shows the *dashboard*. To navigate to other areas of the portal, use the menu on the left-hand side of the page.

    ![Connected Factory solution accelerator dashboard][cf-img-menu]

The dashboard displays the following information:

* A **Factory locations** panel that shows the status, location, and current production configuration in the solution. When you first run the solution, there are a number of simulated devices. The production line simulation is composed of three real OPC UA servers per production line that perform simulated tasks and share data. For more information about OPC UA, see the [Connected Factory FAQ](iot-accelerators-faq-cf.md).
* A **map** that displays the location of each device connected to the solution. The solution can use the Bing Maps API to plot information on the map. If your subscription is enabled for Bing Maps Enterprise API, then this feature is used automatically. If not, see the [FAQ][lnk-faq] to learn how to make the map dynamic.
* An **Alarms** panel that displays alarms generated when a telemetry or OEE/KPI value exceeds a specific threshold.
* An **Overall Equipment Efficiency** panel that shows the OEE values for the whole enterprise, or the factory/production line/station you are viewing. This value is aggregated from the station view to the enterprise level. The OEE figure and its constituent elements can be further analyzed.
* **Key Performance Indicators** panel that displays the number of units produced and energy used by the whole enterprise or the factory/production line/station you are viewing. These values are aggregated from a station view to the enterprise level.

## View factories

The *Factory Locations* panel shows you the geographical location of all the factories in the solution, their status, and current production configuration. From the locations list, you can navigate to the other levels in the solution hierarchy. The rows in the list are hyperlinks that link details of the production lines at that location. It is then possible to drill into the production line details and down to the station level view. You can also apply a filter to the list.

![Connected Factory solution accelerator factories][cf-img-factories]

1. The **Factory panel** shows the factory list for this solution.

2. The factory list initially shows six factories created by the provisioning process. You can add additional simulated and physical devices to the solution.

3. To view the details of a factory, click the row in the factory list.

4. To view the details of a production line, click the row in the list.

5. To view the published OPC UA nodes of a station on the production line, click the row in the list.

6. To view details on a specific node in the station, click the row in the list. This action launches the context panel with Time Series Insights visualizations. Click these graphs to do further analysis in the Time Series Insights explorer environment.

## View map

If your subscription has access to the Bing Maps API, the *Factories* map shows you the geographical location and status of all the factories in the solution. To drill into the location details, click the locations displayed on the map.

![Connected Factory solution accelerator map][cf-img-map]

## View alarms

The **Alarms** panel shows you alarms generated due to a reported value or a calculated OEE/KPI value exceeding its configured threshold. This panel displays alarms at each level of the hierarchy, from the station level view to the global view. The alarms contain a description of the alarm, date, time, location, and number of occurrences. You can gain insights in to the data that caused the alarm using the Time Series Insights data. The Time Series Insights data is visualized in the alarms where applicable. If you are an Administrator, you can take default actions on the alarms such as:

* Close the alarm.
* Acknowledge the alarm.

Optionally, you can take more complex actions. For example, for the Pressure OPC UA Node of the Assembly you could:

* Show supporting information in a web page in a new browser window.
* Mitigate the cause of the alarm by calling an OPC UA method on the device.
* Suppress the availability of the default actions.

    ![Connected Factory solution accelerator alarms][cf-img-alerts]

> [!NOTE]
> These alarms are generated by rules that are specified in a configuration file in the solution accelerator. These rules can generate alarms when the OEE or KPI figures or OPC UA Node values are exceeding their configured threshold.

1. The **Alarms panel** shows the alarms generated in this solution.

2. To view the details of an alarm, click the alarm in the alarms panel.

3. To further analyze the alarm data, click the graph in the alarm panel to open the Time Series Insights explorer environment.

4. To address the alarm, several actions are available in the alarm panel. Choose the appropriate option for you and click the execute action command button.

## View overall equipment efficiency

OEE rates the efficiency of the manufacturing process using a key production-related operational parameters. OEE is an industry standard measure calculated by multiplying the availability rate, performance rate, and quality rate: OEE = availability x performance x quality.

![Connected Factory solution accelerator OEE][cf-img-oee]

1. To view OEE for any level in the hierarchy, navigate to the specific view you require. The OEE for that view displays in the panel along with each of the elements that make up the OEE percentage.

2. To further analyze the OEE for any level in the hierarchy data, click either the OEE, availability, performance, or quality percentage. A context panel appears with Time Series Insights powered visualizations that shows data from the last hour, last 24 hours, and last 7 days.

    ![Connected Factory solution accelerator TSI visualization][cf-img-tsi-visualization]

3. To further analyze the alarm data, click the graph in the alarm panel. This action opens the Time Series Insights explorer environment.

    ![Connected Factory solution accelerator TSI explorer][cf-img-tsi-explorer]

## View Key Performance Indicators

The solution provides two key performance indicators, *units per hour* and *energy used in kWh*.

![Connected Factory solution accelerator KPI][cf-img-kpi]

1. To view units per hour or energy used for any level in the hierarchy, navigate to the specific view you require. The units per hour and energy used display in the panel.

2. To analyze units per hour or energy used for any level in the hierarchy, click the gauge in the **Key Performance Indicators** panel. A context panel appears with Time Series Insights powered visualizations enabling you to view data from the last hour, the last 24 hours, and last 7 days.

## Scenario review

In this scenario, you monitored your factories OEE and KPIs values, in the dashboard. You then used Time Series Insights to provide more information to help drill further into the telemetry data for OEE and KPIs to help with detecting anomalies. You also used the alarm panel to view issues with your factories and you used the actions available to you to resolve the alarm.

## Other features

The following sections describe some additional features of the Connected Factory solution that are not described in the previous scenario.

## Apply filters

1. Click the **funnel** icon to display a list of available filters in either the factory locations panel or the alarms panel.

2. The filters panel is displayed for you.

    ![Connected Factory solution accelerator filters][cf-img-alert-filter]

3. Choose the filter that you require. It is also possible to type free text into the filter fields.

4. The filter is then applied for you. The filter state is also shown in the dashboard via a funnel that displays in the factories and alarms tables.

    ![Connected Factory solution accelerator filters][cf-img-alert-filter-funnel]

    > [!NOTE]
    > An active filter does not affect the displayed OEE and KPI values, it only filters the list contents.

5. To clear a filter, click the funnel and click filter in the filter context panel. The text **All** is displayed in the factories and alarms tables.

## Browse an OPC UA server

When you deploy the solution accelerator, you automatically provision simulated OPC UA servers that you can browse via the solution browser. These servers are *simulated OPC UA servers*. Simulated servers make it easy for you to experiment with the solution accelerator without the need to deploy real, physical servers. If you do want to connect a real OPC UA server to the solution, see the [Connect your OPC UA device to the Connected Factory solution accelerator][lnk-connect-cf] tutorial.

1. Click the **browser icon** in the dashboard navigation bar.

    ![Connected Factory solution accelerator server browser][cf-img-server-browser]

2. Choose one of the servers from the preconfigured list. This list shows the servers that are deployed for you in the solution accelerator.

    ![Connected Factory solution accelerator server selection][cf-img-server-choice]

3. Click **Connect**, a security dialog displays. For the simulation, it is safe to click **Proceed**

4. To expand any of the nodes in the server tree, click it. Nodes that are publishing telemetry have a tick mark beside them.

    ![Connected Factory solution accelerator server tree][cf-img-server-tree]

5. Right-click an item to read, write, publish, or call that node. The actions available to you depend on your permissions and the attributes of the node. The read option displays a context panel showing the value of the specific node. The write option displays a context panel where you can enter a new value. The call option displays a node where you can enter the parameters for the call.

## Publish a node

When you browse a *simulated OPC UA server*, you can also choose to publish new nodes. You can analyze the telemetry from these nodes in the solution. These *simulated OPC UA servers* make it easy to experiment with the solution accelerator without deploying real, physical devices.

1. Browse to a node in the OPC UA server browser tree that you wish to publish.

2. Right-click the node.

3. Choose **Publish**.

    ![Connected Factory publishes node][cf-img-publish-node]

4. A context panel appears which tells you that the publish has succeeded. The node appears in the station level view with a check mark beside it.

    ![Connected Factory preconfigured publishing success][cf-img-publish-success]

## Command and control

The Connected Factory allows you command and control your industry devices directly from the cloud. You can use this feature to respond to alarms generated by the device. For example, you could send a command to the device from the cloud. You can find the available commands in the **StationCommands** node in the OPC UA servers browser tree. In this scenario, you open a pressure release valve on the assembly station of a production line in Munich. To use the command and control functionality, you must be in the **Administrator** role for the solution accelerator deployment.

1. Browse to the **StationCommands** node in the OPC UA server browser tree.

2. Choose the command that you wish use.

3. Right-click the node.

4. Choose **Call**.

    ![Connected Factory solution accelerator call command][cf-img-call-command]

5. A context panel appears informing you which method you are about to call and any parameter details is applicable.

6. Choose **Call**.

    ![Connected Factory solution accelerator call context][cf-img-call-context]

7. The context panel is updated to inform you that the method call succeeded. You can verify the call succeeded by reading the value of the pressure node that updated as a result of the call.

    ![Connected Factory solution accelerator call success][cf-img-call-success]

## Behind the scenes

When you deploy a solution accelerator, the deployment process creates multiple resources in the Azure subscription you selected. You can view these resources in the Azure [portal][lnk-portal]. The deployment process creates a **resource group** with a name based on the name you choose for your solution accelerator:

![Solution accelerator in the Azure portal][img-cf-portal]

You can view the settings of each resource by selecting it in the list of resources in the resource group.

You can also view the source code for the solution accelerator. The Connected Factory solution accelerator source code is in the [azure-iot-connected-factory][lnk-cfgithub] GitHub repository:

When you are done, you can delete the solution accelerator from your Azure subscription on the [azureiotsuite.com][lnk-azureiotsuite] site. This site enables you to easily delete all the resources that were provisioned when you created the solution accelerator.

> [!NOTE]
> To ensure that you delete everything related to the solution accelerator, delete it on the [azureiotsuite.com][lnk-azureiotsuite] site. Do not delete the resource group in the portal.

## Next Steps

Now that you’ve deployed a working solution accelerator, you can continue getting started with IoT solution accelerators by reading the following articles:

* [Connected Factory solution accelerator walkthrough][lnk-rm-walkthrough]
* [Connect your device to the Connected Factory solution accelerator][lnk-connect-cf]
* [Permissions on the azureiotsuite.com site][lnk-permissions]

[img-cf-home]:media/iot-accelerators-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-accelerators-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-accelerators-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-accelerators-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-accelerators-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-accelerators-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-accelerators-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-accelerators-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-accelerators-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-accelerators-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-accelerators-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-accelerators-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-accelerators-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-accelerators-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-accelerators-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-accelerators-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-accelerators-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-accelerators-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-accelerators-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-accelerators-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-accelerators-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]:iot-accelerators-what-are-solution-accelerators.md
[lnk-azureiotsuite]: https://www.azureiotsolutions.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]:iot-accelerators-connected-factory-sample-walkthrough.md
[lnk-connect-cf]:iot-accelerators-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-accelerators-permissions.md
[lnk-faq]: iot-accelerators-faq.md
