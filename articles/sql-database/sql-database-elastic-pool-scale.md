---
title: Scale elastic pool resources - Azure SQL Database | Microsoft Docs
description: This page describes scaling resources for elastic pools in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab

---
# Scale elastic pool resources in Azure SQL Database

This article describes how to scale the compute and storage resources available for elastic pools and pooled databases in Azure SQL Database. 

## vCore-based purchasing model: Change elastic pool storage size

- Storage can be provisioned up to the max size limit: 
 - For Standard storage, increase or decrease size in 10 GB increments
 - For Premium storage, increase or decrease size in 250 GB increments
- Storage for an elastic pool can be provisioned by increasing or decreasing its max size.
- The price of storage for an elastic pool is the storage amount multiplied by the storage unit price of the service tier. For details on the price of extra storage, see [SQL Database pricing](https://azure.microsoft.com/pricing/details/sql-database/).

## vCore-based purchasing model: Change elastic pool compute resources (vCores)

You can increase or decrease the performance level to an elastic pool based on resource needs using the [Azure portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), the [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), or the [REST API](/rest/api/sql/elasticpools/update).

- When rescaling pool vCores, database connections are briefly dropped. This is the same behavior as occurs when rescaling DTUs for a single database (not in a pool). For details on the duration and impact of dropped connections for a database during rescaling operations, see [Rescaling DTUs for a single database](#single-database-change-storage-size). 
- The duration to rescale pool vCores can depend on the total amount of storage space used by all databases in the pool. In general, the rescaling latency averages 90 minutes or less per 100 GB. For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for rescaling the pool is 3 hours or less. In some cases within the Standard or Basic tier, the rescaling latency can be under five minutes regardless of the amount of space used.
- In general, the duration to change the min vCores per database or max vCores per database is five minutes or less.
- When downsizing pool vCores, the pool used space must be smaller than the maximum allowed size of the target service tier and pool vCores.

## DTU-based purchasing model: Change elastic pool storage size

- The eDTU price for an elastic pool includes a certain amount of storage at no additional cost. Extra storage beyond the included amount can be provisioned for an additional cost up to the max size limit in increments of 250 GB up to 1 TB, and then in increments of 256 GB beyond 1 TB. For included storage amounts and max size limits, see [Elastic pool: storage sizes and performance levels](#elastic-pool-storage-sizes-and-performance-levels).
- Extra storage for an elastic pool can be provisioned by increasing its max size using the [Azure portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), the [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), or the [REST API](/rest/api/sql/elasticpools/update).
- The price of extra storage for an elastic pool is the extra storage amount multiplied by the extra storage unit price of the service tier. For details on the price of extra storage, see [SQL Database pricing](https://azure.microsoft.com/pricing/details/sql-database/).

## DTU-based purchasing model: Change elastic pool compute resources (eDTUs)

You can increase or decrease the resources available to an elastic pool based on resource needs using the [Azure portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), the [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), or the [REST API](/rest/api/sql/elasticpools/update).

- When rescaling pool eDTUs, database connections are briefly dropped. This is the same behavior as occurs when rescaling DTUs for a single database (not in a pool). For details on the duration and impact of dropped connections for a database during rescaling operations, see [Rescaling DTUs for a single database](#single-database-change-storage-size). 
- The duration to rescale pool eDTUs can depend on the total amount of storage space used by all databases in the pool. In general, the rescaling latency averages 90 minutes or less per 100 GB. For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for rescaling the pool is 3 hours or less. In some cases within the Standard or Basic tier, the rescaling latency can be under five minutes regardless of the amount of space used.
- In general, the duration to change the min eDTUs per database or max eDTUs per database is five minutes or less.
- When downsizing pool eDTUs, the pool used space must be smaller than the maximum allowed size of the target service tier and pool eDTUs.
- When rescaling pool eDTUs, an extra storage cost applies if (1) the storage max size of the pool is supported by the target pool, and (2) the storage max size exceeds the included storage amount of the target pool. For example, if a 100 eDTU Standard pool with a max size of 100 GB is downsized to a 50 eDTU Standard pool, then an extra storage cost applies since target pool supports a max size of 100 GB and its included storage amount is only 50 GB. So, the extra storage amount is 100 GB – 50 GB = 50 GB. For pricing of extra storage, see [SQL Database pricing](https://azure.microsoft.com/pricing/details/sql-database/). If the actual amount of space used is less than the included storage amount, then this extra cost can be avoided by reducing the database max size to the included amount. 

## Next steps

For overall resource limits, see [SQL Database vCore-based resource limits - elastic pools](sql-database-vcore-resource-limits-elastic-pools.md) and [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-elastic-pools.md).
