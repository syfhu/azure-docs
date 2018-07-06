---
title: Azure Event Hubs bindings for Azure Functions
description: Understand how to use Azure Event Hubs bindings in Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: azure functions, functions, event processing, dynamic compute, serverless architecture

ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra

---
# Azure Event Hubs bindings for Azure Functions

This article explains how to work with [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions. Azure Functions supports trigger and output bindings for Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## Packages - Functions 1.x

For Azure Functions version 1.x, the Event Hubs bindings are provided in the [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet package, version 2.x.
Source code for the package is in the [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub repository.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## Packages - Functions 2.x

For Functions 2.x, use the [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) package, version 3.x.
Source code for the package is in the [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub repository.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## Trigger

Use the Event Hubs trigger to respond to an event sent to an event hub event stream. You must have read access to the event hub to set up the trigger.

When an Event Hubs trigger function is triggered, the message that triggers it is passed into the function as a string.

## Trigger - scaling

Each instance of an Event Hub-Triggered Function is backed by only 1 EventProcessorHost (EPH) instance. Event Hubs ensures that only 1 EPH can get a lease on a given partition.

For example, suppose we begin with the following setup and assumptions for an Event Hub:

1. 10 partitions.
1. 1000 events distributed evenly across all partitions => 100 messages in each partition.

When your function is first enabled, there is only 1 instance of the function. Let's call this function instance Function_0. Function_0 will have 1 EPH that manages to get a lease on all 10 partitions. It will start reading events from partitions 0-9. From this point forward, one of the following will happen:

* **Only 1 function instance is needed** - Function_0 is able to process all 1000 before the Azure Functions' scaling logic kicks in. Hence, all 1000 messages are processed by Function_0.

* **Add 1 more function instance** - Azure Functions' scaling logic determines that Function_0 has more messages than it can process, so a new instance, Function_1, is created. Event Hubs detects that a new EPH instance is trying read messages. Event Hubs will start load balancing the partitions across the EPH instances, e.g., partitions 0-4 are assigned to Function_0 and partitions 5-9 are assigned to Function_1. 

* **Add N more function instances** - Azure Functions' scaling logic determines that both Function_0 and Function_1 have more messages than they can process. It will scale again for Function_2...N, where N is greater than the Event Hub partitions. Event Hubs will load balance the partitions across Function_0...9 instances.

Unique to Azure Functions' current scaling logic is the fact that N is greater than the number of partitions. This is done to ensure that there are always instances of EPH readily available to quickly get a lock on the partition(s) as they become available from other instances. Users are only charged for the resources used when the function instance executes, and are not charged for this over-provisioning.

If all function executions succeed without errors, checkpoints are added to the associated storage account. When check-pointing succeeds, all 1000 messages should never be retrieved again.

## Trigger - example

See the language-specific example:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### Trigger - C# example

The following example shows a [C# function](functions-dotnet-class-library.md) that logs the message body of the event hub trigger.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

To get access to [event metadata](#trigger---event-metadata) in function code, bind to an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (requires a using statement for `Microsoft.ServiceBus.Messaging`). You can also access the same properties by using binding expressions in the method signature.  The following example shows both ways to get the same data:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, 
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

To receive events in a batch, make `string` or `EventData` an array:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### Trigger - C# script example

The following example shows an event hub trigger binding in a *function.json* file and a [C# script function](functions-reference-csharp.md) that uses the binding. The function logs the message body of the event hub trigger.

The following examples show Event Hubs binding data in the *function.json* file. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Here's the C# script code:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

To get access to [event metadata](#trigger---event-metadata) in function code, bind to an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (requires a using statement for `Microsoft.ServiceBus.Messaging`). You can also access the same properties by using binding expressions in the method signature.  The following example shows both ways to get the same data:

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

To receive events in a batch, make `string` or `EventData` an array:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### Trigger - F# example

The following example shows an event hub trigger binding in a *function.json* file and an [F# function](functions-reference-fsharp.md) that uses the binding. The function logs the message body of the event hub trigger.

The following examples show Event Hubs binding data in the *function.json* file. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Here's the F# code:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### Trigger - JavaScript example

The following example shows an event hub trigger binding in a *function.json* file and a [JavaScript function](functions-reference-node.md) that uses the binding. The function reads [event metadata](#trigger---event-metadata) and logs the message.

The following examples show Event Hubs binding data in the *function.json* file. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Here's the JavaScript code:

```javascript
module.exports = function (context, eventHubMessage) {
    context.log('Event Hubs trigger function processed message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);
     
    context.done();
};
```

To receive events in a batch, set `cardinality` to `many` in the *function.json* file, as shown in the following examples. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Here's the JavaScript code:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);
    
    eventHubMessages.forEach(message => {
        context.log(`Processed message ${message}`);
    });

    context.done();
};
```

## Trigger - attributes

In [C# class libraries](functions-dotnet-class-library.md), use the [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) attribute.

The attribute's constructor takes the name of the event hub, the name of the consumer group, and the name of an app setting that contains the connection string. For more information about these settings, see the [trigger configuration section](#trigger---configuration). Here's an `EventHubTriggerAttribute` attribute example:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

For a complete example, see [Trigger - C# example](#trigger---c-example).

## Trigger - configuration

The following table explains the binding configuration properties that you set in the *function.json* file and the `EventHubTrigger` attribute.

|function.json property | Attribute property |Description|
|---------|---------|----------------------|
|**type** | n/a | Must be set to `eventHubTrigger`. This property is set automatically when you create the trigger in the Azure portal.|
|**direction** | n/a | Must be set to `in`. This property is set automatically when you create the trigger in the Azure portal. |
|**name** | n/a | The name of the variable that represents the event item in function code. | 
|**path** |**EventHubName** | Functions 1.x only. The name of the event hub.  | 
|**eventHubName** |**EventHubName** | Functions 2.x only. The name of the event hub.  |
|**consumerGroup** |**ConsumerGroup** | An optional property that sets the [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used to subscribe to events in the hub. If omitted, the `$Default` consumer group is used. | 
|**cardinality** | n/a | For Javascript. Set to `many` in order to enable batching.  If omitted or set to `one`, single message passed to function. | 
|**connection** |**Connection** | The name of an app setting that contains the connection string to the event hub's namespace. Copy this connection string by clicking the **Connection Information** button for the [namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), not the event hub itself. This connection string must have at least read permissions to activate the trigger.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## Trigger - event metadata

The Event Hubs trigger provides several [metadata properties](functions-triggers-bindings.md#binding-expressions---trigger-metadata). These properties can be used as part of binding expressions in other bindings or as parameters in your code. These are properties of the [EventData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.eventdata) class.

|Property|Type|Description|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|The `PartitionContext` instance.|
|`EnqueuedTimeUtc`|`DateTime`|The enqueued time in UTC.|
|`Offset`|`string`|The offset of the data relative to the Event Hub partition stream. The offset is a marker or identifier for an event within the Event Hubs stream. The identifier is unique within a partition of the Event Hubs stream.|
|`PartitionKey`|`string`|The partition to which event data should be sent.|
|`Properties`|`IDictionary<String,Object>`|The user properties of the event data.|
|`SequenceNumber`|`Int64`|The logical sequence number of the event.|
|`SystemProperties`|`IDictionary<String,Object>`|The system properties, including the event data.|

See [code examples](#trigger---example) that use these properties earlier in this article.

## Trigger - host.json properties

The [host.json](functions-host-json.md#eventhub) file contains settings that control Event Hubs trigger behavior.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## Output

Use the Event Hubs output binding to write events to an event stream. You must have send permission to an event hub to write events to it.

## Output - example

See the language-specific example:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### Output - C# example

The following example shows a [C# function](functions-dotnet-class-library.md) that writes a message to an event hub, using the method return value as the output:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### Output - C# script example

The following example shows an event hub trigger binding in a *function.json* file and a [C# script function](functions-reference-csharp.md) that uses the binding. The function writes a message to an event hub.

The following examples show Event Hubs binding data in the *function.json* file. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Here's C# script code that creates one message:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Here's C# script code that creates multiple messages:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### Output - F# example

The following example shows an event hub trigger binding in a *function.json* file and an [F# function](functions-reference-fsharp.md) that uses the binding. The function writes a message to an event hub.

The following examples show Event Hubs binding data in the *function.json* file. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Here's the F# code:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### Output - JavaScript example

The following example shows an event hub trigger binding in a *function.json* file and a [JavaScript function](functions-reference-node.md) that uses the binding. The function writes a message to an event hub.

The following examples show Event Hubs binding data in the *function.json* file. The first example is for Functions 1.x, and the second one is for Functions 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Here's JavaScript code that sends a single message:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Here's JavaScript code that sends multiple messages:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## Output - attributes

For [C# class libraries](functions-dotnet-class-library.md), use the [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) attribute.

The attribute's constructor takes the name of the event hub and the name of an app setting that contains the connection string. For more information about these settings, see [Output - configuration](#output---configuration). Here's an `EventHub` attribute example:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

For a complete example, see [Output - C# example](#output---c-example).

## Output - configuration

The following table explains the binding configuration properties that you set in the *function.json* file and the `EventHub` attribute.

|function.json property | Attribute property |Description|
|---------|---------|----------------------|
|**type** | n/a | Must be set to "eventHub". |
|**direction** | n/a | Must be set to "out". This parameter is set automatically when you create the binding in the Azure portal. |
|**name** | n/a | The variable name used in function code that represents the event. | 
|**path** |**EventHubName** | Functions 1.x only. The name of the event hub.  | 
|**eventHubName** |**EventHubName** | Functions 2.x only. The name of the event hub.  |
|**connection** |**Connection** | The name of an app setting that contains the connection string to the event hub's namespace. Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself. This connection string must have send permissions to send the message to the event stream.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## Output - usage

In C# and C# script, send messages by using a method parameter such as `out string paramName`. In C# script, `paramName` is the value specified in the `name` property of *function.json*. To write multiple messages, you can use `ICollector<string>` or
`IAsyncCollector<string>` in place of `out string`.

In JavaScript, access the output event by using `context.bindings.<name>`. `<name>` is the value specified in the `name` property of *function.json*.

## Exceptions and return codes

| Binding | Reference |
|---|---|
| Event Hub | [Operations Guide](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## Next steps

> [!div class="nextstepaction"]
> [Learn more about Azure functions triggers and bindings](functions-triggers-bindings.md)
