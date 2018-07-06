---
title: Azure Key Vault Overview | Microsoft Docs
description: Azure Key Vault is a cloud service that works as a secure secrets store.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager

ms.assetid: 34af20ee-3fa7-4f28-9d98-6168b1759764
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 05/08/2018
ms.author: barclayn
#Customer intent: As an IT Pro, Decision maker or developer I am trying to learn what Key Vault is and if it offers anything that could be used in my organization.

---
# What is Azure Key Vault?

Azure Key Vault is a cloud service that works as a secure secrets store.

You have passwords, connection strings, and other pieces of information that are needed to keep your applications working. You want to make sure that this information is available but that it is secured. This is where Azure Key Vault can help. Azure Key Vault can help you securely store and manage application secrets.

Key Vault allows you to create multiple secure containers, called vaults. These vaults are backed by hardware security modules (HSMs). Vaults help reduce the chances of accidental loss of security information by centralizing the storage of application secrets. Key Vaults also control and log the access to anything stored in them. Azure Key Vault can handle requesting and renewing Transport Layer Security (TLS) certificates, providing the features required for a robust certificate lifecycle management solution.

 Azure Key vault is designed to support application keys and secrets. Key Vault is not intended to be used as a store for user passwords.

## Why use Azure Key Vault?

### Centralize application secrets

Centralizing storage of application secrets in Azure Key Vault allows you to control their distribution. This greatly reduces the chances that secrets may be accidentally leaked. When using Key Vault, application developers no longer need to store security information in their application. This eliminates the need to make this information part of the code. For example, an application may need to connect to a database. Instead of storing the connection string in the app codes, store it securely in Key Vault.

Your applications can securely access the information they need by using URIs that allow them to retrieve specific versions of a secret after the application’s key or secret is stored in Azure Key Vault. This happens without having to write custom code to protect any of the secret information.

### Securely store secrets

Keys are safeguarded by Azure, using industry-standard algorithms, key lengths, and hardware security modules (HSMs). The HSMs used are Federal Information Processing Standards (FIPS) 140-2 Level 2 validated.

Access to a key vault requires proper authentication and authorization before a caller (user or application) can get access. Authentication establishes the identity of the caller, while authorization determines the operations that they are allowed to perform.

Authentication is done via Azure Active Directory. Authorization may be done via role-based access control (RBAC) or Key Vault access policy. RBAC is used when dealing with the management of the vaults and key vault access policy is used when attempting to access data stored in a vault.

Azure Key Vaults may be either software- or hardware-HSM protected. For situations where you require added assurance you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary. Microsoft uses Thales hardware security modules. You can use Thales tools to move a key from your HSM to Azure Key Vault.

Finally, Azure Key Vault is designed so that Microsoft does not see or extract your keys.

### Monitor access and use

Once that you have created a couple of Key Vaults you will want to monitor how and when your keys and secrets are being accessed. You can do this by enabling logging for Key Vault. You can configure Azure Key Vault to:

- Archive to a storage account.
- Stream to an event hub.
- Send the logs to Log Analytics.

You have control over your logs and you may secure them by restricting access and you may also delete logs that you no longer need.

### Simplified administration of application secrets

When storing valuable data, you must take several steps. Security information must be secured, it must follow a lifecycle, it must be highly available. Azure Key Vault simplifies a lot of this by:

- Removing the need for in-house knowledge of HSMs.
- Scaling up on short notice to meet your organization’s usage spikes.
- Replicating the contents of your Key Vault within a region and to a secondary region. This ensures high availability and takes away the need of any action from the administrator to trigger the fail over.
- Providing standards Azure administration options via the portal, Azure CLI and PowerShell.
- Automating certain tasks on certificates that you purchase from Public CAs, such as enroll and renew.

In addition, Azure Key Vaults allow you to segregate application secrets. Applications may access only the vault that they are allowed to access, and they be limited to only perform specific operations. You can create an Azure Key Vault per application and restrict the secrets stored in a Key Vault to a specific application and team of developers.

### Integrate with other Azure services

As a secure store in Azure, Key Vault has been used to simplify scenarios like [Azure Disk Encryption](../security/azure-security-disk-encryption.md), the [always encrypted]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) functionality in SQL server and Azure SQL, [Azure web apps]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site). Key Vault itself can integrate with storage accounts, event hubs and log analytics.

## Next steps

- [Quickstart: Create an Azure Key Vault using the CLI](quick-create-cli.md)
- [Configure an Azure web application to read a secret from Key vault](tutorial-web-application-keyvault.md)