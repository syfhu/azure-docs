﻿---
title: Manage Azure Search with Powershell scripts | Microsoft Docs
description: Manage your Azure Search service with PowerShell scripts. Create or update an Azure Search service and manage Azure Search admin keys
author: HeidiSteen
manager: cgronlun
tags: azure-resource-manager
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/15/2016
ms.author: heidist

---
# Manage your Azure Search service with PowerShell
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

This topic describes the PowerShell commands to perform many of the management tasks for Azure Search services. We will walk through creating a search service, scaling it, and managing its API keys.
These commands parallel the management options available in the [Azure Search Management REST API](https://docs.microsoft.com/rest/api/searchmanagement).

## Prerequisites
* You must have Azure PowerShell 1.0 or greater. For instructions, see [Install and configure Azure PowerShell](/powershell/azure/overview).
* You must be logged in to your Azure subscription in PowerShell as described below.

First, you must login to Azure with this command:

    Connect-AzureRmAccount

Specify the email address of your Azure account and its password in the Microsoft Azure login dialog.

Alternatively you can [login non-interactively with a service principal](../azure-resource-manager/resource-group-authenticate-service-principal.md).

If you have multiple Azure subscriptions, you need to set your Azure subscription. To see a list of your current subscriptions, run this command.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

To specify the subscription, run the following command. In the following example, the subscription name is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## Commands to help you get started
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource

## Next Steps
Now that your service is created, you can take the next steps: build an [index](search-what-is-an-index.md), [query an index](search-query-overview.md), and finally create and manage your own search application that uses Azure Search.

* [Create an Azure Search index in the Azure portal](search-create-index-portal.md)
* [Query an Azure Search index using Search Explorer in the Azure portal](search-explorer.md)
* [Setup an indexer to load data from other services](search-indexer-overview.md)
* [How to use Azure Search in .NET](search-howto-dotnet-sdk.md)
* [Analyze your Azure Search traffic](search-traffic-analytics.md)

