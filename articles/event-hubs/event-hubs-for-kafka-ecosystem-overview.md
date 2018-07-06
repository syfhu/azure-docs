---
title: Azure Event Hubs for Kafka ecosystems | Microsoft Docs
description: Overview and introduction to Kafka enabled Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: djrosanova
manager: timlt

ms.service: event-hubs
ms.topic: article
ms.date: 05/07/2018
ms.author: darosa

---
# Event Hubs for Kafka ecosystems

Event Hubs for Kafka ecosystems provides a Kafka endpoint that can be used by your existing Kafka based applications as an alternative to running your own Kafka cluster. Event Hubs for Kafka ecosystem supports [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) and newer client versions, and works with your existing Kafka applications, including MirrorMaker. Change your connection string and start streaming events from your applications that use the Kafka protocol into Event Hubs.

## What does Event Hubs for Kafka ecosystems provide?

Event Hubs for Kafka ecosystem provides a protocol head on top of Azure Event Hubs that is binary compatible with Kafka versions 1.0 and later for both reading from and writing to Kafka topics. Conceptually Kafka and Event Hubs are nearly identical: they are both partitioned logs built for streaming data. The following table maps concepts between Kafka and Event Hubs.

### Kafka and Event Hub conceptual mapping

| Kafka Concept | Event Hubs Concept|
| --- | --- |
| Cluster | Namespace |
| Topic | Event Hubs |
| Partition | Partition|
| Consumer Group | Consumer Group |
| Offset | Offset|

### Key differences between Kafka and Event Hubs

While [Apache Kafka](https://kafka.apache.org/) is software, which you can run wherever you choose, Event Hubs is a cloud service similar to Azure Blob Storage. There are no servers or networks to manage and no brokers to configure. You create a namespace, which is an FQDN in which your topics live, and then create Event Hubs or topics within that namespace. For more information about Event Hubs and namespaces, see [What is Event Hubs](event-hubs-what-is-event-hubs.md). As a cloud service, Event Hubs uses a single stable virtual IP address as the endpoint, so clients do not need to know about the brokers or machines within a cluster. 

Scale in Event Hubs is controlled by how many throughput units you purchase, with each throughput unit entitling you to 1 MB per second, or 1000 events per second of ingress. By default, Event Hubs scales up throughput units when you reach your limit with the [Auto-Inflate](event-hubs-auto-inflate.md) feature; this feature also works with Event Hubs for Kafka ecosystems. 

### Security and authentication

Azure Event Hubs requires SSL or TLS for all communication and uses Shared Access Signatures (SAS) for authentication. This requirement is also true for a Kafka endpoint within Event Hubs. For compatibility with Kafka, Event Hubs uses SASL PLAIN for authentication and SASL SSL for transport security. For more information about security in Event Hubs, see [Event Hubs authentication and security](event-hubs-authentication-and-security-model-overview.md).

## Other Event Hubs features available for Kafka

Event Hubs for Kafka ecosystem enables you to write with one protocol and read with another, so that your current Kafka producers can continue publishing via Kafka, and you can add readers with Event Hubs, such as Azure Stream Analytics or Azure Functions. Additionally, Event Hubs features such as [Capture](event-hubs-capture-overview.md) and [Geo Disaster-Recovery](event-hubs-geo-dr.md) also work with Event Hubs for Kafka ecosystems.

## Features that are not supported in the preview

For the public preview of the Event Hubs for Kafka ecosystems integration, the following Kafka features are not supported:

*	Idempotent producer
*	Transaction
*	Compression
*	Size-based retention
*	Log compaction
*	Adding partitions to an existing topic
*	HTTP Kafka API support
*	Kafka Connect
*	Kafka Streams

## Next steps

This article provided an introduction to Event Hubs for Kafka ecosystems. To learn more, see the following links:

* [How to create Kafka enabled Event Hubs](event-hubs-create-kafka-enabled.md)
* [Stream into Event Hubs from your Kafka applications](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* Get started with an [Event Hubs tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs FAQ](event-hubs-faq.md)

 
 

