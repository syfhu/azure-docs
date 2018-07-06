---
title: Azure Event Hubs samples | Microsoft Docs
description: Azure Event Hubs samples 
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2018
ms.author: sethm

---

# Event Hubs samples 

The set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/). This article categorizes and describes the samples available, with links to each.

At the time of this writing, Event Hubs samples are located in several different places:

- [MSDN developer code samples](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

For more information about the different versions of the .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).

More samples will be added over time, so check back here frequently for updates.

## .NET Standard

The following samples demonstrate how to send and receive events using the [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for the [.NET Standard library](/dotnet/articles/standard/library).

### Send events 

The [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how to write a .NET Core console application that sends events to an event hub.

### Receive events 

The [Get started receiving with the Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using the Event Processor Host.

## .NET Framework	

These samples demonstrate various other features of Azure Event Hubs, targeting the [.NET Framework library](/dotnet/framework/index).
 
### Notify users of events received

The [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.

### Get started with Event Hubs 

The [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates the basic capabilities of Event Hubs, such as how to create an event hub, send events to an event hub, and consume events using the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).

### Scale out event processing 

The [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how to use the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) to distribute the workload of Event Hubs stream consumption. It shows how to implement the **EventProcessor** and **EventProcessorFactory** objects to manage the event stream. 

## Next steps

Learn more about .NET Framework versions by visiting the following links:

- [Frameworks and targets](/dotnet/articles/standard/frameworks)
- [.NET Framework 4.6 and 4.5](/dotnet/framework/index)

You can learn more about Event Hubs in the following articles:

- [Event Hubs overview](event-hubs-what-is-event-hubs.md)
- [Event Hubs features](event-hubs-features.md)
- [Event Hubs FAQ](event-hubs-faq.md)