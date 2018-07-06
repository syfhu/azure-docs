---
title: Predictive Maintenance solution accelerator overview - Azure | Microsoft Docs
description: A description of the Azure Predictive Maintenance solution accelerator
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: dobett
---

# Predictive Maintenance solution accelerator overview

The *Predictive Maintenance* [solution accelerator][lnk_preconfigured_solutions] is one of the [Microsoft Azure IoT solution accelerators][lnk_iot_suite] solution accelerators. This solution integrates real-time device telemetry collection with a predictive model created using [Azure Machine Learning][lnk-machine-learning].

With Azure IoT solution accelerators, you can quickly and easily connect to and monitor assets, and analyze telemetry in real time in dashboards and visualizations. In the Predictive Maintenance solution accelerator, the dashboards and visualizations provide you with new intelligence that can drive efficiencies and enhance revenue streams.

## The Scenario

Fabrikam is a regional airline that focuses on great customer experience at competitive prices. One cause of flight delays is maintenance issues and aircraft engine maintenance is particularly challenging. Fabrikam must avoid engine failure during flight at all costs, so it inspects its engines regularly and schedules maintenance according to a plan. However, aircraft engines don’t always wear the same. Some unnecessary maintenance is performed on engines. More importantly, issues arise which can ground an aircraft until maintenance is performed. If an aircraft is at a location where the right technicians or spare parts are not available, these issues can be especially costly.

The engines of Fabrikam’s aircraft are instrumented with sensors that monitor engine conditions during flight. Fabrikam uses the Predictive Maintenance solution accelerator to collect the sensor data collected during the flight. After accumulating years of engine operational and failure data, Fabrikam’s data scientists have modeled a way to predict the Remaining Useful Life (RUL) of an aircraft engine. The model uses a correlation between data from four of the engine sensors and engine wear that leads to eventual failure. While Fabrikam continues to perform regular inspections to ensure safety, it can now use the models to compute the RUL for each engine after every flight. The model uses the telemetry collected from the engines during the flight. Fabrikam can now predict future points of failure and plan for maintenance and repair in advance.

> [!NOTE]
> The solution model uses actual engine wear data.

By predicting the point when maintenance is required, Fabrikam can optimize its operations to reduce costs.

Maintenance coordinators work with schedulers to:

- Plan maintenance to coincide with an aircraft stopping at a particular location.
- Ensure sufficient time is available for the aircraft to be out of service without causing schedule disruption.
- To schedule technicians to ensure that aircraft are serviced efficiently without wait time.

Inventory control managers receive maintenance plans, so they can optimize their ordering process and spare parts inventory.

These activities enable Fabrikam to minimize aircraft ground time and reduce operating costs while ensuring the safety of passengers and crew.

To understand how [Azure IoT solution accelerators][lnk_iot_suite] provides the capabilities customers need to realize the potential of predictive maintenance, review this [infographic][lnk_infographic].

## How the Predictive Maintenance solution accelerator is built

The solution uses an existing Azure Machine Learning model available as a template to show these capabilities working from device telemetry collected through IoT solution accelerators services. Microsoft has built a [regression model][lnk_regression_model] of an aircraft engine based on publically available data<sup>\[1\]</sup>, and step-by-step guidance on how to use the model.

The Azure IoT Predictive Maintenance solution accelerator uses the regression model created from this template. The model is deployed into your Azure subscription and exposed through an automatically generated API. The solution includes a subset of the testing data representing 4 (of 100 total) engines and the 4 (of 21 total) sensor data streams. This data is sufficient to provide an accurate result from the trained model.

*\[1\] A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Research Center, Moffett Field, CA*

## Get started with Predictive Maintenance

This tutorial shows you how to provision the Predictive Maintenance solution accelerator. It also walks you through the basic features of the Predictive Maintenance solution accelerator. You can access many of these features through the solution dashboard that deploys along with the solution accelerator.

To complete this tutorial, you need an active Azure subscription.

