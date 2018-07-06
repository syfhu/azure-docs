---
title: Overview of Azure Service Bus fundamentals | Microsoft Docs
description: An introduction to using Service Bus to connect Azure applications to other software.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt

ms.service: service-bus-messaging
ms.topic: get-started-article
ms.date: 05/23/2018
ms.author: sethm

---

# Azure Service Bus

Whether an application or service runs in the cloud or on premises, it often needs to interact with other applications or services. To provide a broadly useful way to do this, Microsoft Azure offers Service Bus. This article looks at this technology, describing what it is and why you might want to use it.

## Service Bus fundamentals

Different situations call for different styles of communication. Sometimes, letting applications send and receive messages through a simple queue is the best solution. In other situations, an ordinary queue isn't enough; a queue with a publish-and-subscribe mechanism is better. In some cases, all that's needed is a connection between applications, and queues are not required. Azure Service Bus provides all three options, enabling your applications to interact in several different ways.

Service Bus is a multi-tenant cloud service, which means that the service is shared by multiple users. Each user, such as an application developer, creates a *namespace*, then defines the communication mechanisms needed within that namespace. Figure 1 shows this architecture:

![][1]

**Figure 1: Service Bus provides a multi-tenant service for connecting applications through the cloud.**

Within a namespace, you can use one or more instances of three different communication mechanisms, each of which connects applications in a different way. The choices are:

* *Queues*, which allow one-directional communication. Each queue acts as an intermediary (sometimes called a *broker*) that stores sent messages until they are received. Each message is received by a single recipient.
* *Topics*, which provide one-directional communication using *subscriptions*. A single topic can have multiple subscriptions. Like a queue, a topic acts as a broker, but each subscription can optionally use a filter to receive only messages that match specific criteria.
* *Relays*, which provide bi-directional communication. Unlike queues and topics, a relay doesn't store in-flight messages; it's not a broker. Instead, it just passes them on to the destination application.

When you create a queue, topic, or relay, you give it a name. Combined with whatever you called your namespace, this name creates a unique identifier for the object. Applications can provide this name to Service Bus, then use that queue, topic, or relay to communicate with each other. 

To use any of these objects in the relay scenario, Windows applications can use Windows Communication Foundation (WCF). This service is known as [WCF Relay](../service-bus-relay/relay-what-is-it.md). For queues and topics, Windows applications can use Service Bus-defined messaging APIs. To make these objects easier to use from non-Windows applications, Microsoft provides SDKs for Java, Node.js, and other languages. You can also access queues and topics using [REST APIs](/rest/api/servicebus/) over HTTP(s). 

It's important to understand that even though Service Bus itself runs in the cloud (that is, in Microsoft Azure datacenters), applications that use it can run anywhere. You can use Service Bus to connect applications running on Azure, for example, or applications running inside your own datacenter. You can also use it to connect an application running on Azure or another cloud platform, to an on-premises application or with tablets and phones. Service Bus is a communication mechanism in the cloud that's accessible from pretty much anywhere. How you use it depends on what your applications need to do.

## Queues

Suppose you decide to connect two applications using a Service Bus queue. Figure 2 illustrates this situation:

![][2]

**Figure 2: Service Bus queues provide one-way asynchronous queuing.**

A sender sends a message to a Service Bus queue, and a receiver consumes that message at some later time. A queue can have just a single receiver, as Figure 2 shows. Or, multiple applications can read from the same queue. In the latter situation, each message is read by just one receiver. For a multi-cast service, you should use a topic instead.

Each message has two parts: a set of properties, a key/value pair, and a message payload. The payload can be binary, text, or even XML. How they're used depends on what an application is trying to do. For example, an application sending a message about a recent sale might include the properties **Seller="Ava"** and **Amount=10000**. The message body might contain a scanned image of the sale's signed contract or, if there isn't one, remain empty.

A receiver can read a message from a Service Bus queue in two different ways. The first option, called *[ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode)*, receives a message from the queue and immediately deletes it. This option is simple, but if the receiver crashes before it finishes processing the message, the message is lost. Because it's been removed from the queue, no other receiver can access it. 

The second option, *[PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)*, is meant to help with this problem. Like **ReceiveAndDelete**, a **PeekLock** read removes a message from the queue. It doesn't delete the message, however. Instead, it locks the message, making it invisible to other receivers, then waits for one of three events:

* If the receiver processes the message successfully, it calls [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync), and the queue deletes the message. 
* If the receiver decides that it can't process the message successfully, it calls [Abandon()](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync). The queue then removes the lock from the message and makes it available to other receivers.
* If the receiver calls neither of these methods within a configurable period of time (by default, 60 seconds), the queue assumes the receiver has failed. In this case, it behaves as if the receiver had called **Abandon**, making the message available to other receivers.

