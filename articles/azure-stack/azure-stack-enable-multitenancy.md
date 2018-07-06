---
title: Enable multi-tenancy in Azure Stack | Microsoft Docs
description: Learn how to support multiple Azure Active Directory directories in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2018
ms.author: mabrigg

---

# Enable multi-tenancy in Azure Stack

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

You can configure Azure Stack to support users from multiple Azure Active Directory (Azure AD) tenants to use services in Azure Stack. For example, consider the following scenario:

 - You are the Service Administrator of contoso.onmicrosoft.com, where Azure Stack is installed.
 - Mary is the Directory Administrator of fabrikam.onmicrosoft.com, where guest users are located. 
 - Mary's company receives IaaS and PaaS services from your company, and needs to allow users from the guest directory (fabrikam.onmicrosoft.com) to sign in and use Azure Stack resources in contoso.onmicrosoft.com.

This guide provides the steps required, in the context of this scenario, to configure multi-tenancy in Azure Stack. In this scenario, you and Mary must complete steps to enable users from Fabrikam to sign in and consume services from the Azure Stack deployment in Contoso.  

## Before you begin

There are a few pre-requisites to account for before you configure multi-tenancy in Azure Stack:
  
 - You and Mary must coordinate administrative steps across both the directory Azure Stack is installed in (Contoso), and the guest directory (Fabrikam).  
 - Make sure you've [installed](azure-stack-powershell-install.md) and [configured](azure-stack-powershell-configure-admin.md) PowerShell for Azure Stack.
 - [Download the Azure Stack Tools](azure-stack-powershell-download.md), and import the Connect and Identity modules:

    ````PowerShell  
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ````

 - Mary will require [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) access to Azure Stack. 

## Configure Azure Stack directory
In this section, you configure Azure Stack to allow sign-ins from Fabrikam Azure AD directory tenants.

### Onboard guest directory tenant
Next, onboard the Guest Directory Tenant (Fabrikam) to Azure Stack.  This step configures Azure Resource Manager to accept users and service principals from the guest directory tenant.

````PowerShell  
## The following ARM endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location "local" `
 -ResourceGroupName $ResourceGroupName
````



## Configure guest directory
After you complete steps in the Azure Stack directory, Mary must provide consent to Azure Stack accessing the guest directory and register Azure Stack with the guest directory. 

### Registering Azure Stack with the guest directory
Once the guest directory administrator has provided consent for Azure Stack to access Fabrikam's directory, Mary must register Azure Stack with Fabrikam's directory tenant.

````PowerShell
## The following ARM endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````
## Direct users to sign in
Now that you and Mary have completed the steps to onboard Mary's directory, Mary can direct Fabrikam users to sign in.  Fabrikam users (that is, users with the fabrikam.onmicrosoft.com suffix) sign in by visiting https://portal.local.azurestack.external.  

Mary will direct any [foreign principals](../role-based-access-control/rbac-and-directory-admin-roles.md) in the Fabrikam directory (that is, users in the Fabrikam directory without the suffix of fabrikam.onmicrosoft.com) to sign in using https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  If they do not use this URL, they are sent to their default directory (Fabrikam) and receive an error that says their admin has not consented.

## Next Steps

- [Manage delegated providers](azure-stack-delegated-provider.md)
- [Azure Stack key concepts](azure-stack-key-features.md)
