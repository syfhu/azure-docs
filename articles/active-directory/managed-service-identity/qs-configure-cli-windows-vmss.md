---
title: How to configure system and user assigned identities on an Azure VMSS using Azure CLI
description: Step by step instructions for configuring system and user assigned identities on an Azure VMSS, using Azure CLI.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 

ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
---

# Configure a virtual machine scale set Managed Service Identity (MSI) using Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity provides Azure services with an automatically managed identity in Azure Active Directory. You can use this identity to authenticate to any service that supports Azure AD authentication, without having credentials in your code. 

In this article, you learn how to perform the following Managed Service Identity operations on an Azure Virtual Machine Scale Set (VMSS), using the Azure CLI:
- Enable and disable the system assigned identity on an Azure VMSS
- Add and remove a user assigned identity on an Azure VMSS


## Prerequisites

- If you're unfamiliar with Managed Service Identity, check out the [overview section](overview.md). **Be sure to review the [difference between a system assigned and user assigned identity](overview.md#how-does-it-work)**.
- If you don't already have an Azure account, [sign up for a free account](https://azure.microsoft.com/free/) before continuing.

To run the CLI script examples, you have three options:

- Use [Azure Cloud Shell](../../cloud-shell/overview.md) from the Azure portal (see next section).
- Use the embedded Azure Cloud Shell via the "Try It" button, located in the top right corner of each code block.
- [Install the latest version of CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 or later) if you prefer to use a local CLI console. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## System assigned identity

In this section, you learn how to enable and disable the system assigned identity for an Azure VMSS using Azure CLI.

### Enable system assigned identity during creation of an Azure virtual machine scale set

To create a virtual machine scale set with the system assigned identity enabled:

1. If you're using the Azure CLI in a local console, first sign in to Azure using [az login](/cli/azure/reference-index#az_login). Use an account that is associated with the Azure subscription under which you would like to deploy the virtual machine scale set:

   ```azurecli-interactive
   az login
   ```

2. Create a [resource group](../../azure-resource-manager/resource-group-overview.md#terminology) for containment and deployment of your virtual machine scale set and its related resources, using [az group create](/cli/azure/group/#az_group_create). You can skip this step if you already have a resource group you would like to use instead:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Create a virtual machine scale set using [az vmss create](/cli/azure/vmss/#az_vmss_create) . The following example creates a virtual machine scale set named *myVMSS* with a system assigned identity, as requested by the `--assign-identity` parameter. The `--admin-username` and `--admin-password` parameters specify the administrative user name and password account for virtual machine sign-in. Update these values as appropriate for your environment: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### Enable system assigned identity on an existing Azure virtual machine scale set

If you need to enable the system assigned identity on an existing Azure virtual machine scale set:

1. If you're using the Azure CLI in a local console, first sign in to Azure using [az login](/cli/azure/reference-index#az_login). Use an account that is associated with the Azure subscription that contains the virtual machine scale set.

   ```azurecli-interactive
   az login
   ```

2. Use [az vmss identity assign](/cli/azure/vmss/identity/#az_vmss_identity_assign) command to enable a system assigned identity to an existing VM:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### Disable system assigned identity from an Azure virtual machine scale set

> [!NOTE]
> Disabling Managed Service Identity from a Virtual Machine Scale Set is currently not supported. In the meantime, you can switch between using System Assigned and User Assigned Identities. Check back for updates.

If you have a virtual machine scale set that no longer needs a system assigned identity, but still needs user assigned identities, perform the following command:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='UserAssigned' 
```

To remove the MSI VM extension, use [az vmss identity remove](/cli/azure/vmss/identity/#az_vmss_remove_identity) command to remove the system assigned identity from a VMSS:

   ```azurecli-interactive
   az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
   ```

## User assigned identity

In this section, you learn how to enable and remove a user assigned identity using Azure CLI.

### Assign a user assigned identity during the creation of an Azure VMSS

This section walks you through creation of an VMSS and assignment of a user assigned identity to the VMSS. If you already have a VMSS you want to use, skip this section and proceed to the next.

1. You can skip this step if you already have a resource group you would like to use. Create a [resource group](~/articles/azure-resource-manager/resource-group-overview.md#terminology) for containment and deployment of your user assigned identity, using [az group create](/cli/azure/group/#az_group_create). Be sure to replace the `<RESOURCE GROUP>` and `<LOCATION>` parameter values with your own values. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Create a user assigned identity using [az identity create](/cli/azure/identity#az-identity-create).  The `-g` parameter specifies the resource group where the user assigned identity is created, and the `-n` parameter specifies its name. Be sure to replace the `<RESOURCE GROUP>` and `<USER ASSIGNED IDENTITY NAME>` parameter values with your own values:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
The response contains details for the user assigned identity created, similar to the following. The resource `id` value assigned to the user assigned identity is used in the following step.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Create a VMSS using [az vmss create](/cli/azure/vmss/#az-vmss-create). The following example creates a VMSS associated with the new user assigned identity, as specified by the `--assign-identity` parameter. Be sure to replace the `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>`, and `<USER ASSIGNED IDENTITY ID>` parameter values with your own values. For `<USER ASSIGNED IDENTITY ID>`, use the user assigned identity's resource `id` property created in the previous step: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### Assign a user assigned identity to an existing Azure VM

1. Create a user assigned identity using [az identity create](/cli/azure/identity#az-identity-create).  The `-g` parameter specifies the resource group where the user assigned identity is created, and the `-n` parameter specifies its name. Be sure to replace the `<RESOURCE GROUP>` and `<USER ASSIGNED IDENTITY NAME>` parameter values with your own values:

    > [!IMPORTANT]
    > Creating user assigned identities with special characters (i.e. underscore) in the name is not currently supported. Please use alphanumeric characters. Check back for updates.  For more information see [FAQs and known issues](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
The response contains details for the user assigned identity created, similar to the following. The resource `id` value assigned to the user assigned identity is used in the following step.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Assign the user assigned identity to your VMSS using [az vmss identity assign](/cli/azure/vmss/identity#az_vm_assign_identity). Be sure to replace the `<RESOURCE GROUP>` and `<VMSS NAME>` parameter values with your own values. The `<USER ASSIGNED IDENTITY ID>` will be the user assigned identity's resource `id` property, as created in the previous step:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### Remove a user assigned identity from an Azure VMSS

> [!NOTE]
>  Removing all user assigned identities from a Virtual Machine Scale Set is currently not supported, unless you have a system assigned identity. 

If your VMSS has multiple user assigned identities, you can remove all but the last one using [az vmss identity remove](/cli/azure/vmss/identity#az-vmss-identity-remove). Be sure to replace the `<RESOURCE GROUP>` and `<VMSS NAME>` parameter values with your own values. The `<MSI NAME>` is the user assigned identity's name property, which can be found by in the identity section of the VM using `az vm show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```
If your VMSS has both system assigned and user assigned identities, you can remove all the user assigned identities by switching to use only system assigned. Use the following command: 

```azurecli-interactive
az vmss update -n <VMSS NAME> -g <RESOURCE GROUP> --set identity.type='SystemAssigned' identity.identityIds=null
```

## Next steps

- [Managed Service Identity overview](overview.md)
- For the full Azure virtual machine scale set creation Quickstart, see: 

  - [Create a Virtual Machine Scale Set with CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