Note what can happen here: the same message might be delivered twice, perhaps to two different receivers. Applications using Service Bus queues must be prepared for this event. To make duplicate detection easier, each message has a unique [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid#Microsoft_Azure_ServiceBus_Message_MessageId) property that by default stays the same no matter how many times the message is read from a queue. 

Queues are useful in quite a few situations. They enable applications to communicate even if both apps are not running at the same time, something that's especially handy with batch and mobile applications. A queue with multiple receivers also provides automatic load balancing, since sent messages are spread across these receivers.

## Topics

Useful as they are, queues aren't always the right solution. Sometimes, topics are better. Figure 3 illustrates this idea:

![][3]

**Figure 3: Based on the filter a subscribing application specifies, it can receive some or all the messages sent to a Service Bus topic.**

A *topic* is similar in many ways to a queue. Senders submit messages to a topic in the same way that they submit messages to a queue, and those messages look the same as with queues. The difference is that topics enable each receiving application to create its own *subscription* and optionally define a *filter*. A subscriber receives a copy of each message in the topic, but by using a filter, can then receive only the messages that match that filter. For example, Figure 3 shows a sender and a topic with three subscribers, each with its own filter:

* Subscriber 1 receives only messages that contain the property **Seller="Ava"**.
* Subscriber 2 receives messages that contain the property **Seller="Ruby"** and/or contain an **Amount** property whose value is greater than 100,000. Perhaps Ruby is the sales manager, so she wants to see both her own sales and all large sales regardless of who makes them.
* Subscriber 3 has set its filter to **True**, which means that it receives all messages. For example, this application might be responsible for maintaining an audit trail; therefore it needs to see all the messages.

As with queues, subscribers to a topic can read messages using either [ReceiveAndDelete or PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode). Unlike queues, however, a single message sent to a topic can be received by multiple subscriptions. This approach, commonly called *publish and subscribe* (or *pub/sub*), is useful whenever multiple applications are interested in the same messages. By defining the right filter, each subscriber can tap into just the part of the message stream that it needs to see.

## Relays

Both queues and topics provide one-way asynchronous communication through a broker. Traffic flows in just one direction, and there's no direct connection between senders and receivers. But what if you don't want this connection? Suppose your applications need to both send and receive messages, or perhaps you want a direct link between them and you don't need a broker to store messages. To address scenarios such as this, Service Bus provides *relays*, as Figure 4 shows:

![][4]

**Figure 4: Service Bus Relay provides synchronous, two-way communication between applications.**

The obvious question to ask about relays is this: why would I use one? Even if I don't need queues, why make applications communicate via a cloud service rather than just interact directly? The answer is that talking directly can be harder than you might think.

Suppose you want to connect two on-premises applications, both running inside corporate datacenters. Each of these applications sits behind a firewall, and each datacenter probably uses network address translation (NAT). The firewall blocks incoming data on all but a few ports, and NAT implies that the computer on which each application is running doesn't have a fixed IP address that you can reach directly from outside the datacenter. Without some extra help, connecting these applications over the public internet is problematic.

A Service Bus relay can help. To communicate bi-directionally through a relay, each application establishes an outbound TCP connection with Service Bus, then keeps it open. All communication between the two applications travels over these connections. Because each connection was established from inside the datacenter, the firewall allows incoming traffic to each application without opening new ports. This approach also gets around the NAT problem, because each application has a consistent endpoint in the cloud throughout the communication. By exchanging data through the relay, the applications can avoid the problems that would otherwise make communication difficult. 

To use Service Bus relays, applications rely on the Windows Communication Foundation (WCF). Service Bus provides WCF bindings that make it straightforward for Windows applications to interact via relays. Applications that already use WCF can typically specify one of these bindings, then talk to each other through a relay. Unlike queues and topics, however, using relays from non-Windows applications, while possible, requires some programming effort; no standard libraries are provided.

Unlike queues and topics, applications don't explicitly create relays. Instead, when an application that wishes to receive messages establishes a TCP connection with Service Bus, a relay is created automatically. When the connection is dropped, the relay is deleted. To enable an application to find the relay created by a specific listener, Service Bus provides a registry that enables applications to locate a specific relay by name.

Relays are the right solution when you need direct communication between applications. For example, consider an airline reservation system running in an on-premises datacenter that must be accessed from check-in kiosks, mobile devices, and other computers. Applications running on all these systems could rely on Service Bus relays in the cloud to communicate, wherever they might be running.

## Summary

Connecting applications has always been part of building complete solutions, and the range of scenarios that require applications and services to communicate with each other is set to increase as more applications and devices are connected to the internet. By providing cloud-based technologies for achieving communication through queues, topics, and relays, Service Bus aims to make this essential function easier to implement and more broadly available.

## Next steps

Now that you've learned the fundamentals of Azure Service Bus, follow these links to learn more.

* How to use [Service Bus queues](service-bus-dotnet-get-started-with-queues.md)
* How to use [Service Bus topics](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* How to use [Service Bus relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
* [Service Bus samples](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
