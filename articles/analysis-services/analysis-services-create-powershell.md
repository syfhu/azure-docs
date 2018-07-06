﻿---
title: Quickstart - Create an Azure Analysis Services server by using PowerShell | Microsoft Docs
description: Learn how to create an Azure Analysis Services server by using PowerShell
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
#Customer intent: As a BI developer, I want to create an Azure Analysis Services server by using PowerShell.
---

# Quickstart: Create a server - PowerShell

This quickstart describes using PowerShell from the command line to create an Azure Analysis Services server in your Azure subscription.

## Prerequisites

- **Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) to create an account.
- **Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant and you must have an account in that directory. To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).
- **Azure PowerShell module version 4.0 or later**. To find the version, run ` Get-Module -ListAvailable AzureRM`. To install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).

## Import AzureRm.AnalysisServices module

To create a server in your subscription, you use the [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)  component module. Load the AzureRm.AnalysisServices module into your PowerShell session.

```powershell
Import-Module AzureRM.AnalysisServices
```

## Log in to Azure

Log in to your Azure subscription by using the [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) command. Follow the on-screen directions.

```powershell
Connect-AzureRmAccount
```

## Create a resource group

An [Azure resource group](../azure-resource-manager/resource-group-overview.md) is a logical container where Azure resources are deployed and managed as a group. When you create your server, you must specify a resource group in your subscription. If you do not already have a resource group, you can create a new one by using the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command. The following example creates a resource group named `myResourceGroup` in the West US region.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## Create a server

Create a new server by using the [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command. The following example creates a server named myServer in myResourceGroup, in the WestUS region, at the D1 (free) tier, and specifies philipc@adventureworks.com as a server administrator.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## Clean up resources

You can remove the server from your subscription by using the [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command. If you continue with other quickstarts and tutorials in this collection, do not remove your server. The following example removes the server created in the previous step.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## Next steps

In this quickstart, you learned how to create a server in your Azure subscription by using PowerShell. Now that you have server, you can help secure it by configuring an (optional) server firewall. You can also add a basic sample data model to your server right from the portal. Having a sample model is helpful when learning about configuring model database roles and testing client connections. To learn more, continue to the tutorial for adding a sample model.

> [!div class="nextstepaction"]
> [Quickstart: Configure server firewall - Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Tutorial: Add a sample model to your server](analysis-services-create-sample-model.md)