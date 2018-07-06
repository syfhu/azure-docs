---
title: Azure SQL logical servers and single databases | Microsoft Docs
description: Learn about Azure SQL Database logical server and single database concepts, and their resources.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab

---

# Azure SQL Database logical servers and single databases, and their resources

## What is an Azure SQL logical server?

A logical server acts as a central administrative point for multiple single or [pooled](sql-database-elastic-pool.md) databases, [logins](sql-database-manage-logins.md), [firewall rules](sql-database-firewall-configure.md), [auditing rules](sql-database-auditing.md), [threat detection policies](sql-database-threat-detection.md), and [failover groups](sql-database-geo-replication-overview.md). A logical server can be in a different region than its resource group. The logical server must exist before you can create the Azure SQL database. All databases on a server are created within the same region as the logical server.

A logical server is a logical construct that is distinct from a SQL Server instance that you may be familiar with in the on-premises world. Specifically, the SQL Database service makes no guarantees regarding location of the databases in relation to their logical servers, and exposes no instance-level access or features. In contrast, a server in a SQL Database Managed Instance is similar to a SQL Server instance that you may be familiar with in the on-premises world.

When you create a logical server, you provide a server login account and password that has administrative rights to the master database on that server and all databases created on that server. This initial account is a SQL login account. Azure SQL Database supports SQL authentication and Azure Active Directory Authentication for authentication. For information about logins and authentication, see [Managing Databases and Logins in Azure SQL Database](sql-database-manage-logins.md). Windows Authentication is not supported. 

> [!TIP]
> For valid resource group and server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

An Azure Database logical server:

- Is created within an Azure subscription, but can be moved with its contained resources to another subscription
- Is the parent resource for databases, elastic pools, and data warehouses
- Provides a namespace for databases, elastic pools, and data warehouses
- Is a logical container with strong lifetime semantics - delete a server and it deletes the contained databases, elastic pools, and data warehouses
- Participates in [Azure role-based access control (RBAC)](/azure/role-based-access-control/overview) - databases, elastic pools, and data warehouses within a server inherit access rights from the server
- Is a high-order element of the identity of databases, elastic pools, and data warehouses for Azure resource management purposes (see the URL scheme for databases and pools)
- Collocates resources in a region
- Provides a connection endpoint for database access (<serverName>.database.windows.net)
- Provides access to metadata regarding contained resources via DMVs by connecting to a master database 
- Provides the scope for management policies that apply to its databases - logins, firewall, audit, threat detection, etc. 
- Is restricted by a quota within the parent subscription (six servers per subscription by default - [see Subscription limits here](../azure-subscription-service-limits.md))
- Provides the scope for database quota and DTU or vCore quota for the resources it contains (such as 45,000 DTU)
- Is the versioning scope for capabilities enabled on contained resources 
- Server-level principal logins can manage all databases on a server
- Can contain logins similar to those in instances of SQL Server on your premises that are granted access to one or more databases on the server, and can be granted limited administrative rights. For more information, see [Logins](sql-database-manage-logins.md).
- The default collation for all user databases created on a logical server is `SQL_LATIN1_GENERAL_CP1_CI_AS`, where `LATIN1_GENERAL` is English (United States), `CP1` is code page 1252, `CI` is case-insensitive, and `AS` is accent-sensitive.

## Logical servers and databases

On a logical server, you can create:

- A single database created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) with a [combined set of compute and storage resources](sql-database-service-tiers-dtu.md) or an [independent scale of compute and storage resources](sql-database-service-tiers-vcore.md). An Azure SQL database is associated with an Azure SQL Database logical server, which is created within a specific Azure region.
- A database created as part of a [pool of databases](sql-database-elastic-pool.md) within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) with a [combined set of compute and storage resources (DTU-based)](sql-database-service-tiers-dtu.md) or an [independent scale of compute and storage resources (vCore-based)](sql-database-service-tiers-vcore.md) that are shared among all of the databases in the pool. An Azure SQL database is associated with an Azure SQL Database logical server, which is created within a specific Azure region.

