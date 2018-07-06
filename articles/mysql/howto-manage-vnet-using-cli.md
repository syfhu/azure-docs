---
title: Create and manage Azure Database for MySQL VNet service endpoints and rules using Azure CLI | Microsoft Docs
description: This article describes how to create and manage Azure Database for MySQL VNet service endpoints and rules using Azure CLI command line.
services: mysql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/01/2018
---
# Create and manage Azure Database for MySQL VNet service endpoints using Azure CLI
Virtual Network (VNet) services endpoints and rules extend the private address space of a Virtual Network to your Azure Database for MySQL server. Using convenient Azure Command Line Interface (CLI) commands, you can create, update, delete, list, and show VNet service endpoints and rules to manage your server. For an overview of Azure Database for MySQL VNet service endpoints, including limitations, see [Azure Database for MySQL Server VNet service endpoints](concepts-data-access-and-security-vnet.md). VNet service endpoints are available in public preview in all supported regions for Azure Database for MySQL.

## Prerequisites
To step through this how-to guide, you need:
- Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use the Azure Cloud Shell in the browser.
- An [Azure Database for MySQL server and database](quickstart-create-mysql-server-database-using-azure-cli.md).

> [!NOTE]
> Support for VNet service endpoints is only for General Purpose and Memory Optimized servers.

## Configure Vnet service endpoints for Azure Database for MySQL
The [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) commands are used to configure Virtual Networks.

If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

If you choose to install and use the CLI locally, this article requires that you are running the Azure CLI version 2.0 or later. To see the version installed, run the `az --version` command. If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli). 

If you are running the CLI locally, you need to log in to your account using the [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) command. Note the **id** property from the command output for the corresponding subscription name.
```azurecli-interactive
az login
```

Install the CLI extension for Azure Database for MySQL VNet service endpoints using the `az extension add --name rdbms-vnet` command. 
```azurecli-interactive
az extension add --name rdbms-vnet
```

Run the `az extension list` command to verify the installation of the CLI extension.
```azurecli-interactive
az extension list
```
The output of the command lists all installed extensions. The Azure Database for MySQL CLI extension is:

 {
    "extensionType": "whl",
    "name": "rdbms-vnet",
    "version": "10.0.0"
  }

> [!NOTE]
> To uninstall the CLI extension run the `az extension remove -n rdbms-vnet` command. 

If you have multiple subscriptions, choose the appropriate subscription in which the resource should be billed. Select the specific subscription ID under your account using [az account set](/cli/azure/account#az_account_set) command. Substitute the **id** property from the **az login** output for your subscription into the subscription id placeholder.

- The account must have the necessary permissions to create a virtual network and service endpoint.

Service endpoints can be configured on virtual networks independently, by a user with write access to the virtual network.

To secure Azure service resources to a VNet, the user must have permission to "Microsoft.Network/JoinServicetoaSubnet" for the subnets being added. This permission is included in the built-in service administrator roles, by default and can be modified by creating custom roles.

Learn more about [built-in roles](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) and assigning specific permissions to [custom roles](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

VNets and Azure service resources can be in the same or different subscriptions. If the VNet and Azure service resources are in different subscriptions, the resources should be under the same Active Directory (AD) tenant, at the time of this preview.

> [!IMPORTANT]
> It is highly recommended to read this article about service endpoint configurations and considerations before running the sample script below, or configuring service endpoints. **Virtual Network service endpoint:** A [Virtual Network service endpoint](../virtual-network/virtual-network-service-endpoints-overview.md) is a subnet whose property values include one or more formal Azure service type names. VNet services endpoints use the service type name **Microsoft.Sql**, which refers to the Azure service named SQL Database. This service tag also applies to the Azure SQL Database, Azure Database for PostgreSQL and MySQL services. It is important to note when applying the **Microsoft.Sql** service tag to a VNet service endpoint it configures service endpoint traffic for all Azure Database services, including Azure SQL Database, Azure Database for PostgreSQL and Azure Database for MySQL servers on the subnet. 
> 

### Sample script to create an Azure Database for MySQL database, create a VNet, VNet service endpoint and secure the server to the subnet with a VNet rule
In this sample script, change the highlighted lines to customize the admin username and password. Replace the SubscriptionID used in the `az account set --subscription` command with your own subscription identifier.
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/create-mysql-server.sh?highlight=5,20 "Create an Azure Database for MySQL, VNet, VNet service endpoint, and VNet rule.")]

## Clean up deployment
After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/delete-mysql.sh "Delete the resource group.")]
