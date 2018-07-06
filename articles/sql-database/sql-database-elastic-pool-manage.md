---
title: Create and manage elastic pools - Azure SQL database | Microsoft Docs
description: Create and manage Azure SQL elastic pools.
keywords: multiple databases, database resources, database performance
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 06/20/2018
ms.author: ninarn
ms.topic: conceptual
ms.reviewer: carlrab
---

# Create and manage elastic pools in Azure SQL Database

With an elastic pool, you determine the amount of resources that the elastic pool requires to handle the workload of its databases, and the amount of resources for each pooled database. 

## Azure portal: Manage elastic pools and pooled databases

All pool settings can be found in one place: the **Configure pool** blade. To get here, find an elastic pool in the portal and click **Configure pool** either from the top of the blade or from the resource menu on the left.

From here you can make any combination of the following changes and save them all in one batch:
1. Change the service tier of the pool
2. Scale the performance (DTU or vCores) and storage up or down
3. Add or remove databases to/from the pool
4. Set a min (guaranteed) and max performance limit for the databases in the pools
5. Review the cost summary to view any changes to your bill as a result of your new selections

![Elastic pool configuration blade](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## PowerShell: Manage elastic pools and pooled databases 

To create and manage SQL Database elastic pools and pooled databases with Azure PowerShell, use the following PowerShell cmdlets. If you need to install or upgrade PowerShell, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps). To create and manage databases, servers, and firewall rules, see [Create and manage Azure SQL Database servers and databases using PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> For PowerShell example scripts, see [Create elastic pools and move databases between pools and out of a pool using PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) and [Use PowerShell to monitor and scale a SQL elastic pool in Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Description |
| --- | --- |
|[New-​Azure​Rm​Sql​Elastic​Pool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Creates an elastic database pool on a logical SQL server.|
|[Get-​Azure​Rm​Sql​Elastic​Pool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Gets elastic pools and their property values on a logical SQL server.|
|[Set-​Azure​Rm​Sql​Elastic​Pool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Modifies properties of an elastic database pool on a logical SQL server. For example, use the **StorageMB** property to modify the max storage of an elastic pool.|
|[Remove-​Azure​Rm​Sql​Elastic​Pool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Deletes an elastic database pool on a logical SQL server.|
|[Get-​Azure​Rm​Sql​Elastic​Pool​Activity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Gets the status of operations on an elastic pool on a logical SQL server.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Creates a new database in an existing pool or as a single database. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Gets one or more databases.|
|[Set-​Azure​Rm​Sql​Database](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Sets properties for a database, or moves an existing database into, out of, or between elastic pools.|
|[Remove-​Azure​Rm​Sql​Database](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Removes a database.|


> [!TIP]
> Creation of many databases in an elastic pool can take time when done using the portal or PowerShell cmdlets that create only a single database at a time. To automate creation into an elastic pool, see [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## Azure CLI: Manage elastic pools and pooled databases

To create and manage SQL Database elastic pools with the [Azure CLI](/cli/azure), use the following [Azure CLI SQL Database](/cli/azure/sql/db) commands. Use the [Cloud Shell](/azure/cloud-shell/overview) to run the CLI in your browser, or [install](/cli/azure/install-azure-cli) it on macOS, Linux, or Windows.

> [!TIP]
> For Azure CLI example scripts, see [Use CLI to move an Azure SQL database in a SQL elastic pool](scripts/sql-database-move-database-between-pools-cli.md) and [Use Azure CLI to scale a SQL elastic pool in Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Description |
| --- | --- |
|[az sql elastic-pool create](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Creates an elastic pool.|
|[az sql elastic-pool list](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Returns a list of elastic pools in a server.|
|[az sql elastic-pool list-dbs](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Returns a list of databases in an elastic pool.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Also includes available pool DTU settings, storage limits, and per database settings. In order to reduce verbosity, additional storage limits and per database settings are hidden by default.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Updates an elastic pool.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Deletes the elastic pool.|

## Transact-SQL: Manage pooled databases

To create and move databases within existing elastic pools or to return information about an SQL Database elastic pool with Transact-SQL, use the following T-SQL commands. You can issue these commands using the Azure portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), or any other program that can connect to an Azure SQL Database server and pass Transact-SQL commands. To create and manage databases, servers, and firewall rules, see [Create and manage Azure SQL Database servers and databases using Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> You cannot create, update, or delete an Azure SQL Database elastic pool using Transact-SQL. You can add or remove databases from an elastic pool, and you can use DMVs to return information about existing elastic pools.
>

| Command | Description |
| --- | --- |
|[CREATE DATABASE (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Creates a new database in an existing pool or as a single database. You must be connected to the master database to create a new database.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Move a database into, out of, or between elastic pools.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Deletes a database.|
|[sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Returns resource usage statistics for all the elastic database pools in a logical server. For each elastic database pool, there is one row for each 15 second reporting window (four rows per minute). This includes CPU, IO, Log, storage consumption and concurrent request/session utilization by all databases in the pool.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Returns the edition (service tier), service objective (pricing tier), and elastic pool name, if any, for an Azure SQL database or an Azure SQL Data Warehouse. If logged on to the master database in an Azure SQL Database server, returns information on all databases. For Azure SQL Data Warehouse, you must be connected to the master database.|

## REST API: Manage elastic pools and pooled databases

To create and manage SQL Database elastic pools and pooled databases, use these REST API requests.

| Command | Description |
| --- | --- |
|[Elastic Pools - Create Or Update](/rest/api/sql/elasticpools/createorupdate)|Creates a new elastic pool or updates an existing elastic pool.|
|[Elastic Pools - Delete](/rest/api/sql/elasticpools/delete)|Deletes the elastic pool.|
|[Elastic Pools - Get](/rest/api/sql/elasticpools/get)|Gets an elastic pool.|
|[Elastic Pools - List By Server](/rest/api/sql/elasticpools/listbyserver)|Returns a list of elastic pools in a server.|
|[Elastic Pools - Update](/rest/api/sql/elasticpools/update)|Updates an existing elastic pool.|
|[Recommended Elastic Pools - Get](/rest/api/sql/recommendedelasticpools/get)|Gets a recommended elastic pool.|
|[Recommended Elastic Pools - List By Server](/rest/api/sql/recommendedelasticpools/listbyserver)|Returns recommended elastic pools.|
|[Recommended Elastic Pools - List Metrics](/rest/api/sql/recommendedelasticpools/listmetrics)|Returns recommended elastic pool metrics.|
|[Elastic Pool Activities](/rest/api/sql/elasticpoolactivities)|Returns elastic pool activities.|
|[Elastic Pool Database Activities](/rest/api/sql/elasticpooldatabaseactivities)|Returns activity on databases inside of an elastic pool.|
|[Databases - Create Or Update](/rest/api/sql/databases/createorupdate)|Creates a new database or updates an existing database.|
|[Databases - Get](/rest/api/sql/databases/get)|Gets a database.|
|[Databases - Get By Elastic Pool](/rest/api/sql/databases/getbyelasticpool)|Gets a database inside of an elastic pool.|
|[Databases - Get By Recommended Elastic Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Gets a database inside of a recommended elastic pool.|
|[Databases - List By Elastic Pool](/rest/api/sql/databases/listbyelasticpool)|Returns a list of databases in an elastic pool.|
|[Databases - List By Recommended Elastic Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Returns a list of databases inside a recommended elastic pool.|
|[Databases - List By Server](/rest/api/sql/databases/listbyserver)|Returns a list of databases in a server.|
|[Databases - Update](/rest/api/sql/databases/update)|Updates an existing database.|

## Next steps

* For a video, see [Microsoft Virtual Academy video course on Azure SQL Database elastic capabilities](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* To learn more about design patterns for SaaS applications using elastic pools, see [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* For a SaaS tutorial using elastic pools, see [Introduction to the Wingtip SaaS application](sql-database-wtp-overview.md).
