---
title: Cloud business continuity - database recovery - SQL Database | Microsoft Docs
description: Learn how Azure SQL Database supports cloud business continuity and database recovery and helps keep mission-critical cloud applications running.
keywords: business continuity,cloud business continuity,database disaster recovery,database recovery
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: "On Demand"
ms.date: 06/27/2018
ms.author: sashan
ms.reviewer: carlrab

---
# Overview of business continuity with Azure SQL Database

This overview describes the capabilities that Azure SQL Database provides for business continuity and disaster recovery. Learn about options, recommendations, and tutorials for recovering from disruptive events that could cause data loss or cause your database and application to become unavailable. Learn what to do when a user or application error affects data integrity, an Azure region has an outage, or your application requires maintenance.

## SQL Database features that you can use to provide business continuity

SQL Database provides several business continuity features, including automated backups and optional database replication. Each has different characteristics for estimated recovery time (ERT) and potential data loss for recent transactions. Once you understand these options, you can choose among them - and, in most scenarios, use them together for different scenarios. As you develop your business continuity plan, you need to understand the maximum acceptable time before the application fully recovers after the disruptive event - this is your recovery time objective (RTO). You also need to understand the maximum amount of recent data updates (time interval) the application can tolerate losing when recovering after the disruptive event - this is your recovery point objective (RPO).

The following table compares the ERT and RPO for each service tier for the three most common scenarios.

| Capability | Basic | Standard | Premium  | General Purpose | Business Critical
| --- | --- | --- | --- |--- |--- |
| Point in Time Restore from backup |Any restore point within 7 days |Any restore point within 35 days |Any restore point within 35 days |Any restore point within configured period (up to 35 days)|Any restore point within configured period (up to 35 days)|
| Geo-restore from geo-replicated backups |ERT < 12h, RPO < 1h |ERT < 12h, RPO < 1h |ERT < 12h, RPO < 1h |ERT < 12h, RPO < 1h|ERT < 12h, RPO < 1h|
| Restore from Azure Backup Vault |ERT < 12h, RPO < 1 wk |ERT < 12h, RPO < 1 wk |ERT < 12h, RPO < 1 wk |ERT < 12h, RPO < 1 wk|ERT < 12h, RPO < 1 wk|
| Active geo-replication |ERT < 30s, RPO < 5s |ERT < 30s, RPO < 5s |ERT < 30s, RPO < 5s |ERT < 30s, RPO < 5s|ERT < 30s, RPO < 5s|

### Use point-in-time restore to recover a database

SQL Database automatically performs a combination of full database backups weekly, differential database backups hourly, and transaction log backups every five - ten minutes to protect your business from data loss. If you're using the [DTU-based purchasing model](sql-database-service-tiers-dtu.md), then these backups are stored in RA-GRS storage for 35 days for databases in the Standard and Premium service tiers and 7 days for databases in the Basic service tier. If the retention period for your service tier does not meet your business requirements, you can increase the retention period by [changing the service tier](sql-database-single-database-scale.md). If you're using the [vCore-based purchasing model (preview)](sql-database-service-tiers-vcore.md), the backups retention is configurable up to 35 days in the General purpose and Business critical tiers. The full and differential database backups are also replicated to a [paired data center](../best-practices-availability-paired-regions.md) for protection against a data center outage. For more information, see [automatic database backups](sql-database-automated-backups.md).

If the maximum supported point-in-time restore (PITR) retention period is not sufficient for your application, you can extend it by configuring a long-term retention (LTR) policy for the database(s). For more information, see [Automated Backups](sql-database-automated-backups.md) and [Long-term backup retention](sql-database-long-term-retention.md).

