---
title: Using Kafka MirrorMaker with Azure Event Hubs for Kafka Ecosystem | Microsoft Docs
description: Use Kafka MirrorMaker to mirror a Kafka cluster in Event Hubs.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt

ms.service: event-hubs
ms.topic: mirror-maker
ms.custom: mvc
ms.date: 05/07/2018
ms.author: bahariri

---

# Using Kafka MirrorMaker with Event Hubs for Kafka ecosystems

> [!NOTE]
> This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs)

One major consideration for modern cloud scale apps is the ability to update, improve, and change infrastructure without interrupting service. This tutorial shows how a Kafka-enabled event hub and Kafka MirrorMaker can integrate an existing Kafka pipeline into Azure by "mirroring" the Kafka input stream in the Event Hubs service. 

An Azure Event Hubs Kafka endpoint enables you to connect to Azure Event Hubs using the Kafka protocol (i.e. Kafka clients). By making minimal changes to a Kafka application, you can connect to Azure Event Hubs and enjoy the benefits of the Azure ecosystem. Kafka enabled Event Hubs currently supports Kafka versions 1.0 and later.

This example shows how to mirror a Kafka broker in a Kafka enabled event hub using Kafka MirrorMaker.

   ![Kafka MirrorMaker with Event Hubs](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

## Prerequisites

To complete this tutorial, make sure you have:

* An Azure subscription. If you do not have one, create a [free account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) before you begin.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * On Ubuntu, run `apt-get install default-jdk` to install the JDK.
    * Be sure to set the JAVA_HOME environment variable to point to the folder where the JDK is installed.
* [Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a Maven binary archive
    * On Ubuntu, you can run `apt-get install maven` to install Maven.
* [Git](https://www.git-scm.com/downloads)
    * On Ubuntu, you can run `sudo apt-get install git` to install Git.

## Create an Event Hubs namespace

An Event Hubs namespace is required to send and receive from any Event Hubs service. See [Creating a Kafka enabled Event Hub](event-hubs-create.md) for instructions on getting an Event Hubs Kafka endpoint. Make sure to copy the Event Hubs connection string for later use.

## Clone the example project

Now that you have a Kafka enabled Event Hubs connection string, clone the Azure Event Hubs repository and navigate to the `mirror-maker` subfolder:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/mirror-maker
```

## Set up a Kafka cluster

Use the [Kafka quickstart guide](https://kafka.apache.org/quickstart) to set up a cluster with the desired settings (or use an existing Kafka cluster).

## Kafka MirrorMaker

Kafka MirrorMaker enables the "mirroring" of a stream. Given source and destination Kafka clusters, MirrorMaker ensures any messages sent to the source cluster are received by both the source and destination clusters. This example shows how to mirror a source Kafka cluster with a destination Kafka-enabled event hub. This scenario can be used to send data from an existing Kafka pipeline to Event Hubs without interrupting the flow of data. 

For more detailed information on Kafka MirrorMaker, see the [Kafka Mirroring/MirrorMaker guide](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

### Configuration

To configure Kafka MirrorMaker, give it a Kafka cluster as its consumer/source and a Kafka-enabled event hub as its producer/destination.

#### Consumer configuration

Update the consumer configuration file `source-kafka.config`, which tells MirrorMaker the properties of the source Kafka cluster.

##### source-kafka.config

```xml
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### Producer configuration

Now update the producer configuration file `mirror-eventhub.config`, which tells MirrorMaker to send the duplicated (or "mirrored") data to the Event Hubs service. Specifically, change `bootstrap.servers` and `sasl.jaas.config` to point to your Event Hubs Kafka endpoint. The Event Hubs service requires secure (SASL) communication, which is achieved by setting the last three properties in the following configuration: 

##### mirror-eventhub.config

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### Run MirrorMaker

Run the Kafka MirrorMaker script from the root Kafka directory using the newly updated configuration files. Make sure to either copy the config files to the root Kafka directory, or update their paths in the following command.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

To verify that events are reaching the Kafka-enabled event hub, see the ingress statistics in the [Azure portal](https://azure.microsoft.com/features/azure-portal/), or run a consumer against the event hub.

With MirrorMaker running, any events sent to the source Kafka cluster are received by both the Kafka cluster and the mirrored Kafka enabled event hub service. By using MirrorMaker and an Event Hubs Kafka endpoint, you can migrate an existing Kafka pipeline to the managed Azure Event Hubs service without changing the existing cluster or interrupting any ongoing data flow.

## Next steps

* [Learn about Event Hubs](event-hubs-what-is-event-hubs.md)
* [Learn about Event Hubs for Kafka ecosystem](event-hubs-for-kafka-ecosystem-overview.md)
* Learn more about [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) to stream events from Kafka on-premises to Kafka enabled event hubs on cloud.
* Learn how to stream into Kafka enabled Event Hubs using [native Kafka applications](event-hubs-quickstart-kafka-enabled-event-hubs.md), [Apache Flink](event-hubs-kafka-flink-tutorial.md), or [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md).
