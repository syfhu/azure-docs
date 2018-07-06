---
title: Introduction to the Azure Cosmos DB Table API | Microsoft Docs
description: Learn how you can use Azure Cosmos DB to store and query massive volumes of key-value data with low latency by using the popular OSS MongoDB APIs.
services: cosmos-db
author: SnehaGunda
manager: kfile

ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: na
ms.topic: overview
ms.date: 11/20/2017
ms.author: sngun

---
# Introduction to Azure Cosmos DB: Table API

[Azure Cosmos DB](introduction.md) provides the Table API for applications that are written for Azure Table storage and that need premium capabilities like:

* [Turnkey global distribution](distribute-data-globally.md).
* [Dedicated throughput](partition-data.md) worldwide.
* Single-digit millisecond latencies at the 99th percentile.
* Guaranteed high availability.
* [Automatic secondary indexing](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Applications written for Azure Table storage can migrate to Azure Cosmos DB by using the Table API with no code changes and take advantage of premium capabilities. The Table API has client SDKs available for .NET, Java, Python, and Node.js.

We recommend that you watch the following video, where Aravind Krishna explains how to get started with the Azure Cosmos DB Table API:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## Table offerings
If you currently use Azure Table Storage, you gain the following benefits by moving to the Azure Cosmos DB Table API:

| | Azure Table storage | Azure Cosmos DB Table API |
| --- | --- | --- |
| Latency | Fast, but no upper bounds on latency. | Single-digit millisecond latency for reads and writes, backed with <10-ms latency reads and <15-ms latency writes at the 99th percentile, at any scale, anywhere in the world. |
| Throughput | Variable throughput model. Tables have a scalability limit of 20,000 operations/s. | Highly scalable with [dedicated reserved throughput per table](request-units.md) that's backed by SLAs. Accounts have no upper limit on throughput and support >10 million operations/s per table. |
| Global distribution | Single region with one optional readable secondary read region for high availability. You can't initiate failover. | [Turnkey global distribution](distribute-data-globally.md) from one to 30+ regions. Support for [automatic and manual failovers](regional-failover.md) at any time, anywhere in the world. |
| Indexing | Only primary index on PartitionKey and RowKey. No secondary indexes. | Automatic and complete indexing on all properties, no index management. |
| Query | Query execution uses index for primary key, and scans otherwise. | Queries can take advantage of automatic indexing on properties for fast query times. |
| Consistency | Strong within primary region. Eventual within secondary region. | [Five well-defined consistency levels](consistency-levels.md) to trade off availability, latency, throughput, and consistency based on your application needs. |
| Pricing | Storage-optimized. | Throughput-optimized. |
| SLAs | 99.99% availability. | 99.99% availability SLA for all single region accounts and all multi-region accounts with relaxed consistency, and 99.999% read availability on all multi-region database accounts [Industry-leading comprehensive SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) on general availability. |

## Get started

Create an Azure Cosmos DB account in the [Azure portal](https://portal.azure.com). Then get started with our [Quick Start for Table API by using .NET](create-table-dotnet.md). 

> [!IMPORTANT]
> If you created a Table API account during the preview, please create a [new Table API account](create-table-dotnet.md#create-a-database-account) to work with the generally available Table API SDKs.
>

## Next steps

Here are a few pointers to get you started:
* [Build a .NET application by using the Table API](create-table-dotnet.md)
* [Develop with the Table API in .NET](tutorial-develop-table-dotnet.md)
* [Query table data by using the Table API](tutorial-query-table.md)
* [Learn how to set up Azure Cosmos DB global distribution by using the Table API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table .NET API](table-sdk-dotnet.md)
* [Azure Cosmos DB Table Java API](table-sdk-java.md)
* [Azure Cosmos DB Table Node.js API](table-sdk-nodejs.md)
* [Azure Cosmos DB Table SDK for Python](table-sdk-python.md)

