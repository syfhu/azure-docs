---
title: "Troubleshoot Azure SQL Data Sync | Microsoft Docs"
description: "Learn how to troubleshoot common issues with Azure SQL Data Sync."
services: sql-database
ms.date: 06/20/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.custom: data-sync
---
# Troubleshoot issues with SQL Data Sync

This article describes how to troubleshoot known issues with Azure SQL Data Sync. If there is a resolution for an issue, it's provided here.

For an overview of SQL Data Sync, see [Sync data across multiple cloud and on-premises databases with Azure SQL Data Sync](sql-database-sync-data.md).

## Sync issues

### Sync fails in the portal UI for on-premises databases that are associated with the client agent

#### Description and symptoms

Sync fails on the SQL Data Sync portal UI for on-premises databases that are associated with the agent. On the local computer that's running the agent, you see System.IO.IOException errors in the Event Log. The errors say that the disk has insufficient space.

#### Resolution

Create more space on the drive on which the %TEMP% directory is located.

### My sync group is stuck in the processing state

#### Description and symptoms

A sync group in SQL Data Sync has been in the processing state for a long time. It doesn't respond to the **stop** command, and the logs show no new entries.

#### Cause

Any of the following conditions might result in a sync group being stuck in the processing state:

-   **The client agent is offline**. Be sure that the client agent is online and then try again.

-   **The client agent is uninstalled or missing**. If the client agent is uninstalled or otherwise missing:

    1. Remove the agent XML file from the SQL Data Sync installation folder, if the file exists.
    2. Install the agent on an on-premises computer (it can be the same or a different computer). Then, submit the agent key that's generated in the portal for the agent that's showing as offline.

-   **The SQL Data Sync service is stopped**.

    1. In the **Start** menu, search for **Services**.
    2. In the search results, select **Services**.
    3. Find the **SQL Data Sync** service.
    4. If the service status is **Stopped**, right-click the service name, and then select **Start**.

#### Resolution

If the preceding information doesn't move your sync group out of the processing state, Microsoft Support can reset the status of your sync group. To have your sync group status reset, in the [Azure SQL Database forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), create a post. In the post, include your subscription ID and the sync group ID for the group that needs to be reset. A Microsoft Support engineer will respond to your post, and will let you know when the status has been reset.

### I see erroneous data in my tables

#### Description and symptoms

If tables that have the same name but which are from different database schemas are included in a sync, you see erroneous data in the tables after the sync.

#### Cause

The SQL Data Sync provisioning process uses the same tracking tables for tables that have the same name but which are in different schemas. Because of this, changes from both tables are reflected in the same tracking table. This causes erroneous data changes during sync.

#### Resolution

Ensure that the names of tables that are involved in a sync are different, even if the tables belong to different schemas in a database.

### I see inconsistent primary key data after a successful sync

#### Description and symptoms

A sync is reported as successful, and the log shows no failed or skipped rows, but you observe that primary key data is inconsistent among the databases in the sync group.

#### Cause

This result is by design. Changes in any primary key column result in inconsistent data in the rows where the primary key was changed.

#### Resolution

To prevent this issue, ensure that no data in a primary key column is changed.

To fix this issue after it has occurred, delete the row that has inconsistent data from all endpoints in the sync group. Then, reinsert the row.

### I see a significant degradation in performance

#### Description and symptoms

Your performance degrades significantly, possibly to the point where you can't even open the Data Sync UI.

#### Cause

The most likely cause is a sync loop. A sync loop occurs when a sync by sync group A triggers a sync by sync group B, which then triggers a sync by sync group A. The actual situation might be more complex, and it might involve more than two sync groups in the loop. The issue is that there is a circular triggering of syncing that's caused by sync groups overlapping one another.

#### Resolution

The best fix is prevention. Ensure that you don't have circular references in your sync groups. Any row that is synced by one sync group can't be synced by another sync group.

