﻿---
title: Azure CLI Script Sample - Restrict web traffic | Microsoft Docs
description: Azure CLI Script Sample - Create an application gateway with a web application firewall and a virtual machine scale set that uses OWASP rules to restrict traffic.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager

ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
---

# Restrict web traffic using the Azure CLI

This script creates an application gateway with a web application firewall that uses a virtual machine scale set for backend servers. The web application firewall restricts web traffic based on OWASP rules. After running the script, you can test the application gateway using its public IP address.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## Clean up deployment 

Run the following command to remove the resource group, application gateway, and all related resources.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## Script explanation

This script uses the following commands to create the deployment. Each item in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az_net) | Creates a virtual network. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Creates a subnet in a virtual network. |
| [az network public-ip create](https://docs.microsoft.com/en-us/cli/azure/network/public-ip?view=azure-cli-latest) | Creates the public IP address for the application gateway. |
| [az network application-gateway create](https://docs.microsoft.com/en-us/cli/azure/network/application-gateway?view=azure-cli-latest) | Create an application gateway. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Creates a virtual machine scale set. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Creates a storage account. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) | Creates a storage account. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_show) | Gets the public IP address of the application gateway. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).

Additional application gateway CLI script samples can be found in the [Azure application gateway documentation](../cli-samples.md).
