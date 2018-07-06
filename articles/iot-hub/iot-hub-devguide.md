---
title: Developer guide for Azure IoT Hub | Microsoft Docs
description: The Azure IoT Hub developer guide includes discussions of endpoints, security, the identity registry, device management, direct methods, device twins, file uploads, jobs, the IoT Hub query language, and messaging.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
---

# Azure IoT Hub developer guide

Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Azure IoT Hub provides you with:

* Secure communications by using per-device security credentials and access control.
* Multiple device-to-cloud and cloud-to-device hyper-scale communication options.
* Queryable storage of per-device state information and meta-data.
* Easy device connectivity with device libraries for the most popular languages and platforms.

This IoT Hub developer guide includes the following articles:

* [Device-to-cloud communication guidance][lnk-d2c-guidance] helps you choose between device-to-cloud messages, device twin's reported properties, and file upload.
* [Cloud-to-device communication guidance][lnk-c2d-guidance] helps you choose between direct methods, device twin's desired properties, and cloud-to-device messages.
* [Device-to-cloud and cloud-to-device messaging with IoT Hub][devguide-messaging] describes the messaging features (device-to-cloud and cloud-to-device) that IoT Hub exposes.
  * [Send device-to-cloud messages to IoT Hub][devguide-messages-d2c].
  * [Read device-to-cloud messages from the built-in endpoint][devguide-builtin].
  * [Use custom endpoints and routing rules for device-to-cloud messages][devguide-custom].
  * [Send cloud-to-device messages from IoT Hub][devguide-messages-c2d].
  * [Create and read IoT Hub messages][devguide-format].
* [Upload files from a device][devguide-upload] describes how you can upload files from a device. The article also includes information about topics such as the notifications the upload process can send.
* [Manage device identities in IoT Hub][devguide-identities] describes what information each IoT hub's identity registry stores. The article also describes how you can access and modify it.
* [Control access to IoT Hub][devguide-security] describes the security model used to grant access to IoT Hub functionality for both devices and cloud components. The article includes information about using tokens and X.509 certificates, and details of the permissions you can grant.
* [Use device twins to synchronize state and configurations][devguide-device-twins] describes the *device twin* concept. The article also descibes the functionality device twins expose, such as synchronizing a device with its device twin. The article includes information about the data stored in a device twin.
* [Invoke a direct method on a device][devguide-directmethods] describes the lifecycle of a direct method. The article describes how to invoke methods on a device from your back-end app and handle the direct method on your device.
* [Schedule jobs on multiple devices][devguide-jobs] describes how you can schedule jobs on multiple devices. The article describes how to submit jobs that perform tasks as executing a direct method, updating a device using a device twin. It also describes how to query the status of a job.
* [Reference - choose a communication protocol][devguide-protocol] describes the communication protocols that IoT Hub supports for device communication and lists the ports that should be open.
* [Reference - IoT Hub endpoints][devguide-endpoints] describes the various endpoints that each IoT hub exposes for runtime and management operations. The article also describes how you can create additional endpoints in your IoT hub, and how to use a field gateway to enable connectivity to your IoT Hub endpoints in non-standard scenarios.
* [Reference - IoT Hub query language for device twins, jobs, and message routing][devguide-query] describes that IoT Hub query language that enables you to retrieve information from your hub about your device twins and jobs.
* [Reference - quotas and throttling][devguide-quotas] summarizes the quotas set in the IoT Hub service and the throttling that occurs when you exceed a quota.
* [Reference - pricing][devguide-pricing] provides general information on different SKUs and pricing for IoT Hub and details on how the various IoT Hub functionalities are metered as messages by IoT Hub.
* [Reference - Device and service SDKs][devguide-sdks] lists the Azure IoT SDKs for developing device and service apps that interact with your IoT hub. The article includes links to online API documentation.
* [Reference - IoT Hub MQTT support][devguide-mqtt] provides detailed information about how IoT Hub supports the MQTT protocol. The article describes the support for the MQTT protocol built-in to the Azure IoT SDKs and provides information about using the MQTT protocol directly.
* [Glossary][devguide-glossary] a list of common IoT Hub-related terms.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
