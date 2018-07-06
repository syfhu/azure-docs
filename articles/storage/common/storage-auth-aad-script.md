---
title: Run Azure CLI or PowerShell commands under an Azure AD identity to access Azure Storage (Preview) | Microsoft Docs
description: Azure CLI and PowerShell support logging in with an Azure AD identity to run commands on Azure Storage containers and queues and their data. An access token is provided for the session and used to authorize calling operations. Permissions depend on the role assigned to the Azure AD identity. 
services: storage
author: tamram
manager: jeconnoc

ms.service: storage
ms.topic: article
ms.date: 05/30/2018
ms.author: tamram
---

# Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)

Azure Storage provides preview extensions for Azure CLI and PowerShell that enable you to log in and run scripting commands under an Azure Active Directory (Azure AD) identity. The Azure AD identity can be a user, group, or application service principal, or it can be a [managed service identity](../../active-directory/managed-service-identity/overview.md). You can assign permissions to access storage resources to the Azure AD identity via role-based access control (RBAC). For more information about RBAC roles in Azure Storage, see [Manage access rights to Azure Storage data with RBAC (Preview)](storage-auth-aad-rbac.md).

When you log in to Azure CLI or PowerShell with an Azure AD identity, an access token is returned for accessing Azure Storage under that identity. That token is then automatically used by CLI or PowerShell to authorize operations against Azure Storage. For supported operations, you no longer need to pass an account key or SAS token with the command.

> [!IMPORTANT]
> This preview is intended for non-production use only. Production service-level agreements (SLAs) will not be available until Azure AD integration for Azure Storage is declared generally available. If Azure AD integration is not yet supported for your scenario, continue to use Shared Key authorization or SAS tokens in your applications. For additional information about the preview, see [Authenticate access to Azure Storage using Azure Active Directory (Preview)](storage-auth-aad.md).
>
> During the preview, RBAC role assignments may take up to five minutes to propagate.
>
> Azure AD integration with Azure Storage requires that you use HTTPS for Azure Storage operations.

## Supported operations

The preview extensions are supported for operations on containers and queues. Which operations you may call depends on the permissions granted to the Azure AD identity with which you log in to Azure CLI or PowerShell. Permissions to Azure Storage containers or queues are assigned via role-based access control (RBAC). For example, if a Data Reader role is assigned to the identity, then you can run scripting commands that read data from a container or queue. If a Data Contributer role is assigned to the identity, then you can run scripting commands that read, write, or delete a container or queue or the data they contain. 

For details about the permissions required for each Azure Storage operation on a container or queue, see [Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## Call CLI commands with an Azure AD identity

To install the preview extension for Azure CLI:

1. Make sure that you have installed Azure CLI version 2.0.32 or later. Run `az --version` to check your installed version.
2. Run the following command to install the preview extension: 

    ```azurecli
    az extension add -n storage-preview
    ```

The preview extension adds a new `--auth-mode` parameter to supported commands:

- Set the `--auth-mode` parameter to `login` to sign in using an Azure AD identity.
- Set the `--auth-mode` parameter to the legacy `key` value to attempt to query for an account key if no authentication parameters for the account are provided. 

For example, to download a blob in Azure CLI using an Azure AD identity, first run `az login`, then call the command with `--auth-mode` set to `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

The environment variable associated with the `--auth-mode` parameter is `AZURE_STORAGE_AUTH_MODE`.

## Call PowerShell commands with an Azure AD identity

To use Azure PowerShell to sign in with an Azure AD identity:

1. Make sure that you have the latest version of PowerShellGet installed. Run the following command to install the latest:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

2. Uninstall any previous installations of Azure PowerShell.
3. Install AzureRM:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Install the preview module:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Call the [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) cmdlet to create a context, and include the `-UseConnectedAccount` parameter. 
6. To call a cmdlet with an Azure AD identity, pass the context to the cmdlet.

The following example shows how to list the blobs in a container from Azure PowerShell using an Azure AD identity: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## Next steps

- To learn more about RBAC roles for Azure storage, see [Manage access rights to storage data with RBAC (Preview)](storage-auth-aad-rbac.md).
- To learn about using Managed Service Identity with Azure Storage, see [Authenticate with Azure AD from an Azure Managed Service Identity (Preview)](storage-auth-aad-msi.md).
- To learn how to authorize access to containers and queues from within your storage applications, see [Use Azure AD with storage applications](storage-auth-aad-app.md).
- For additional information about Azure AD integration for Azure Blobs and Queues, see the Azure Storage team blog post, [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