> [!NOTE]
> If you don’t have an account, you can create a free trial account in just a couple of minutes. For details, see [Azure Free Trial][lnk_free_trial].

1. Log on to [azureiotsuite.com][lnk-azureiotsuite] using your Azure account credentials, and click **+** to create a solution.
1. Click **Select** the **Predictive Maintenance** tile.
1. Enter a **Solution name** for your Predictive Maintenance solution accelerator.
1. Select the **Region** and **Subscription** you want to use to provision the solution.
1. Click **Create Solution** to begin the provisioning process. This process typically takes several minutes to run.

### Wait for the provisioning process to complete

1. Click the tile for your solution with **Provisioning** status.
1. Notice the **Provisioning states** as Azure services are deployed in your Azure subscription.
1. Once provisioning completes, the status changes to **Ready**.
1. Click the tile to see the details of your solution in the right-hand pane. From this pane, you can launch the solution dashboard and access the Machine Learning workspace.

> [!NOTE]
> If you encounter issues deploying the solution accelerator, review [Permissions on the azureiotsuite.com site][lnk-permissions] and the [FAQ][lnk-faq]. If the issues persist, create a service ticket on the [portal][lnk-portal].

Are there details you'd expect to see that aren't listed for your solution? Make feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## View the solution

This section guides you through the solution UI.

### Predictive Maintenance Dashboard

This page in the web application uses PowerBI JavaScript controls (see the [PowerBI-visuals repository][lnk-powerbi]) to visualize:

* The output data from the Stream Analytics jobs in blob storage.
* The RUL and cycle count per aircraft engine.

### Observing the behavior of the cloud solution

In the Azure portal, navigate to the resource group with the solution name you chose to view your provisioned resources.

![][img-resource-group]

When you provision the solution accelerator, you receive an email with a link to the Machine Learning workspace. You can also navigate to the Machine Learning workspace from the [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution. A tile is available on this page when the solution is in the **Ready** state.

![][img-machine-learning]

In the solution portal, you can see that the sample is provisioned with four simulated devices to represent two aircraft with two engines per aircraft, each with four sensors. When you first navigate to the solution portal, the simulation is stopped.

![][img-simulation-stopped]

Click **Start simulation** to begin the simulation. The sensor history, RUL, Cycles, and RUL history populate the dashboard.

![][img-simulation-running]

When RUL is less than 160 (an arbitrary threshold chosen for demonstration purposes), the solution portal displays a warning symbol next to the RUL display. The solution portal also highlights the aircraft engine in yellow. Notice how the RUL values have a general downward trend overall, but tend to bounce up and down. This behavior results from the varying cycle lengths and the model accuracy.

![][img-simulation-warning]

The full simulation takes around 35 minutes to complete 148 cycles. The 160 RUL threshold is met for the first time at around 5 minutes and both engines hit the threshold at around 8 minutes.

The simulation runs through the complete dataset for 148 cycles and settles on final RUL and cycle values.

You can stop the simulation at any point, but clicking **Start Simulation** replays the simulation from the start of the dataset.

## Next steps

To learn more about how Azure IoT enables predictive maintenance scenarios, read [Capture value from the Internet of Things][lnk_capture_value].

Take a [walkthrough][lnk-predictive-walkthrough] of the Predictive Maintenance solution accelerator.

You can also explore some of the other features and capabilities of the IoT solution accelerators:

* [Frequently asked questions for IoT solution accelerators][lnk-faq]
* [IoT security from the ground up][lnk-security-groundup]

[img-resource-group]: media/iot-accelerators-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-accelerators-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-accelerators-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-accelerators-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-accelerators-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]:iot-accelerators-predictive-walkthrough.md
[lnk_preconfigured_solutions]:iot-accelerators-what-are-solution-accelerators.md
[lnk_iot_suite]:iot-accelerators-options.md
[lnk_infographic]: https://azure.microsoft.com/en-us/solutions/architecture/predictive-maintenance/
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-azureiotsuite]: https://www.azureiotsolutions.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsolutions.com
[lnk-permissions]: iot-accelerators-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/