---
title: Azure CLI Script Sample - Get the hostname, ports, and keys for Azure Redis Cache | Microsoft Docs
description: Azure CLI Script Sample - Get the hostname, ports, and keys for an Azure Redis Cache instance
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management

ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
---

# Get the hostname, ports, and keys for Azure Redis Cache

In this scenario, you learn how to retrieve the hostname, ports, and keys used to connect to an Azure Redis Cache instance.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## Sample script

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## Script explanation

This script uses the following commands to retrieve the hostname, keys, and ports of an Azure Redis Cache instance. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Retrieve details of an Azure Redis Cache instance. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Retrieve access keys for an Azure Redis Cache instance. |


## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure).

Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).