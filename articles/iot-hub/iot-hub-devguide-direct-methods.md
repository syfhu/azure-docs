---
title: Understand Azure IoT Hub direct methods | Microsoft Docs
description: Developer guide - use direct methods to invoke code on your devices from a service app.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: nberdy
---

# Understand and invoke direct methods from IoT Hub
IoT Hub gives you the ability to invoke direct methods on devices from the cloud. Direct methods represent a request-reply interaction with a device similar to an HTTP call in that they succeed or fail immediately (after a user-specified timeout). This approach is useful for scenarios where the course of immediate action is different depending on whether the device was able to respond.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Each device method targets a single device. [Jobs][lnk-devguide-jobs] provide a way to invoke direct methods on multiple devices, and schedule method invocation for disconnected devices.

Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.

Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result. For example, interactive control of the device, such as turning on a fan.

Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.

## Method lifecycle
Direct methods are implemented on the device and may require zero or more inputs in the method payload to correctly instantiate. You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`). A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`) or through AMQP links (`IoThub-methodname` and `IoThub-status` application properties). 

> [!NOTE]
> When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in the following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Direct methods are synchronous and either succeed or fail after the timeout period (default: 30 seconds, settable up to 3600 seconds). Direct methods are useful in interactive scenarios where you want a device to act if and only if the device is online and receiving commands. For example, turning on a light from a phone. In these scenarios, you want to see an immediate success or failure so the cloud service can act on the result as soon as possible. The device may return some message body as a result of the method, but it isn't required for the method to do so. There is no guarantee on ordering or any concurrency semantics on method calls.

Direct methods are HTTPS-only from the cloud side, and MQTT or AMQP from the device side.

The payload for method requests and responses is a JSON document up to 128 KB.

## Invoke a direct method from a back-end app
### Method invocation
Direct method invocations on a device are HTTPS calls that comprise:

* The *URI* specific to the device (`{iot hub}/twins/{device id}/methods/`)
* The POST *method*
* *Headers* that contain the authorization, request ID, content type, and content encoding
* A transparent JSON *body* in the following format:

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

Timeout is in seconds. If timeout is not set, it defaults to 30 seconds.

### Response
The back-end app receives a response that comprises:

* *HTTP status code*, which is used for errors coming from the IoT Hub, including a 404 error for devices not currently connected
* *Headers* that contain the ETag, request ID, content type, and content encoding
* A JSON *body* in the following format:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Both `status` and `body` are provided by the device and used to respond with the device's own status code and/or description.

### Method invocation for IoT Edge modules
Invoking direct methods using a module ID is supported in the C# preview SDK (available [here](https://www.nuget.org/packages/Microsoft.Azure.Devices/1.16.0-preview-004)).

For this purpose, use the `ServiceClient.InvokeDeviceMethodAsync()` method and pass in the `deviceId` and `moduleId` as parameters.

## Handle a direct method on a device
### MQTT
#### Method invocation
Devices receive direct method requests on the MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`

The body that the device receives is in the following format:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Method requests are QoS 0.

#### Response
The device sends responses to `$iothub/methods/res/{status}/?$rid={request id}`, where:

* The `status` property is the device-supplied status of method execution.
* The `$rid` property is the request ID from the method invocation received from IoT Hub.

The body is set by the device and can be any status.

### AMQP
#### Method invocation
The device receives direct method requests by creating a receive link on address `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

The AMQP message arrives on the receive link that represents the method request. It contains the following:
* The correlation ID property, which contains a request ID that should be passed back with the corresponding method response
* An application property named `IoThub-methodname`, which contains the name of the method being invoked
* The AMQP message body containing the method payload as JSON

#### Response
The device creates a sending link to return the method response on address `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

The method’s response is returned on the sending link and is structured as follows:
* The correlation ID property, which contains the request ID passed in the method’s request message
* An application property named `IoThub-status`, which contains the user supplied method status
* The AMQP message body containing the method response as JSON

## Additional reference material
Other reference topics in the IoT Hub developer guide include:

* [IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.
* [Throttling and quotas][lnk-quotas] describes the quotas that apply and the throttling behavior to expect when you use IoT Hub.
* [Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.
* [IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.
* [IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.

## Next steps
Now you have learned how to use direct methods, you may be interested in the following IoT Hub developer guide article:

* [Schedule jobs on multiple devices][lnk-devguide-jobs]

If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:

* [Use direct methods][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