> [!IMPORTANT]
> SQL Database Managed Instance, currently in public preview, is an [instance of a SQL server](sql-database-managed-instance.md) (a Managed Instance) created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) with a defined set of compute and storage resources for all databases on that server instance. A managed instance contains both system and user databases. Managed Instance is designed to enable database lift-and-shift to a fully managed PaaS, without redesigning the application. Managed Instance provides high compatibility with the on-premises SQL Server programming model and supports the large majority of SQL Server features and accompanying tools and services. For more information, see [SQL Database Managed Instance](sql-database-managed-instance.md). The remainder of this article does not apply to Managed Instance.

## TDS and TCP/IP connections

Microsoft Azure SQL Database supports tabular data stream (TDS) protocol client version 7.3 or later and allows only encrypted TCP/IP connections.

## Azure SQL databases protected by SQL Database firewall

To help protect your data, a [SQL Database firewall](sql-database-firewall-configure.md) prevents all access to your database server or any of its databases from outside of your connection to the server directly through your Azure subscription connection. To enable additional connectivity, you must [create one or more firewall rules](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). For creating and managing elastic pools, see [Elastic pools](sql-database-elastic-pool.md).

## Manage Azure SQL servers, databases, and firewalls using the Azure portal

You can create the Azure SQL database's resource group ahead of time or while creating the server itself. There are multiple methods for getting to a new SQL server form, either by creating a new SQL server or as part of creating a new database. 

### Create a blank SQL server (logical server)