### I see this message: "Cannot insert the value NULL into the column \<column\>. Column does not allow nulls." What does this mean, and how can I fix it? 
This error message indicates that one of the two following issues has occurred:
-  A table doesn't have a primary key. To fix this issue, add a primary key to all the tables that you're syncing.
-  There's a WHERE clause in your CREATE INDEX statement. Data Sync doesn't handle this condition. To fix this issue, remove the WHERE clause or manually make the changes to all databases. 
 
### How does Data Sync handle circular references? That is, when the same data is synced in multiple sync groups, and keeps changing as a result?
Data Sync doesn’t handle circular references. Be sure to avoid them. 

## Client agent issues

### The client agent install, uninstall, or repair fails

#### Cause

Many scenarios might cause this failure. To determine the specific cause for this failure, look at the logs.

#### Resolution

To find the specific cause of the failure, generate and look at the Windows Installer logs. You can turn on logging at a command prompt. For example, if the downloaded AgentServiceSetup.msi file is LocalAgentHost.msi, generate and examine log files by using the following command lines:

-   For installs: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
-   For uninstalls: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

You can also turn on logging for all installations that are performed by Windows Installer. The Microsoft Knowledge Base article [How to enable Windows Installer logging](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) provides a one-click solution to turn on logging for Windows Installer. It also provides the location of the logs.

### My client agent doesn't work after I cancel the uninstall

#### Description and symptoms

The client agent doesn't work, even after you cancel its uninstallation.

#### Cause

This occurs because the SQL Data Sync client agent doesn't store credentials.

#### Resolution

You can try these two solutions:

