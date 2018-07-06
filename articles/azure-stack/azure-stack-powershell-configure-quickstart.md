---
title: Install and configure PowerShell for Azure Stack quickstart  | Microsoft Docs
description: Learn about installing and configuring PowerShell for Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.assetid: 6996DFC1-5E05-423A-968F-A9427C24317C
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mabrigg

---

# Get up and running with PowerShell in Azure Stack

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

This quickstart helps you to install and configure an Azure Stack environment with PowerShell. The script that we provide in this article is scoped to the **Azure Stack operator** only.

This article is a condensed version of the steps that are described in the [Install PowerShell]( azure-stack-powershell-install.md), [Download tools]( azure-stack-powershell-download.md), and [Configure the Azure Stack operator's PowerShell environment]( azure-stack-powershell-configure-admin.md) articles. By using the scripts in this topic, you can set up PowerShell for Azure Stack environments that are deployed with Azure Active Directory or Active Directory Federation Services (AD FS).  


## Set up PowerShell for Azure Active Directory-based deployments

Sign in to your Azure Stack Development Kit, or a Windows-based external client if you are connected through VPN. Open an elevated PowerShell ISE session, and then run the following script. Make sure to update the **TenantName**, **ArmEndpoint**, and **GraphAudience** variables as necessary for your environment configuration:

```powershell
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Add-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## Set up PowerShell for AD FS-based deployments

You can use the following script if you are operating Azure Stack when connected to internet. However if you are operating Azure Stack without internet connectivity, use the [disconnected way of installing PowerShell](azure-stack-powershell-install.md#install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) and the cmdlets to configure PowerShell will remain same as shown in this script. Sign in to your Azure Stack Development Kit, or a Windows-based external client if you are connected through VPN. Open an elevated PowerShell ISE session, and then run the following script. Make sure to update the **ArmEndpoint** and **GraphAudience** variables as necessary for your environment configuration:

```powershell

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Add-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID
```

## Test the connectivity

Now that you’ve configured PowerShell, you can test the configuration by creating a resource group:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

> [!note]  
> To specify a resource group, you will need to have a resource group in your subscription. For more information about subscriptions, see [Plan, offer, quota, and subscription overview](azure-stack-plan-offer-quota-overview.md)

After the resource group is created, the **Provisioning state** property is set to **Succeeded**.

## Next steps

* [Install and configure CLI](azure-stack-connect-cli.md)

* [Develop templates](user/azure-stack-develop-templates.md)
