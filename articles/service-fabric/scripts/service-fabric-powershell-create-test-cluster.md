﻿---
title: Azure PowerShell Script Sample - Create a Service Fabric cluster | Microsoft Docs
description: Azure PowerShell Script Sample - Create a three-node test Service Fabric cluster.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management

ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: ryanwi
ms.custom: mvc
---

# Create a three-node test Service Fabric cluster

This sample script creates a three-node test Service Fabric cluster secured with an X.509 certificate. The three node cluster configuration is supported for dev/test because you can safely perform upgrades and survive individual node failures (as long as they don't happen simultaneously). Production cluster require five or more nodes in order to be resilient to simultaneous failures.  

The command creates a self-signed certificate and uploads it to a new key vault, which is created in the same resource group as the cluster. The certificate is also copied to a local directory.  Set the *-OS* parameter to choose the version of Windows or Linux that runs on the cluster nodes.  Customize the parameters as needed.

If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview) and then run `Connect-AzureRmAccount` to create a connection with Azure. 

## Sample script

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## Clean up deployment 

After the script sample has been run, the following command can be used to remove the resource group, cluster, and all related resources.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Creates a new Service Fabric cluster. |

## Next steps

For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).

Additional Azure Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).
