---
title: Azure CLI Script Sample - Map a custom domain to a function app | Microsoft Docs
description: Azure CLI Script Sample - Map a custom domain to a function app in Azure.
services: functions
documentationcenter: 
author: ggailey777   
manager: cfowler
editor: 
tags: azure-service-management

ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/26/2018
ms.author: glenga
ms.custom: mvc
---
# Map a custom domain to a function app

This sample script creates a function app with related resources, and then maps `www.<yourdomain>` to it. 
When your function app is hosted in an [App Service plan](../functions-scale.md#app-service-plan), you can map a custom domain using either a CNAME or an A record. For function apps in a [Consumption plan](../functions-scale.md#consumption-plan), only the CNAME option is supported.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

If you choose to install and use the CLI locally, you must use the Azure CLI version 2.0 or a later version. To find the version, run `az --version`. If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands: Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Creates a storage account required by the function app. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Creates an App Service plan required to map a custom domain. |
| [az functionapp create]() | Creates a function app. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Maps a custom domain to a function app. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure).

Additional Functions CLI script samples can be found in the [Azure Functions documentation]().
