---
title: include file
description: include file
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
---

### Virtual Networks

|  |  |
|---------|---------|
| [Allowed Application Gateway SKUs](../articles/azure-policy/scripts/allowed-app-gate-sku.md) | Requires that application gateways use an approved SKU. You specify an array of approved SKUs. |
| [No network peering to ER network](../articles/azure-policy/scripts/no-peering-er-net.md) | Prohibits a network peering from being associated to a network in a specified resource group. Use to prevent connection with central managed network infrastructure. You specify the name of the resource group to prevent association. |
| [No User Defined Route Table](../articles/azure-policy/scripts/no-user-def-route-table.md)  |Prohibits virtual networks from being deployed with a user-defined route table. |
| [NSG X on every subnet](../articles/azure-policy/scripts/nsg-on-subnet.md) | Requires that a specific network security group is used with every virtual subnet. You specify the ID of the network security group to use. |
| [Use approved subnet for VM network interfaces](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) | Requires that network interfaces use an approved subnet. You specify the ID of the approved subnet. |
| [Use approved vNet for VM network interfaces](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) | Requires that network interfaces use an approved virtual network. You specify the ID of the approved virtual network. |