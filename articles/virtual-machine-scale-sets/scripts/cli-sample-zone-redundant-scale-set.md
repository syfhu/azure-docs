---
title: Azure CLI 2.0 Samples - Zone-redundant scale set | Microsoft Docs
description: Azure CLI 2.0 Samples
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager

ms.assetid:
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc

---

# Create a zone-redundant virtual machine scale set with PowerShell
This script creates a virtual machine scale set running Ubuntu across multiple Availability Zones. After running the script, you can access the virtual machine over RDP.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## Sample script
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh "Create zone-redundant scale set")]

## Clean up deployment
Run the following command to remove the resource group, scale set, and all related resources.

```azurecli-interactive
az group delete --name myResourceGroup
```

## Script explanation
This script uses the following commands to create a resource group, virtual machine scale set, and all related resources. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Creates a resource group in which all resources are stored. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Creates the virtual machine scale set and connects it to the virtual network, subnet, and network security group. A load balancer is also created to distribute traffic to multiple VM instances. This command also specifies the VM image to be used and administrative credentials.  |
| [az group delete](/cli/azure/ad/group#delete) | Deletes a resource group including all nested resources. |

## Next steps
For more information on the Azure CLI 2.0, see [Azure CLI 2.0 documentation](https://docs.microsoft.com/cli/azure/overview).

Additional virtual machine scale set Azure CLI 2.0 script samples can be found in the [Azure virtual machine scale set documentation](../cli-samples.md).