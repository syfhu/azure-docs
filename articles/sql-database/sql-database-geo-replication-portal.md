---
title: 'Azure portal: SQL Database geo-replication | Microsoft Docs'
description: Configure geo-replication for Azure SQL Database in the Azure portal and initiate failover
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab

---
# Configure active geo-replication for Azure SQL Database in the Azure portal and initiate failover

This article shows you how to configure active geo-replication for SQL Database in the [Azure portal](http://portal.azure.com) and to initiate failover.

To initiate failover with the Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with the Azure portal](sql-database-geo-replication-portal.md).

To configure active geo-replication by using the Azure portal, you need the following resource:

* An Azure SQL database: The primary database that you want to replicate to a different geographical region.

> [!Note]
Active geo-replication must be between databases in the same subscription.

## Add a secondary database
The following steps create a new secondary database in a geo-replication partnership.  

To add a secondary database, you must be the subscription owner or co-owner.

The secondary database has the same name as the primary database and has, by default, the same service level. The secondary database can be a single database or a database in an elastic pool. For more information, see [DTU-based purchasing model](sql-database-service-tiers-dtu.md) and [vCore-based purchasing model (preview)](sql-database-service-tiers-vcore.md).
After the secondary is created and seeded, data begins replicating from the primary database to the new secondary database.

> [!NOTE]
> If the partner database already exists (for example, as a result of terminating a previous geo-replication relationship) the command fails.
> 

1. In the [Azure portal](http://portal.azure.com), browse to the database that you want to set up for geo-replication.
2. On the SQL database page, select **geo-replication**, and then select the region to create the secondary database. You can select any region other than the region hosting the primary database, but we recommend the [paired region](../best-practices-availability-paired-regions.md).
   
    ![Configure geo-replication](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Select or configure the server and pricing tier for the secondary database.
   
    ![Configure secondary](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Optionally, you can add a secondary database to an elastic pool. To create the secondary database in a pool, click **elastic pool** and select a pool on the target server. A pool must already exist on the target server. This workflow does not create a pool.
5. Click **Create** to add the secondary.
6. The secondary database is created and the seeding process begins.
   
    ![Configure secondary](./media/sql-database-geo-replication-portal/seeding0.png)
7. When the seeding process is complete, the secondary database displays its status.
   
    ![Seeding complete](./media/sql-database-geo-replication-portal/seeding-complete.png)

## Initiate a failover

The secondary database can be switched to become the primary.  

1. In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.
2. On the SQL Database blade, select **All settings** > **geo-replication**.
3. In the **SECONDARIES** list, select the database you want to become the new primary and click **Failover**.
   
    ![failover](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Click **Yes** to begin the failover.

The command immediately switches the secondary database into the primary role. 

There is a short period during which both databases are unavailable (on the order of 0 to 25 seconds) while the roles are switched. If the primary database has multiple secondary databases, the command automatically reconfigures the other secondaries to connect to the new primary. The entire operation should take less than a minute to complete under normal circumstances. 

> [!NOTE]
> This command is designed for quick recovery of the database in case of an outage. It triggers failover without data synchronization (forced failover).  If the primary is online and committing transactions when the command is issued some data loss may occur. 
> 
> 

## Remove secondary database
This operation permanently terminates the replication to the secondary database, and changes the role of the secondary to a regular read-write database. If the connectivity to the secondary database is broken, the command succeeds but the secondary does not become read-write until after connectivity is restored.  

1. In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.
2. On the SQL database page, select **geo-replication**.
3. In the **SECONDARIES** list, select the database you want to remove from the geo-replication partnership.
4. Click **Stop Replication**.
   
    ![Remove secondary](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. A confirmation window opens. Click **Yes** to remove the database from the geo-replication partnership. (Set it to a read-write database not part of any replication.)

## Next steps
* To learn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).
* For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).

