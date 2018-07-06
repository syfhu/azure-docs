---
title: Create a function in Azure that is deployed from Visual Studio Team Services | Microsoft Docs 
description: Create a Function App and deploy function code from Visual Studio Team Services
services: functions 
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
---
# Create a function app and deploy function code from Visual Studio Team Services

This topic shows you how to use Azure Functions to create a [serverless](https://azure.microsoft.com/overview/serverless-computing/) function app using the [consumption plan](../functions-scale.md#consumption-plan). The function app, which is a container for your functions, is continuously deployed from a Visual Studio Team Services (VSTS) repository. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

To complete this topic, you must have:

* A VSTS repository that contains your function app project and to which you have administrative permissions.
* A [personal access token (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) to access your VSTS repository.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

If you rather use the Azure CLI locally, you must install and use version 2.0 or a later version. To determine the Azure CLI version, run `az --version`. If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## Sample script

This sample creates an Azure Function app and deploys function code from Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands to create a resource group, storage account, function app, and all related resources. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Associates a function app with a Git or Mercurial repository. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure).

Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).