You can use these automatic database backups to recover a database from various disruptive events, both within your data center and to another data center. Using automatic database backups, the estimated time of recovery depends on several factors including the total number of databases recovering in the same region at the same time, the database size, the transaction log size, and network bandwidth. The recovery time is usually less than 12 hours. It may take longer to recover a very large or active database. For more details about recovery time, see [database recovery time](sql-database-recovery-using-backups.md#recovery-time). When recovering to another data region, the potential data loss is limited to 1 hour by the geo-redundant storage of hourly differential database backups.

> [!IMPORTANT]
> To recover using automated backups, you must be a member of the SQL Server Contributor role or the subscription owner - see [RBAC: Built-in roles](../role-based-access-control/built-in-roles.md). You can recover using the Azure portal, PowerShell, or the REST API. You cannot use Transact-SQL.
>

Use automated backups as your business continuity and recovery mechanism if your application:

* Is not considered mission critical.
* Doesn't have a binding SLA - a downtime of 24 hours or longer does not result in financial liability.
* Has a low rate of data change (low transactions per hour) and losing up to an hour of change is an acceptable data loss.
* Is cost sensitive.

If you need faster recovery, use [active geo-replication](sql-database-geo-replication-overview.md) (discussed next). If you need to be able to recover data from a period older than 35 days, use [Long-term retention](sql-database-long-term-retention.md). 

### Use active geo-replication and auto-failover groups (in-preview) to reduce recovery time and limit data loss associated with a recovery

In addition to using database backups for database recovery if a business disruption occurs, you can use [active geo-replication](sql-database-geo-replication-overview.md) to configure a database to have up to four readable secondary databases in the regions of your choice. These secondary databases are kept synchronized with the primary database using an asynchronous replication mechanism. This feature is used to protect against business disruption if a data center outage occurs or during an application upgrade. Active geo-replication can also be used to provide better query performance for read-only queries to geographically dispersed users.

To enable automated and transparent failover you should organize your geo-replicated databases into groups using the [auto-failover group](sql-database-geo-replication-overview.md) feature of SQL Database (in-preview).

If the primary database goes offline unexpectedly or you need to take it offline for maintenance activities, you can quickly promote a secondary to become the primary (also called a failover) and configure applications to connect to the promoted primary. If your application is connecting to the databases using the failover group listener, you don’t need to change the SQL connection string configuration after failover. With a planned failover, there is no data loss. With an unplanned failover, there may be some small amount of data loss for very recent transactions due to the nature of asynchronous replication. Using auto-failover groups (in-preview), you can customize the failover policy to minimize the potential data loss. After a failover, you can later failback - either according to a plan or when the data center comes back online. In all cases, users experience a small amount of downtime and need to reconnect.

> [!IMPORTANT]
> To use active geo-replication and auto-failover groups (in-preview), you must either be the subscription owner or have administrative permissions in SQL Server. You can configure and fail over using the Azure portal, PowerShell, or the REST API using Azure subscription permissions or using Transact-SQL with SQL Server permissions.
> 

Use active geo-replication and auto-failover groups (in preview) if your application meets any of these criteria:

* Is mission critical.
* Has a service level agreement (SLA) that does not allow for 24 hours or more of downtime.
* Downtime may result in financial liability.
* Has a high rate of data change is high and losing an hour of data is not acceptable.
* The additional cost of active geo-replication is lower than the potential financial liability and associated loss of business.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## Recover a database after a user or application error

No one is perfect! A user might accidentally delete some data, inadvertently drop an important table, or even drop an entire database. Or, an application might accidentally overwrite good data with bad data due to an application defect.

In this scenario, these are your recovery options.

### Perform a point-in-time restore
You can use the automated backups to recover a copy of your database to a known good point in time, provided that time is within the database retention period. After the database is restored, you can either replace the original database with the restored database or copy the needed data from the restored data into the original database. If the database uses active geo-replication, we recommend copying the required data from the restored copy into the original database. If you replace the original database with the restored database, you need to reconfigure and resynchronize active geo-replication (which can take quite some time for a large database). While this restores a database to the last available point in time, restoring the geo-secondary to any point in time is not currently supported.

For more information and for detailed steps for restoring a database to a point in time using the Azure portal or using PowerShell, see [point-in-time restore](sql-database-recovery-using-backups.md#point-in-time-restore). You cannot recover using Transact-SQL.

### Restore a deleted database
If the database is deleted but the logical server has not been deleted, you can restore the deleted database to the point at which it was deleted. This restores a database backup to the same logical SQL server from which it was deleted. You can restore it using the original name or provide a new name or the restored database.

For more information and for detailed steps for restoring a deleted database using the Azure portal or using PowerShell, see [restore a deleted database](sql-database-recovery-using-backups.md#deleted-database-restore). You cannot restore using Transact-SQL.

> [!IMPORTANT]
> If the logical server is deleted, you cannot recover a deleted database.


### Restore backups from long-term retention

If the data loss occurred outside the current retention period for automated backups and your database is configured for long-term retention, you can restore from a full backup in the LTR storage to a new database. At this point, you can either replace the original database with the restored database or copy the needed data from the restored database into the original database. If you need to retrieve an old version of your database prior to a major application upgrade, satisfy a request from auditors or a legal order, you can create a database using a full backup saved in the Azure Backup Vault.  For more information, see [Long-term retention](sql-database-long-term-retention.md).

## Recover a database to another region from an Azure regional data center outage
<!-- Explain this scenario -->

Although rare, an Azure data center can have an outage. When an outage occurs, it causes a business disruption that might only last a few minutes or might last for hours.

* One option is to wait for your database to come back online when the data center outage is over. This works for applications that can afford to have the database offline. For example, a development project or free trial you don't need to work on constantly. When a data center has an outage, you do not know how long the outage might last, so this option only works if you don't need your database for a while.
* Another option is to either fail over to another data region if you are using active geo-replication or the recover a database using geo-redundant database backups (geo-restore). Failover takes only a few seconds while database recovery from backups takes hours.

When you take action, how long it takes you to recover, and how much data loss you incur depends upon how you decide to use these business continuity features in your application. Indeed, you may choose to use a combination of database backups and active geo-replication depending upon your application requirements. For a discussion of application design considerations for stand-alone databases and for elastic pools using these business continuity features, see [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) and [Elastic Pool disaster recovery strategies](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

The following sections provide an overview of the steps to recover using either database backups or active geo-replication. For detailed steps including planning requirements, post recovery steps, and information about how to simulate an outage to perform a disaster recovery drill, see [Recover a SQL Database from an outage](sql-database-disaster-recovery.md).

### Prepare for an outage
Regardless of the business continuity feature you use, you must:

* Identify and prepare the target server, including server-level firewall rules, logins, and master database level permissions.
* Determine how to redirect clients and client applications to the new server
* Document other dependencies, such as auditing settings and alerts

If you do not prepare properly, bringing your applications online after a failover or a database recovery takes additional time and likely also require troubleshooting at a time of stress - a bad combination.

### Fail over to a geo-replicated secondary database
If you are using active geo-replication and auto-failover groups (in-preview) as your recovery mechanism, you can configure an automatic failover policy or use [manual failover](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Once initiated, the failover causes the secondary to become the new primary and ready to record new transactions and respond to queries - with minimal data loss for the data not yet replicated. For information on designing the failover process, see [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> When the data center comes back online the old primaries automatically reconnect to the new primary and become secondary databases. If you need to relocate the primary back to the original region, you can initiate a planned failover manually (failback). 
> 

### Perform a geo-restore
If you are using automated backups with geo-redundant storage replication as your recovery mechanism, [initiate a database recovery using geo-restore](sql-database-disaster-recovery.md#recover-using-geo-restore). Recovery usually takes place within 12 hours - with data loss of up to one hour determined by when the last hourly differential backup with taken and replicated. Until the recovery completes, the database is unable to record any transactions or respond to any queries. While this restores a database to the last available point in time, restoring the geo-secondary to any point in time is not currently supported.

> [!NOTE]
> If the data center comes back online before you switch your application over to the recovered database, you can cancel the recovery.  
>
>

### Perform post failover / recovery tasks
After recovery from either recovery mechanism, you must perform the following additional tasks before your users and applications are back up and running:

* Redirect clients and client applications to the new server and restored database
* Ensure appropriate server-level firewall rules are in place for users to connect (or use [database-level firewalls](sql-database-firewall-configure.md#creating-and-managing-firewall-rules))
* Ensure appropriate logins and master database level permissions are in place (or use [contained users](https://msdn.microsoft.com/library/ff929188.aspx))
* Configure auditing, as appropriate
* Configure alerts, as appropriate

## Upgrade an application with minimal downtime
Sometimes an application must be taken offline because of planned maintenance such as an application upgrade. [Manage application upgrades](sql-database-manage-application-rolling-upgrade.md) describes how to use active geo-replication to enable rolling upgrades of your cloud application to minimize downtime during upgrades and provide a recovery path if something goes wrong. 

## Next steps
For a discussion of application design considerations for stand-alone databases and for elastic pools, see [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) and [Elastic Pool disaster recovery strategies](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
