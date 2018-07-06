---
title: Azure IoT device management with iothub-explorer | Microsoft Docs
description: Use the iothub-explorer CLI tool for Azure IoT Hub device management, featuring the Direct methods and the Twin's desired properties management options.
author: rangv
manager: nasing
keywords: azure iot device management, azure iot hub device management, device management iot, iot hub device management
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
---

# Use iothub-explorer for Azure IoT Hub device management

![End-to-end diagram](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer to manage device identities in your IoT hub registry. It comes with management options that you can use to perform various tasks.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Management option          | Task                                                                                                                            |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------|
| Direct methods             | Make a device act such as starting or stopping sending messages or rebooting the device.                                        |
| Twin desired properties    | Put a device into certain states, such as setting an LED to green or setting the telemetry send interval to 30 minutes.         |
| Twin reported properties   | Get the reported state of a device. For example, the device reports the LED is blinking now.                                    |
| Twin tags                  | Store device-specific metadata in the cloud. For example, the deployment location of a vending machine.                         |
| Cloud-to-device messages   | Send notifications to a device. For example, "It is very likely to rain today. Don't forget to bring an umbrella."              |
| Device twin queries        | Query all device twins to retrieve those with arbitrary conditions, such as identifying the devices that are available for use. |

For more detailed explanation on the differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).

Device twins are JSON documents that store device state information (metadata, configurations, and conditions). IoT Hub persists a device twin for each device that connects to it. For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).

## What you learn

You learn using iothub-explorer with various management options on your development machine.

## What you do

Run iothub-explorer with various management options.

## What you need

- Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:
- An active Azure subscription.
- An Azure IoT hub under your subscription.
- A client application that sends messages to your Azure IoT hub.
- Make sure your device is running with the client application during this tutorial.
- iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.

## Connect to your IoT hub

Connect to your IoT hub by running the following command:

```bash
iothub-explorer login <your IoT hub connection string>
```

## Use iothub-explorer with direct methods

Invoke the `start` method in the device app to send messages to your IoT hub by running the following command:

```bash
iothub-explorer device-method <your device Id> start
```

Invoke the `stop` method in the device app to stop sending messages to your IoT hub by running the following command:

```bash
iothub-explorer device-method <your device Id> stop
```

## Use iothub-explorer with twin's desired properties

Set a desired property interval = 3000 by running the following command:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

This property can be read by your device.

## Use iothub-explorer with twin's reported properties

Get the reported properties of the device by running the following command:

```bash
iothub-explorer get-twin <your device id>
```

One of the properties is $metadata.$lastUpdated which shows the last time this device sends or receives a message.

## Use iothub-explorer with twin's tags

Display the tags and properties of the device by running the following command:

```bash
iothub-explorer get-twin <your device id>
```

Add a field role = temperature&humidity to the device by running the following command:

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"
```

## Use iothub-explorer with Cloud-to-device messages

Send a "Hello World" message to the device by running the following command:

```bash
iothub-explorer send <device-id> "Hello World"
```

See [Use iothub-explorer to send and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.

## Use iothub-explorer with device twins queries

Query devices with a tag of role = 'temperature&humidity' by running the following command:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Query all devices except those with a tag of role = 'temperature&humidity' by running the following command:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## Next steps

You've learned how to use iothub-explorer with various management options.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
