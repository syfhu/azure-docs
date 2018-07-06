---
title: Managing credentials in the elastic database client library | Microsoft Docs
description: How to set the right level of credentials, admin to read-only, for elastic database apps
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein

---
# Credentials used to access the Elastic Database client library
The [Elastic Database client library](sql-database-elastic-database-client-library.md) uses three different kinds  of credentials to access the [shard map manager](sql-database-elastic-scale-shard-map-management.md). Depending on the need, use the credential with  the lowest level of access possible.

* **Management credentials**: for creating or manipulating a shard map manager. (See the [glossary](sql-database-elastic-scale-glossary.md).) 
* **Access credentials**: to access an existing shard map manager to obtain information about shards.
* **Connection credentials**: to connect to shards. 

See also [Managing databases and logins in Azure SQL Database](sql-database-manage-logins.md). 

## About management credentials
Management credentials are used to create a **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)) object for applications that manipulate shard maps. (For example, see [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [data-dependent routing](sql-database-elastic-scale-data-dependent-routing.md)). The user of the elastic scale client library creates the SQL users and SQL logins and makes sure each is granted the read/write permissions on the global shard map database and all shard databases as well. These credentials are used to maintain the global shard map and the local shard maps when changes to the shard map are performed. For instance, use the management credentials to create the shard map manager object (using **GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.getsqlshardmapmanager), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)): 

```
// Obtain a shard map manager. 
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmAdminConnectionString,ShardMapManagerLoadPolicy.Lazy); 
```

The variable **smmAdminConnectionString** is a connection string that contains the management credentials. The user ID and password provide read/write access to both shard map database and individual shards. The management connection string also includes the server name and database name to identify the global shard map database. Here is a typical connection string for that purpose:

```
"Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 
```

Do not use values in the form of "username@server"—instead just use the "username" value.  This is because credentials must work against both the shard map manager database and individual shards, which may be on different servers.

## Access credentials
When creating a shard map manager in an application that does not administer shard maps, use credentials that have read-only permissions on the global shard map. The information retrieved from the global shard map under these credentials is used for [data-dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and to populate the shard map cache on the client. The credentials are provided through the same call pattern to **GetSqlShardMapManager**: 

```
// Obtain shard map manager. 
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmReadOnlyConnectionString, ShardMapManagerLoadPolicy.Lazy);  
```

Note the use of the **smmReadOnlyConnectionString** to reflect the use of different credentials for this access on behalf of **non-admin** users: these credentials should not provide write permissions on the global shard map. 

## Connection credentials
Additional credentials are needed when using the **OpenConnectionForKey**  ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._list_shard_mapper.openconnectionforkey), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)) method to access a shard associated with a sharding key. These credentials need to provide permissions for read-only access to the local shard map tables residing on the shard. This is needed to perform connection validation for data-dependent routing on the shard. This code snippet allows data access in the context of data-dependent routing: 

```csharp
using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>(targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 
```

In this example, **smmUserConnectionString** holds the connection string for the user credentials. For Azure SQL DB, here is a typical connection string for user credentials: 

```
"User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  
```

As with the admin credentials, do not use values in the form of "username@server". Instead, just use "username".  Also note that the connection string does not contain a server name and database name. That is because the **OpenConnectionForKey** call automatically directs the connection to the correct shard based on the key. Hence, the database name and server name are not provided. 

## See also
[Managing databases and logins in Azure SQL Database](sql-database-manage-logins.md)

[Securing your SQL Database](sql-database-security-overview.md)

[Getting started with Elastic Database jobs](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

