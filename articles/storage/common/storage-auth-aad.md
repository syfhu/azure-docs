---
title: Authenticate access to Azure Storage using Azure Active Directory (Preview) | Microsoft Docs
description: Authenticate access to Azure Storage using Azure Active Directory (Preview).  
services: storage
author: tamram
manager: jeconnoc

ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
---

# Authenticate access to Azure Storage using Azure Active Directory (Preview)

Azure Storage supports authentication and authorization with Azure Active Directory (AD) for the Blob and Queue services. With Azure AD, you can use role-based access control (RBAC) to grant access to users, groups, or application service principals. 

Authorizing applications that access Azure Storage using Azure AD provides superior security and ease of use over other authorization options. While you can continue to use Shared Key authorization with your applications, using Azure AD circumvents the need to store your account access key with your code. Similarly, you can continue to use shared access signatures (SAS) to grant fine-grained access to resources in your storage account, but Azure AD offers similar capabilities without the need to manage SAS tokens or worry about revoking a compromised SAS.

## About the preview

Keep in mind the following points about the preview:

- Azure AD integration is available for the Blob and Queue services only in the preview.
- Azure AD integration is available for GPv1, GPv2, and Blob storage accounts in all public regions. 
- Only storage accounts created with the Resource Manager deployment model are supported. 
- Support for caller identity information in Azure Storage Analytics logging is coming soon.
- Azure AD authorization of access to resources in standard storage accounts is currently supported. Authorization of access to page blobs in premium storage accounts will be supported soon.
- Azure Storage supports both built-in and custom RBAC roles. You can assign roles scoped to the subscription, the resource group, the storage account, or an individual container or queue.
- The Azure Storage client libraries that currently support Azure AD integration include:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (use 7.1.x-Preview)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [Queue](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> This preview is intended for non-production use only. Production service-level agreements (SLAs) will not be available until Azure AD integration for Azure Storage is declared generally available. If Azure AD integration is not yet supported for your scenario, continue to use Shared Key authorization or SAS tokens in your applications.
>
> During the preview, RBAC role assignments may take up to five minutes to propagate.
>
> Azure AD integration with Azure Storage requires that you use HTTPS for Azure Storage operations.

## Get started with Azure AD for Storage

The first step in using Azure AD integration with Azure Storage is to assign RBAC roles for storage data to your service principal (a user, group, or application service principal) or Managed Service Identity (MSI). RBAC roles encompass common sets of permissions for containers and queues. To learn more about RBAC roles for Azure Storage, see [Manage access rights to storage data with RBAC (Preview)](storage-auth-aad-rbac.md).

To use Azure AD to authorize access to storage resources in your applications, you need to request an OAuth 2.0 access token from your code. To learn how to request an access token and use it to authorize requests to Azure Storage, see [Authenticate with Azure AD from an Azure Storage application (Preview)](storage-auth-aad-app.md). If you are using an Azure Managed Service Identity (MSI), see [Authenticate with Azure AD from an Azure VM Managed Service Identity (Preview)](storage-auth-aad-msi.md).

Azure CLI and PowerShell now support logging in with an Azure AD identity. After you log in with an Azure AD identity, your session runs under that identity. To learn more, see [Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)](storage-auth-aad-script.md).

## Next steps

For additional information about Azure AD integration for Azure Blobs and Queues, see the Azure Storage team blog post, [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
