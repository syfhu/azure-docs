---
title: Understand Azure IoT Hub messaging | Microsoft Docs
description: Developer guide - device-to-cloud and cloud-to-device messaging with IoT Hub. Includes information about message formats and supported communications protocols.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
---

# Device-to-cloud and cloud-to-device messaging with IoT Hub

Use IoT Hub messaging to communicate with your devices by:

* Sending [device-to-cloud][lnk-d2c] messages from your devices to your solution back end.
* Sending [cloud-to-device][lnk-c2d] messages from the solution back end to your devices.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Core properties of IoT Hub messaging functionality are the reliability and durability of messages. These properties enable resilience to intermittent connectivity on the device side, and to load spikes in event processing on the cloud side. IoT Hub implements *at least once* delivery guarantees for both device-to-cloud and cloud-to-device messaging.

For an introduction to the capabilities of IoT Hub, see the [Overview of the Azure IoT Hub service][lnk-iot-hub-overview].

## When to use IoT Hub messaging

Use device-to-cloud messages for sending time series telemetry and alerts from your device app, and cloud-to-device messages for one-way notifications to your device app.

* Refer to [Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using device-to-cloud messages, reported properties, or file upload.
* Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using cloud-to-device messages, desired properties, or direct methods.

## Next steps

* Learn about IoT Hub [device-to-cloud messaging][lnk-d2c].
* Learn about IoT Hub [cloud-to-device messaging][lnk-c2d].

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md