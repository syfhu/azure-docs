---
title: Define a new device type in Azure IoT Central | Microsoft Docs
description: This tutorial shows you, as a builder, how to define a new device type in your Azure IoT Central application. You define the telemetry, state, properties and settings for your type.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
---

# Tutorial: Define a new device type in your Azure IoT Central application

This tutorial shows you, as a builder, how to use a device template to define a new type of device in your Microsoft Azure IoT Central application. A device template defines the telemetry, state, properties, and settings for your device type.

To enable you to test your application before you connect a real device, Azure IoT Central generates a simulated device from the device template when you create it.

In this tutorial, you create a **Connected Air Conditioner** device template. A connected air conditioner device:

* Sends telemetry such as temperature and humidity.
* Reports state such as whether it is on or off.
* Has properties such as firmware version and serial number.
* Has settings such as target temperature and fan speed.

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create a new device template
> * Add telemetry to your device
> * View simulated telemetry
> * Define event measurement
> * View simulated events
> * Define state measurement
> * View simulated state
> * Use device properties
> * Use device settings
> * Use commands

## Prerequisites

To complete this tutorial, you need an Azure IoT Central application. If you completed the [Create an Azure IoT Central application](quick-deploy-iot-central.md) quickstart, you can reuse the application you created in the quickstart. Otherwise, complete the following steps to create an empty Azure IoT Central application:

1. Navigate to the Azure IoT Central [Application Manager](https://aka.ms/iotcentral) page.

2. Enter the email address and password you use to access your Azure subscription:

   ![Enter your organization account](./media/tutorial-define-device-type/sign-in.png)

3. To start creating a new Azure IoT Central application, choose **New Application**:

    ![Azure IoT Central Application Manager page](./media/tutorial-define-device-type/iotcentralhome.png)

4. To create a new Azure IoT Central application:

    * Choose a friendly application name, such as **Contoso Air Conditioners**. Azure IoT Central generates a unique URL prefix for you. You can change this URL prefix to something more memorable.
    
    * Choose an Azure Active Directory and Azure subscription to use. For more information about directories and subscriptions, see [Create an Azure IoT Central application](howto-create-application.md).
    
    * Either use an existing resource group, or create a new resource group with a name of your choice. For example, **contoso-rg**.
    
    * Choose the region geographically closest to you.
    
    * Choose the **Custom Application** application template.
    
    * Choose the **Free 30 Day Trial Application** payment plan.
    
    * Choose **Create**.

    ![Azure IoT Central Create Application page](./media/tutorial-define-device-type/iotcentralcreate.png)

For more information, see [How to create an Azure IoT Central application](howto-create-application.md).

## Create a new custom device template

As a builder, you can create and edit the device templates in your application. When you create a device template, Azure IoT Central generates a simulated device from the template. The simulated device generates telemetry that enables you to test the behavior of your application before you connect a physical device.

To add a new device template to your application, you need to go to the **Application Builder** page. To do so choose the **Application builder** on the left navigation menu.

![Application Builder page](./media/tutorial-define-device-type/builderhome.png)

## Add a device and define telemetry

The following steps show you how to create a new **Connected Air Conditioner** device template for devices that send temperature telemetry to your application:

1. On the **Application Builder** page, choose **Create Device Template**:

    ![Application Builder page, Create Device Template](./media/tutorial-define-device-type/builderhomedevices.png)

2. On the **Device Templates** page, choose **Custom**. A **Custom** device template enables you to define all the characteristics and behaviors of your connected air conditioner:

    ![Devices](./media/tutorial-define-device-type/builderhomedevicescustom.png)

3. On the **New Device Template** page, enter **Connected Air Conditioner** as the name of your device, and then choose **Create**. You can also upload an image of your device that's visible to operators in the device explorer:

    ![Custom Device](./media/tutorial-define-device-type/createcustomdevice.png)

4. In the **Connected Air Conditioner** device template, make sure you are on the **Measurements** page where you define the telemetry. Each device template you define has separate pages for you to:

    * Specify the measurements, such as telemetry, event, and state, sent by the device.
    
    * Define the settings used to control the device.
    
    * Define the properties used to record information about the device.
    
    * Define the rules associated with the device.
    
    * Customize the device dashboard for your operators.

    ![Air conditioner measurements](./media/tutorial-define-device-type/airconmeasurements.png)

    > [!NOTE]
    > To change the name of the device or device template, click on the text at the top of the page.

5. To add the temperature telemetry measurement, choose **New Measurement**. Then choose **Telemetry** as the measurement type:

    ![Connected air conditioner measurements](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Each type of telemetry you define for a device template includes [configuration options](howto-set-up-template.md) such as:

    * Display options.

    * Details of the telemetry.

    * Simulation parameters.

    To configure your **Temperature** telemetry, use the information in the following table:

    | Setting              | Value         |
    | -------------------- | -----------   |
    | Display Name         | Temperature   |
    | Field Name           | temperature   |
    | Units                | F             |
    | Min                  | 60            |
    | Max                  | 110           |
    | Decimal places       | 0             |

    You can also choose a color for the telemetry display. To save the telemetry definition, choose **Save**:

    ![Configure Temperature simulation](./media/tutorial-define-device-type/temperaturesimulation.png)

7. After a short while, the **Measurements** page shows a chart of the temperature telemetry from your simulated connected air conditioner device. Use the controls to manage visibility, aggregation, or to edit the telemetry definition:

    ![View temperature simulation](./media/tutorial-define-device-type/viewsimulation.png)

8. You can also customize the chart using the **Line**, **Stacked**, and **Edit Time Range** controls:

    ![Customize the chart](./media/tutorial-define-device-type/customizechart.png)

## Define Event measurement

You can use Event to define point-in-time data that is sent by the device to signify something of significance like an error or a component failure. Like telemetry measurements, Azure IoT Central can simulate device events to enable you to test the behavior of your application before you connect a physical device. You define event measurements for your device type in the **Measurements** view.

1. To add the **Fan Motor Error** event measurement, choose **New Measurement**. Then choose **Event** as the measurement type:

    ![Connected air conditioner measurements](./media/tutorial-define-device-type/eventnew.png)

2. Each type of Event you define for a device template includes [configuration options](howto-set-up-template.md) such as:

   * Display Name.

   * Field Name.

   * Severity.

    To configure your **Fan Motor Error** event, use the information in the following table:

    | Setting              | Value             |
    | -------------------- | -----------       |
    | Display Name         | Fan Motor Error   |
    | Field Name           | fanmotorerr       |
    | Severity             | Error             |

    To save the event definition, choose **Save**:

    ![Configure Event measurement](./media/tutorial-define-device-type/eventconfiguration.png)

3. After a short while, the **Measurements** page shows a chart of the events randomly generated from your simulated connected air conditioner device. Use the controls to manage visibility, or to edit the event definition:

    ![View event simulation](./media/tutorial-define-device-type/eventview.png)

1. To see additional details about the event, click the event on the chart:

    ![View Event Details](./media/tutorial-define-device-type/eventviewdetail.png)

## Define State measurement

You can use State to define and visualize the state of the device or its component over a period of time. Like telemetry measurements, Azure IoT Central can simulate device state to enable you to test the behavior of your application before you connect a physical device. You define state measurements for your device type in the **Measurements** view.

1. To add **Fan Mode** measurement, choose **New Measurement**. Then choose **State** as the measurement type:

    ![Connected air conditioner state measurements](./media/tutorial-define-device-type/statenew.png)

2. Each type of State you define for a device template includes [configuration options](howto-set-up-template.md) such as:

   * Display Name.

   * Field Name.

   * Values with optional display labels.

   * Color for each value.

    To configure your **Fan Mode** state, use the information in the following table:

    | Setting              | Value             |
    | -------------------- | -----------       |
    | Display Name         | Fan Mode          |
    | Field Name           | fanmode           |
    | Value                | 1                 |
    | Display label        | Operating         |
    | Value                | 0                 |
    | Display label        | Stopped           |

    To save the state measurement definition, choose **Save**:

    ![Configure State measurement](./media/tutorial-define-device-type/stateconfiguration.png)

3. After a short while, the **Measurements** page shows a chart of the states randomly generated from your simulated connected air conditioner device. Use the controls to manage visibility, or to edit the state definition:

    ![View state simulation](./media/tutorial-define-device-type/stateview.png)

4. In case, there are too many data points sent by the device within a small duration, the state measurement is shown with a different visual as shown below. If you click on the chart, then all the data points within that time period are displayed in a chronological order. You can also narrow down the time range so see the measurement plotted on the chart.

    ![View state Details](./media/tutorial-define-device-type/stateviewdetail.png)

## Settings, properties, and commands

Settings, properties and device properties, and commands are different values defined in a device template and associated with each individual device:

* You use _settings_ to send configuration data to a device from your application. For example, an operator could use a setting to change the device's telemetry interval from two seconds to five seconds. When an operator changes a setting, the setting is marked as pending in the UI until the device acknowledges that it has actioned the setting change.

* You use _properties_ to record information about your device in your application. For example, you can use properties to record a device's serial number or the device manufacturer's phone number. Properties are stored in the application and do not synchronize with the device. An operator can assign values to properties.

* You use _device properties_ to enable a device to send property values to your application. These properties can only be changed by the device. For an operator, device properties are read-only.

* You use _commands_ to remotely manage your device from your application. You can directly run commands on the device from the cloud to control the devices. For example, an operator can run commands such as reboot, to instantly reboot the device.

## Use settings

You use *settings* to enable an operator to send configuration data to a device. In this section, you add a setting to your **Connected Air Conditioner** device template that enables an operator to set the target temperature of the connected air conditioner.

1. Navigate to the **Settings** page for your **Connected Air Conditioner** device template:

    ![Prepare to add a setting](./media/tutorial-define-device-type/deviceaddsetting.png)

    You can create settings of different types such as numbers or text.

2. Choose **Number** to add a number setting to your device.

3. To configure your **Set Temperature** setting, use the information in the following table:

    | Field                | Value           |
    | -------------------- | -----------     |
    | Display Name         | Set Temperature |
    | Field Name           | setTemperature  |
    | Unit of measurement  | F               |
    | Decimal places       | 1               |
    | Minimum value        | 20              |
    | Maximum value        | 200             |
    | Initial value        | 80              |
    | Description          | Set the target temperature for the air conditioner |

    Then choose **Save**:

    ![Configure Set Temperature setting](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > When the device acknowledges a setting change, the status of the setting changes to **synced**.

4. You can customize the layout of the **Settings** page by moving and resizing settings tiles:

    ![Customize settings layout](./media/tutorial-define-device-type/settingslayout.png)

## Use properties / device properties

You use *properties* to store information about your device in the application. In this section, you add device properties to your **Connected Air Conditioner** device template to store the device serial number and firmware version. Note that both of these are read-only properties reported by the device -- you cannot assign values to them. Properties you might use that you can assign values to include the location of the device, ownership information, and the last service date/time for the device.
 
1. Navigate to the **Properties** page for your **Connected Air Conditioner** device template:

    ![Prepare to add a property](./media/tutorial-define-device-type/deviceaddproperty.png)

    You can create device properties of different types such as numbers or text. To add a serial number property to your device template, choose **Text**.

2. To configure your serial number property, use the information in the following table:

    | Field                | Value                |
    | -------------------- | -------------------- |
    | Display Name         | Serial number        |
    | Field Name           | serialNumber         |
    | Initial Value        | cac00001             |
    | Description          | Device serial number |

    Leave other fields with their default values.

    ![Configure the device properties](./media/tutorial-define-device-type/configureproperties.png)

    Choose **Save**.

3. To add a firmware version to the device properties for your device template, choose **Text**.

4. To configure your firmware version device property, use the information in the following table:

    | Field                | Value                   |
    | -------------------- | ----------------------- |
    | Display Name         | Firmware version        |
    | Field Name           | firmwareVersion         |
    | Initial Value        | 0.1                     |
    | Description          | Device firmware version |

    ![Configure the device properties](./media/tutorial-define-device-type/configureproperties2.png)

    Choose **Save**.

5. You can customize the layout of the **Properties** page by moving and resizing property tiles:

    ![Customize properties layout](./media/tutorial-define-device-type/propertieslayout.png)


## Use commands

You use _commands_ to enable an operator to run commands directly on the device. In this section, you add a command to your **Connected Air Conditioner** device template that enables an operator to echo a certain message on the connected air conditioner display (this works with MxChip sample code).

1. Navigate to the **Commands** page for your **Connected Air Conditioner** device template:

    ![Prepare to add a setting](media/tutorial-define-device-type/commandsecho.png)

    You can create commands of different types based on your requirements. 

1. Click **New Command** to add a command to your device.

1. To configure your new command, use the information in the following table:

    | Field                | Value           |
    | -------------------- | -----------     |
    | Display Name         | Echo Command    |
    | Field Name           | echo            |
    | Default Timeout      | 30              |
    | Display Name         | Display Text    |
    | Display Type         | text            |  

You can add additional inputs to the command by clicking **+** for inputs.

2. Choose **Save**.

3. You can customize the layout of the **Commands** page by moving and resizing commands tiles:

    ![Customize settings layout](media/tutorial-define-device-type/commandstileresize.png)

## View your simulated device

Now you have defined your **Connected Air Conditioner** device template, you can customize its **Dashboard** to include the measurements, settings, and properties you defined. Then you can preview the dashboard as an operator:

1. Choose the **Dashboard** page for your **Connected Air Conditioner** device template:

    ![Connected air conditioner dashboards](./media/tutorial-define-device-type/aircondashboards.png)

2. Choose **Line Chart** to add the component onto the **Dashboard**:

    ![Dashboard components](./media/tutorial-define-device-type/dashboardcomponents1.png)

3. Configure the **Line Chart** component using the information in the following table:

    | Setting      | Value       |
    | ------------ | ----------- |
    | Title        | Temperature |
    | Time Range   | Past 30 minutes |
    | Measurements | temperature (choose **Visibility** next to **temperature**) |

    ![Line chart settings](./media/tutorial-define-device-type/linechartsettings.png)

    Then choose **Save**.

4. Configure the **Event Chart** component using the information in the following table:

    | Setting      | Value       |
    | ------------ | ----------- |
    | Title        | Events |
    | Time Range   | Past 30 minutes |
    | Measurements | Fan Motor Error (choose **Visibility** next to **Fan Motor Error**) |

    ![Line chart settings](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Then choose **Save**.

5. Configure the **State Chart** component using the information in the following table:

    | Setting      | Value       |
    | ------------ | ----------- |
    | Title        | Fan Mode |
    | Time Range   | Past 30 minutes |
    | Measurements | Fan Mode (choose **Visibility** next to **Fan Mode**) |

    ![Line chart settings](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Then choose **Save**.

6. To add the set temperature setting to the dashboard, choose **Settings and Properties**:

    ![Dashboard components](./media/tutorial-define-device-type/dashboardcomponents4.png)

7. Configure the **Settings and Properties** component using the information in the following table:

    | Setting                 | Value         |
    | ----------------------- | ------------- |
    | Title                   | Set target temperature |
    | Settings and Properties | Set Temperature |

    ![Serial number property settings](./media/tutorial-define-device-type/propertysettings3.png)

    Then choose **Save**.

8. To add to the device serial number to the dashboard, choose **Settings and Properties**:

    ![Dashboard components](./media/tutorial-define-device-type/dashboardcomponents3.png)

9. Configure the **Settings and Properties** component using the information in the following table:

    | Setting                 | Value         |
    | ----------------------- | ------------- |
    | Title                   | Serial number |
    | Settings and Properties | Serial Number |

    ![Serial number property settings](./media/tutorial-define-device-type/propertysettings1.png)

    Then choose **Save**.

10. To add to the device firmware version to the dashboard, choose **Settings and Properties**:

    ![Dashboard components](./media/tutorial-define-device-type/dashboardcomponents4.png)

11. Configure the **Settings and Properties** component using the information in the following table:

    | Setting                 | Value            |
    | ----------------------- | ---------------- |
    | Title                   | Firmware version |
    | Settings and Properties | Firmware Version |

    ![Serial number property settings](./media/tutorial-define-device-type/propertysettings2.png)

    Then choose **Save**.

12. To view the dashboard as an operator, switch off **Design Mode** on the top right of the page.

## Next steps

In this tutorial, you learned how to:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Create a new device template
> * Add telemetry to your device
> * View simulated telemetry
> * Define device events
> * View simulated events
> * Define your state
> * View simulated state
> * Use device properties
> * Use device settings

Now that you have defined a device template in your Azure IoT Central application, here are the suggested next steps:

* [Configure rules and actions for your device](tutorial-configure-rules.md)
* [Customize the operator's views](tutorial-customize-operator.md)