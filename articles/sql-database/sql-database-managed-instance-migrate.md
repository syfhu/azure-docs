---
title: Migrate SQL Server instance to Azure SQL Database Managed Instance | Microsoft Docs
description: Learn how to migrate a SQL Server instance to Azure SQL Database Managed Instance. 
keywords: database migration,sql server database migration,database migration tools,migrate database,migrate sql database
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: bonova

---
# SQL Server instance migration to Azure SQL Database Managed Instance

In this article, you learn about the methods for migrating a SQL Server 2005 or later version instance to Azure SQL Database Managed Instance (preview). 

SQL Database Managed Instance is an expansion of the existing SQL Database service, providing a third deployment option alongside single databases and elastic pools.  It is designed to enable database lift-and-shift to a fully managed PaaS, without redesigning the application. SQL Database Managed Instance provides high compatibility with the on-premises SQL Server programming model and out-of-box support for the large majority of SQL Server features and accompanying tools and services.

At the high level, application migration process looks like on the following diagram:

![migration process](./media/sql-database-managed-instance-migration/migration-process.png)

- [Assess Managed Instance compatibility](sql-database-managed-instance-migrate.md#assess-managed-instance-compatibility)
- [Choose app connectivity option](sql-database-managed-instance-migrate.md#choose-app-connectivity-option)
- [Deploy to an optimally sized Managed Instance](sql-database-managed-instance-migrate.md#deploy-to-an-optimally-sized-managed-instance)
- [Select migration method and migrate](sql-database-managed-instance-migrate.md#select-migration-method-and-migrate)
- [Monitor applications](sql-database-managed-instance-migrate.md#monitor-applications)

> [!NOTE]
> To migrate a single database into either a single database or elastic pool, see [Migrate a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md).

## Assess Managed Instance compatibility

First, determine whether Managed Instance is compatible with the database requirements of your application. Managed Instance is designed to provide easy lift and shift migration for the majority of existing applications that use SQL Server on-premises or on virtual machines. However, you may sometimes require features or capabilities that are not yet supported and the cost of implementing a workaround are too high. 

Use [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) to detect potential compatibility issues impacting database functionality on Azure SQL Database. DMA does not yet support Managed Instance as migration destination, but it is recommended to run assessment against Azure SQL Database and carefully review list of reported feature parity and compatibility issues against product documentation. Most of the blocking issues preventing a migration to Azure SQL Database have been removed with Managed Instance. For instance, features like cross-database queries, cross-database transactions within the same instance, linked server to other SQL sources, CLR, global temp tables, instance level views, Service Broker and the like are available in Managed Instances. 

However, there are some cases when you need to consider an alternative option, such as [SQL Server on Virtual Machines in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Here are some examples:

- If you require direct access to the operating system or file system, for instance to install third party or custom agents on the same virtual machine with SQL Server.
- If you have strict dependency on features that are still not supported, such as FileStream / FileTable, PolyBase, and cross-instance transactions.
- If absolutely you need to stay at a specific version of SQL Server (2012, for instance).
- If your compute requirements are much lower that Managed Instance offers in public preview (one vCore, for instance) and database consolidation is not acceptable option.

## Deploy to an optimally sized Managed Instance

Managed Instance is tailored for on-premises workloads that are planning to move to the cloud. It introduces a new purchasing model that provides greater flexibility in selecting the right level of resources for your workloads. In the on-premises world, you are probably accustomed to sizing these workloads by using physical cores. The new purchasing model for Managed Instance is based upon virtual cores, or “vCores,” with additional storage and IO available separately. The vCore model is a simpler way to understand your compute requirements in the cloud versus what you use on-premises today. This new model enables you to right-size your destination environment in the cloud.

You can select compute and storage resources at deployment time and then change it afterwards without introducing downtime for your application.

![managed instance sizing](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

To learn how to create the VNet infrastructure and a Managed Instance, see [Create a Managed Instance](sql-database-managed-instance-create-tutorial-portal.md).

> [!IMPORTANT]
> It is important to keep your destination VNet and subnet always in accordance with [Managed Instance VNET requirements](sql-database-managed-instance-vnet-configuration.md#requirements). Any incompatibility can prevent you from creating new instances or using those that you already created.

## Select migration method and migrate

Managed Instance targets user scenarios requiring mass database migration from on-premises or IaaS database implementations. They are optimal choice when you need to lift and shift the back end of the applications that regularly use instance level and / or cross-database functionalities. If this is your scenario, you can move an entire instance to a corresponding environment in Azure without the need to rearchitecture your applications. 

To move SQL instances, you need to plan carefully:

-	The migration of all databases that need to be collocated (ones running on the same instance)
-	The migration of instance-level objects that your application depends on, including logins, credentials, SQL Agent Jobs and Operators, and server level triggers. 

Managed Instance is a fully managed service that allows you to delegate some of the regular DBA activities to the platform as they are built in. Therefore, some instance level data does not need to be migrated, such as maintenance jobs for regular backups or Always On configuration, as [high availability](sql-database-high-availability.md) is built in.

Managed Instance supports the following database migration options (currently these are the only supported migration methods):

- Azure Database Migration Service - migration with near-zero downtime
- Native RESTORE from URL - uses native backups from SQL Server and requires some downtime

### Azure Database Migration Service

The [Azure Database Migration Service (DMS)](../dms/dms-overview.md) is a fully managed service designed to enable seamless migrations from multiple database sources to Azure Data platforms with minimal downtime. This service streamlines the tasks required to move existing third party and SQL Server databases to Azure. Deployment options at Public Preview include Azure SQL Database, Managed Instance, and SQL Server in an Azure Virtual Machine. DMS is the recommended method of migration for your enterprise workloads. 

If you use SQL Server Integration Services (SSIS) on your SQL Server on premises, DMS does not yet support migrating SSIS catalog (SSISDB) that stores SSIS packages, but you can provision Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) that will create a new SSISDB in Azure SQL Database/Managed Instance and then you can redeploy your packages to it, see [Create Azure-SSIS IR in ADF](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

To learn more about this scenario and configuration steps for DMS, see [Migrate your on-premises database to Managed Instance using DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### Native RESTORE from URL

RESTORE of native backups (.bak files) taken from SQL Server on-premises or [SQL Server on Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/), available on [Azure Storage](https://azure.microsoft.com/services/storage/), is one of key capabilities on SQL DB Managed Instance that enables quick and easy offline database migration. 

The following diagram explains the process at the high level:

![migration-flow](./media/sql-database-managed-instance-migration/migration-flow.png)

The following table provides more information regarding the method you can use depending on source SQL Server version you are running:

|Step|SQL Engine and version|Backup / Restore method|
|---|---|---|
|Put backup to Azure Storage|Prior SQL 2012 SP1 CU2|Upload .bak file directly to Azure storage|
||2012 SP1 CU2 - 2016|Direct backup using deprecated [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) syntax|
||2016 and above|Direct backup using [WITH SAS CREDENTIAL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Restore from Azure Storage	to Managed Instance|[RESTORE FROM URL with SAS CREDENTIAL](sql-database-managed-instance-restore-from-backup-tutorial.md)|

> [!IMPORTANT]
> Restore of system databases is not supported. To migrate instance level objects (stored in master or msdb databases), we recommend to script them out and run T-SQL scripts on the destination instance.

For a full tutorial that includes restoring a database backup to a Managed Instance using a SAS credential, see [Restore from backup to a Managed Instance](sql-database-managed-instance-restore-from-backup-tutorial.md).

## Monitor applications

Track application behavior and performance after migration. In Managed Instance, some changes are only enabled once the [database compatibility level has been changed](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Database migration to Azure SQL Database keeps its original compatibility level in majority of cases. If the compatibility level of a user database was 100 or higher before the migration, it remains the same after migration. If the compatibility level of a user database was 90 before migration, in the upgraded database, the compatibility level is set to 100, which is the lowest supported compatibility level in Managed Instance. Compatibility level of system databases is 140.

To reduce migration risks, change the database compatibility level only after performance monitoring. Use Query Store as optimal tool for getting information about workload performance before and after database compatibility level change, as explained in [Keep performance stability during the upgrade to newer SQL Server version](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Once you are on a fully managed platform, take advantages that are provided automatically as part of the SQL Database service. For instance, you don’t have to create backups on Managed Instance -  the service performs backups for you automatically. You no longer must worry about scheduling, taking, and managing backups. Managed Instance provides you the ability to restore to any point in time within this retention period using [Point in Time Recovery (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). During public preview, the retention period is fixed to seven days.
Additionally, you do not need to worry about setting up high availability as [high availability](sql-database-high-availability.md) is built in.

To strengthen security, consider using some of the features that are available:
- Azure Active Directory Authentication at the database level
- Auditing and Threat Detection to monitor activities
- Controlling Access to sensitive and privileged data ([Row-Level Security](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) and [Dynamic Data Masking](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)).

## Next steps

- For information about Managed Instances, see [What is a Managed Instance?](sql-database-managed-instance.md).
- For a tutorial that includes a restore from backup, see [Create a Managed Instance](sql-database-managed-instance-create-tutorial-portal.md).
- For tutorial showing migration using DMS, see [Migrate your on-premises database to Managed Instance using DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