-   Use services.msc to reenter the credentials for the client agent.
-   Uninstall this client agent and then install a new one. Download and install the latest client agent from [Download Center](http://go.microsoft.com/fwlink/?linkid=221479).

### My database isn't listed in the agent list

#### Description and symptoms

When you attempt to add an existing SQL Server database to a sync group, the database doesn't appear in the list of agents.

#### Cause

These scenarios might cause this issue:

-   The client agent and sync group are in different datacenters.
-   The client agent's list of databases isn't current.

#### Resolution

The resolution depends on the cause.

- **The client agent and sync group are in different datacenters**

    The client agent and the sync group must be in the same datacenter. To set this up, you have two options:

    -   Create a new agent in the datacenter where the sync group is located. Then, register the database with that agent.
    -   Delete the current sync group. Then, re-create the sync group in the datacenter where the agent is located.

- **The client agent's list of databases isn't current**

    Stop and then restart the client agent service.

    The local agent downloads the list of associated databases only on the first submission of the agent key. It doesn't download the list of associated databases on subsequent agent key submissions. Databases that are registered during an agent move don't show up in the original agent instance.

### Client agent doesn't start (Error 1069)

#### Description and symptoms

You discover that the agent isn't running on a computer that hosts SQL Server. When you attempt to manually start the agent, you see a dialog box that displays the message, "Error 1069: The service did not start due to a logon failure."

![Data Sync error 1069 dialog box](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

#### Cause

A likely cause of this error is that the password on the local server has changed since you created the agent and agent password.

#### Resolution

Update the agent's password to your current server password:

1. Locate the SQL Data Sync client agent service.  
    a. Select **Start**.  
    b. In the search box, enter **services.msc**.  
    c. In the search results, select **Services**.  
    d. In the **Services** window, scroll to the entry for **SQL Data Sync Agent**.  
2. Right-click **SQL Data Sync Agent**, and then select **Stop**.
3. Right-click **SQL Data Sync Agent**, and then select **Properties**.
4. On **SQL Data Sync Agent Properties**, select the **Log in** tab.
5. In the **Password** box, enter your password.
6. In the **Confirm Password** box, reenter your password.
7. Select **Apply**, and then select **OK**.
8. In the **Services** window, right-click the **SQL Data Sync Agent** service, and then click **Start**.
9. Close the **Services** window.

### I can't submit the agent key

#### Description and symptoms

After you create or re-create a key for an agent, you try to submit the key through the SqlAzureDataSyncAgent application. The submission fails to complete.

![Sync Error dialog box - Can't submit agent key](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Before you proceed, check for the following conditions:

-   The SQL Data Sync Windows service is running.  
-   The service account for SQL Data Sync Windows service has network access.    
-   The outbound 1433 port is open in your local firewall rule.
-   The local ip is added to the server or database firewall rule for the sync metadata database.

#### Cause

The agent key uniquely identifies each local agent. The key must meet two conditions:

-   The client agent key on the SQL Data Sync server and the local computer must be identical.
-   The client agent key can be used only once.

#### Resolution

If your agent isn't working, it's because one or both of these conditions are not met. To get your agent to work again:

1. Generate a new key.
2. Apply the new key to the agent.

To apply the new key to the agent:

1. In File Explorer, go to your agent installation directory. The default installation directory is C:\\Program Files (x86)\\Microsoft SQL Data Sync.
2. Double-click the bin subdirectory.
3. Open the SqlAzureDataSyncAgent application.
4. Select **Submit Agent Key**.
5. In the space provided, paste the key from your clipboard.
6. Select **OK**.
7. Close the program.

### The client agent can't be deleted from the portal if its associated on-premises database is unreachable

#### Description and symptoms

If a local endpoint (that is, a database) that is registered with a SQL Data Sync client agent becomes unreachable, the client agent can't be deleted.

#### Cause

The local agent can't be deleted because the unreachable database is still registered with the agent. When you try to delete the agent, the deletion process tries to reach the database, which fails.

#### Resolution

Use "force delete" to delete the unreachable database.

> [!NOTE]
> If sync metadata tables remain after a "force delete", use deprovisioningutil.exe to clean them up.

### Local Sync Agent app can't connect to the local sync service

#### Resolution

Try the following steps:

1. Exit the app.  
2. Open the Component Services Panel.  
    a. In the search box on the taskbar, enter **services.msc**.  
    b. In the search results, double-click **Services**.  
3. Stop the **SQL Data Sync** service.
4. Restart the **SQL Data Sync** service.  
5. Reopen the app.

## Setup and maintenance issues

### I get a "disk out of space" message

#### Cause

The "disk out of space" message might appear if leftover files need to be deleted. This might be caused by antivirus software, or files are open when delete operations are attempted.

#### Resolution

Manually delete the sync files that are in the %temp% folder (`del \*sync\* /s`). Then, delete the subdirectories in the %temp% folder.

> [!IMPORTANT]
> Don't delete any files while sync is in progress.

### I can't delete my sync group

#### Description and symptoms

Your attempt to delete a sync group fails.

#### Causes

Any of the following scenarios might result in failure to delete a sync group:

-   The client agent is offline.
-   The client agent is uninstalled or missing. 
-   A database is offline. 
-   The sync group is provisioning or syncing. 

#### Resolution

To resolve failure to delete a sync group:

-   Ensure that the client agent is online and then try again.
-   If the client agent is uninstalled or otherwise missing:  
    a. Remove the agent XML file from the SQL Data Sync installation folder, if the file exists.  
    b. Install the agent on an on-premises computer (it can be the same or a different computer). Then, submit the agent key that's generated in the portal for the agent that's showing as offline.
-   Ensure that the SQL Data Sync service is running:  
    a. In the **Start** menu, search for **Services**.  
    b. In the search results, select **Services**.  
    c. Find the **SQL Data Sync** service.  
    d. If the service status is **Stopped**, right-click the service name, and then select **Start**.
-   Ensure that your SQL databases and SQL Server databases are all online.
-   Wait until the provisioning or sync process finishes and then retry deleting the sync group.

### I can't unregister an on-premises SQL Server database

#### Cause

Most likely, you are trying to unregister a database that has already been deleted.

#### Resolution

To unregister an on-premises SQL Server database, select the database and then select **Force Delete**.

If this operation fails to remove the database from the sync group:

1. Stop and then restart the client agent host service:  
    a. Select the **Start** menu.  
    b. In the search box, enter **services.msc**.  
    c. In the **Programs** section of the search results pane, double-click **Services**.  
    d. Right-click the **SQL Data Sync** service.  
    e. If the service is running, stop it.  
    f. Right-click the service, and then select **Start**.  
    g. Check whether the database is still registered. If it is no longer registered, you're done. Otherwise, proceed with the next step.
2. Open the client agent app (SqlAzureDataSyncAgent).
3. Select **Edit Credentials**, and then enter the credentials for the database.
4. Proceed with unregistration.

### I don't have sufficient privileges to start system services

#### Cause

This error occurs in two situations:
-   The user name and/or the password are incorrect.
-   The specified user account doesn't have sufficient privileges to log on as a service.

#### Resolution

Grant log-on-as-a-service credentials to the user account:

1. Go to **Start** > **Control Panel** > **Administrative Tools** > **Local Security Policy** > **Local Policy** > **User Rights Management**.
2. Select **Log on as a service**.
3. In the **Properties** dialog box, add the user account.
4. Select **Apply**, and then select **OK**.
5. Close all windows.

### A database has an "Out-of-Date" status

#### Cause

SQL Data Sync removes databases that have been offline from the service for 45 days or more (as counted from the time the database went offline). If a database is offline for 45 days or more and then comes back online, its status is **Out-of-Date**.

#### Resolution

You can avoid an **Out-of-Date** status by ensuring that none of your databases go offline for 45 days or more.

If a database's status is **Out-of-Date**:

1. Remove the database that has an **Out-of-Date** status from the sync group.
2. Add the database back in to the sync group.

> [!WARNING]
> You lose all changes made to this database while it was offline.

### A sync group has an "Out-of-Date" status

#### Cause

If one or more changes fail to apply for the whole retention period of 45 days, a sync group can become outdated.

#### Resolution

To avoid an **Out-of-Date** status for a sync group, examine the results of your sync jobs in the history viewer on a regular basis. Investigate and resolve any changes that fail to apply.

If a sync group's status is **Out-of-Date**, delete the sync group and then re-create it.

### A sync group can't be deleted within three minutes of uninstalling or stopping the agent

#### Description and symptoms

You can't delete a sync group within three minutes of uninstalling or stopping the associated SQL Data Sync client agent.

#### Resolution

1. Remove a sync group while the associated sync agents are online (recommended).
2. If the agent is offline but is installed, bring it online on the on-premises computer. Wait for the status of the agent to appear as **Online** in the SQL Data Sync portal. Then, remove the sync group.
3. If the agent is offline because it was uninstalled:  
    a.  Remove the agent XML file from the SQL Data Sync installation folder, if the file exists.  
    b.  Install the agent on an on-premises computer (it can be the same or a different computer). Then, submit the agent key that's generated in the portal for the agent that's showing as offline.  
    c. Try to delete the sync group.

### What happens when I restore a lost or corrupted database?

If you restore a lost or corrupted database from a backup, there might be a nonconvergence of data in the sync groups to which the database belongs.

## Next steps
For more information about SQL Data Sync, see:

-   [Sync data across multiple cloud and on-premises databases with Azure SQL Data Sync](sql-database-sync-data.md)  
-   [Set up Azure SQL Data Sync](sql-database-get-started-sql-data-sync.md)  
-   [Best practices for Azure SQL Data Sync](sql-database-best-practices-data-sync.md)  
-   [Monitor Azure SQL Data Sync with Log Analytics](sql-database-sync-monitor-oms.md)  
-   Complete PowerShell examples that show how to configure SQL Data Sync:  
    -   [Use PowerShell to sync between multiple Azure SQL databases](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [Use PowerShell to sync between an Azure SQL Database and a SQL Server on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [Download the SQL Data Sync REST API documentation](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

For more information about SQL Database, see:

-   [SQL Database Overview](sql-database-technical-overview.md)
-   [Database Lifecycle Management](https://msdn.microsoft.com/library/jj907294.aspx)
