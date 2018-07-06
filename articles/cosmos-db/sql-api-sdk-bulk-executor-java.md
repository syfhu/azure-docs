---
title: 'Azure Cosmos DB: bulk executor Java API, SDK & resources | Microsoft Docs'
description: Learn all about the bulk executor Java API and SDK including release dates, retirement dates, and changes made between each version of the Azure Cosmos DB bulk executor Java SDK.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun

ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/07/2018
ms.author: ramkris

---

# Java bulk executor library: Download information

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET Change Feed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Resource Provider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [bulk executor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [bulk executor - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Description**</td><td>The bulk executor library allows client applications to perform bulk operations in Azure Cosmos DB accounts. bulk executor library provides BulkImport, and BulkUpdate namespaces. The BulkImport module can bulk ingest documents in an optimized way such that the throughput provisioned for a collection is consumed to its maximum extent. The BulkUpdate module can bulk update existing data in Azure Cosmos DB containers as patches.</td></tr>

<tr><td>**SDK download**</td><td>[Maven](https://search.maven.org/#search%7Cga%7C1%7Cdocumentdb-bulkexecutor)</td></tr>

<tr><td>**BulkExecutor library in GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started)</td></tr>

<tr><td>**API documentation**</td><td>[.Net API reference documentation](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)</td></tr>

<tr><td>**Get started**</td><td>[Get started with the bulk executor library Java SDK](bulk-executor-java.md)</td></tr>

<tr><td>**Minimum supported runtime**</td><td>JDK 7</td></tr>
</table></br>