To create an Azure SQL Database server (without a database) using the [Azure portal](https://portal.azure.com), navigate to a blank SQL server (logical server) form.  

### Create a blank or sample SQL database

To create an Azure SQL database using the [Azure portal](https://portal.azure.com), navigate to a blank SQL Database form and provide the requested information. You can create the Azure SQL database's resource group and logical server ahead of time or while creating the database itself. You can create a blank database or create a sample database based on Adventure Works LT. 

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> For information on selecting the pricing tier for your database, see [DTU-based purchasing model](sql-database-service-tiers-dtu.md) and [vCore-based purchasing model (preview)](sql-database-service-tiers-vcore.md).

To create a Managed Instance, see [Create a Managed Instance](sql-database-managed-instance-create-tutorial-portal.md)

### Manage an existing SQL server

To manage an existing server, navigate to the server using a number of methods - such as from specific SQL database page, the **SQL servers** page, or the **All resources** page. 

To manage an existing database, navigate to the **SQL databases** page and click the database you wish to manage. The following screenshot shows how to begin setting a server-level firewall for a database from the **Overview** page for a database. 

   ![server firewall rule](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> To configure performance properties for a database, see [DTU-based purchasing model](sql-database-service-tiers-dtu.md) and [vCore-based purchasing model (preview)](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> For an Azure portal quickstart, see [Create an Azure SQL database in the Azure portal](sql-database-get-started-portal.md).
>

## Manage Azure SQL servers, databases, and firewalls using PowerShell

To create and manage Azure SQL server, databases, and firewalls with Azure PowerShell, use the following PowerShell cmdlets. If you need to install or upgrade PowerShell, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps). For creating and managing elastic pools, see [Elastic pools](sql-database-elastic-pool.md).

| Cmdlet | Description |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Creates a database |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Gets one or more databases|
|[Set-​Azure​Rm​Sql​Database](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Sets properties for a database, or moves an existing database into an elastic pool|
|[Remove-​Azure​Rm​Sql​Database](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Removes a database|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Creates a resource group|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Creates a  server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Returns information about servers|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Modifies properties of a server|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Removes a server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Creates a server-level firewall rule |
|[Get-​Azure​Rm​Sql​Server​Firewall​Rule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Gets firewall rules for a server|
|[Set-​Azure​Rm​Sql​Server​Firewall​Rule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Modifies a firewall rule in a server|
|[Remove-​Azure​Rm​Sql​Server​Firewall​Rule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Deletes a firewall rule from a server.|
| New-AzureRmSqlServerVirtualNetworkRule | Creates a [*virtual network rule*](sql-database-vnet-service-endpoint-rule-overview.md), based on a subnet that is a Virtual Network service endpoint. |

> [!TIP]
> For a PowerShell quickstart, see [Create a single Azure SQL database using PowerShell](sql-database-get-started-portal.md). For PowerShell example scripts, see [Use PowerShell to create a single Azure SQL database and configure a firewall rule](scripts/sql-database-create-and-configure-database-powershell.md) and [Monitor and scale a single SQL database using PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## Manage Azure SQL servers, databases, and firewalls using the Azure CLI

To create and manage Azure SQL server, databases, and firewalls with the [Azure CLI](/cli/azure), use the following [Azure CLI SQL Database](/cli/azure/sql/db) commands. Use the [Cloud Shell](/azure/cloud-shell/overview) to run the CLI in your browser, or [install](/cli/azure/install-azure-cli) it on macOS, Linux, or Windows. For creating and managing elastic pools, see [Elastic pools](sql-database-elastic-pool.md).

| Cmdlet | Description |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Creates a database|
|[az sql db list](/cli/azure/sql/db#az_sql_db_list)|Lists all databases and data warehouses in a server, or all databases in an elastic pool|
|[az sql db list-editions](/cli/azure/sql/db#az_sql_db_list_editions)|Lists available service objectives and storage limits|
|[az sql db list-usages](/cli/azure/sql/db#az_sql_db_list_usages)|Returns database usages|
|[az sql db show](/cli/azure/sql/db#az_sql_db_show)|Gets a database or data warehouse|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Updates a database|
|[az sql db delete](/cli/azure/sql/db#az_sql_db_delete)|Removes a database|
|[az group create](/cli/azure/group#az_group_create)|Creates a resource group|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Creates a server|
|[az sql server list](/cli/azure/sql/server#az_sql_server_list)|Lists servers|
|[az sql server list-usages](/cli/azure/sql/server#az_sql_server_list_usages)|Returns  server usages|
|[az sql server show](/cli/azure/sql/server#az_sql_server_show)|Gets a server|
|[az sql server update](/cli/azure/sql/server#az_sql_server_update)|Updates a server|
|[az sql server delete](/cli/azure/sql/server#az_sql_server_delete)|Deletes a server|
|[az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Creates a server firewall rule|
|[az sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Lists the firewall rules on a server|
|[az sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Shows the detail of a firewall rule|
|[az sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Updates a firewall rule|
|[az sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Deletes a firewall rule|

> [!TIP]
> For an Azure CLI quickstart, see [Create a single Azure SQL database using the Azure CLI](sql-database-get-started-cli.md). For Azure CLI example scripts, see [Use CLI to create a single Azure SQL database and configure a firewall rule](scripts/sql-database-create-and-configure-database-cli.md) and [Use CLI to monitor and scale a single SQL database](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## Manage Azure SQL servers, databases, and firewalls using Transact-SQL

To create and manage Azure SQL server, databases, and firewalls with Transact-SQL, use the following T-SQL commands. You can issue these commands using the Azure portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), or any other program that can connect to an Azure SQL Database server and pass Transact-SQL commands. For managing elastic pools, see [Elastic pools](sql-database-elastic-pool.md).

> [!IMPORTANT]
> You cannot create or delete a server using Transact-SQL.
>

| Command | Description |
| --- | --- |
|[CREATE DATABASE (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Creates a new database. You must be connected to the master database to create a new database.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Modifies an Azure SQL database. |
|[ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Modifies an Azure SQL Data Warehouse.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Deletes a database.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Returns the edition (service tier), service objective (pricing tier), and elastic pool name, if any, for an Azure SQL database or an Azure SQL Data Warehouse. If logged on to the master database in an Azure SQL Database server, returns information on all databases. For Azure SQL Data Warehouse, you must be connected to the master database.|
|[sys.dm_db_resource_stats (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Returns CPU, IO, and memory consumption for an Azure SQL Database database. One row exists for every 15 seconds, even if there is no activity in the database.|
|[sys.resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Returns CPU usage and storage data for an Azure SQL Database. The data is collected and aggregated within five-minute intervals.|
|[sys.database_connection_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Contains statistics for SQL Database database connectivity events, providing an overview of database connection successes and failures. |
|[sys.event_log (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Returns successful Azure SQL Database database connections, connection failures, and deadlocks. You can use this information to track or troubleshoot your database activity with SQL Database.|
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Creates or updates the server-level firewall settings for your SQL Database server. This stored procedure is only available in the master database to the server-level principal login. A server-level firewall rule can only be created using Transact-SQL after the first server-level firewall rule has been created by a user with Azure-level permissions|
|[sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Returns information about the server-level firewall settings associated with your Microsoft Azure SQL Database.|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Removes server-level firewall settings from your SQL Database server. This stored procedure is only available in the master database to the server-level principal login.|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Creates or updates the database-level firewall rules for your Azure SQL Database or SQL Data Warehouse. Database firewall rules can be configured for the master database, and for user databases on SQL Database. Database firewall rules are useful when using contained database users. |
|[sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Returns information about the database-level firewall settings associated with your Microsoft Azure SQL Database. |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Removes database-level firewall setting from your Azure SQL Database or SQL Data Warehouse. |


> [!TIP]
> For a quickstart using SQL Server Management Studio on Microsoft Windows, see [Azure SQL Database: Use SQL Server Management Studio to connect and query data](sql-database-connect-query-ssms.md). For a quickstart using Visual Studio Code on the macOS, Linux, or Windows, see [Azure SQL Database: Use Visual Studio Code to connect and query data](sql-database-connect-query-vscode.md).

## Manage Azure SQL servers, databases, and firewalls using the REST API

To create and manage Azure SQL server, databases, and firewalls, use these REST API requests.

| Command | Description |
| --- | --- |
|[Servers - Create Or Update](/rest/api/sql/servers/createorupdate)|Creates or updates a new server.|
|[Servers - Delete](/rest/api/sql/servers/delete)|Deletes a SQL server.|
|[Servers - Get](/rest/api/sql/servers/get)|Gets a server.|
|[Servers - List](/rest/api/sql/servers/list)|Returns a list of servers.|
|[Servers - List By Resource Group](/rest/api/sql/servers/listbyresourcegroup)|Returns a list of servers in a resource group.|
|[Servers - Update](/rest/api/sql/servers/update)|Updates an existing server.|
|[Databases - Create Or Update](/rest/api/sql/databases/createorupdate)|Creates a new database or updates an existing database.|
|[Databases - Get](/rest/api/sql/databases/get)|Gets a database.|
|[Databases - Get By Elastic Pool](/rest/api/sql/databases/getbyelasticpool)|Gets a database inside of an elastic pool.|
|[Databases - Get By Recommended Elastic Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Gets a database inside of a recommented elastic pool.|
|[Databases - List By Elastic Pool](/rest/api/sql/databases/listbyelasticpool)|Returns a list of databases in an elastic pool.|
|[Databases - List By Recommended Elastic Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Returns a list of databases inside a recommended elastic pool.|
|[Databases - List By Server](/rest/api/sql/databases/listbyserver)|Returns a list of databases in a server.|
|[Databases - Update](/rest/api/sql/databases/update)|Updates an existing database.|
|[Firewall Rules - Create Or Update](/rest/api/sql/firewallrules/createorupdate)|Creates or updates a firewall rule.|
|[Firewall Rules - Delete](/rest/api/sql/firewallrules/delete)|Deletes a firewall rule.|
|[Firewall Rules - Get](/rest/api/sql/firewallrules/get)|Gets a firewall rule.|
|[Firewall Rules - List By Server](/rest/api/sql/firewallrules/listbyserver)|Returns a list of firewall rules.|

## Next steps

- To learn about migrating a SQL Server database to Azure, see [Migrate to Azure SQL Database](sql-database-cloud-migrate.md).
- For information about supported features, see [Features](sql-database-features.md).
