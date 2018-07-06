---
title: Enable automatic tuning for Azure SQL Database | Microsoft Docs
description: You can enable automatic tuning on your Azure SQL Database easily.
services: sql-database
author: danimir 
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: vvasic 

---
# Enable automatic tuning

Azure SQL Database is an automatically managed data service that constantly monitors your queries and identifies the action that you can perform to improve performance of your workload. You can review recommendations and manually apply them, or let Azure SQL Database automatically apply corrective actions - this is known as **automatic tuning mode**. Automatic tuning can be enabled at the server or the database level.

## Enable automatic tuning on server
On the server level you can choose to inherit automatic tuning configuration from "Azure Defaults" or not to inherit the configuration. Azure defaults are FORCE_LAST_GOOD_PLAN is enabled, CREATE_INDEX is enabled, and DROP_INDEX is disabled.

### Azure portal
To enable automatic tuning on Azure SQL Database logical **server**, navigate to the server in Azure portal and then select **Automatic tuning** in the menu.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Please note that **DROP_INDEX** option at this time is not compatible with applications using partition switching and index hints and should not be enabled in these cases.
>

Select the automatic tuning options you want to enable and select **Apply**.

Automatic tuning options on a server are applied to all databases on this server. By default, all databases inherit configuration from their parent server, but this can be overridden and specified for each database individually.

### REST API
[Click here, to read more about how to enable automatic tuning on the server level via REST API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## Enable automatic tuning on an individual database

The Azure SQL Database enables you to individually specify the automatic tuning configuration for each database. On the database level you can choose to inherit automatic tuning configuration from the parent server, "Azure Defaults" or not to inherit the configuration. Azure Defaults are set to FORCE_LAST_GOOD_PLAN is enabled, CREATE_INDEX is enabled, and DROP_INDEX is disabled.

> [!NOTE]
> The general recommendation is to manage the automatic tuning configuration at **server level** so the same configuration settings can be applied on every database automatically. Configure automatic tuning on an individual database only if you need that database to have different settings than others inheriting settings from the same server.
>

### Azure portal

To enable automatic tuning on a **single database**, navigate to the database in Azure portal and select **Automatic tuning**.

Individual automatic tuning settings can be separately configured for each database. You can manually configure an individual automatic tuning option, or specify that an option inherits its settings from the server.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Please note that DROP_INDEX option at this time is not compatible with applications using partition switching and index hints and should not be enabled in these cases.

Once you have selected your desired configuration, click **Apply**.

### Rest API
[Click here to read more about how to enable automatic tuning on a single database via REST API](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning)

### T-SQL

To enable automatic tuning on a single database via T-SQL, connect to the database and execute the following query:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Setting automatic tuning to AUTO will apply Azure Defaults. Setting it to INHERIT, automatic tuning configuration will be inherited from the parent server. Choosing CUSTOM, you will need to manually configure automatic tuning.

To configure individual automatic tuning options via T-SQL, connect to the database and execute the query such as this one:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
Setting the individual tuning option to ON, will override any setting that database inherited and enable the tuning option. Setting it to OFF, will also override any setting that database inherited and disable the tuning option. Automatic tuning option, for which DEFAULT is specified, will inherit the configuration from the database level automatic tuning setting.  

## Disabled by the system
Automatic tuning is monitoring all the actions it takes on the database and in some cases it can determine that automatic tuning can't properly work on the database. In this situation, tuning option will be disabled by the system. In most cases this happens because Query Store is not enabled or it's in read-only state on a specific database.

## Configure automatic tuning e-mail notifications

See [Automatic tuning e-mail notifications](sql-database-automatic-tuning-email-notifications.md)

## Next steps
* Read the [Automatic tuning article](sql-database-automatic-tuning.md) to learn more about automatic tuning and how it can help you improve your performance.
* See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.
* See [Query Performance Insights](sql-database-query-performance.md) to learn about viewing the performance impact of your top queries.
