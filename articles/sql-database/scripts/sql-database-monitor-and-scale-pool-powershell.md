﻿---
title: PowerShell example-monitor-scale-SQL elastic pool-Azure SQL Database | Microsoft Docs
description: Azure PowerShell example script to monitor and scale a SQL elastic pool in Azure SQL Database
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: carlrab
tags: azure-service-management

ms.assetid:
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/01/2018
ms.author: carlrab
---

# Use PowerShell to monitor and scale a SQL elastic pool in Azure SQL Database

This PowerShell script example monitors the performance metrics of an elastic pool, scales it to a higher performance level, and creates an alert rule on one of the performance metrics. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## Sample script

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]

## Clean up deployment

After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
 [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Creates a resource group in which all resources are stored. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Creates a logical server that hosts a database or elastic pool. |
| [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Creates an elastic pool within a logical server. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Creates a database in a logical server as a single or a pooled database. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Shows the size usage information for the database.|
| [Add-AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Adds or updates a metric-based alert rule. |
| [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | Updates elastic pool properties |
| [Add-AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Sets an alert rule to automatically monitor DTUs in the future. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Deletes a resource group including all nested resources. |
|||

## Next steps

For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).

Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).
