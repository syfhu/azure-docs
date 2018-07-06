﻿---
title: Create identity for Azure app with PowerShell | Microsoft Docs
description: Describes how to use Azure PowerShell to create an Azure Active Directory application and service principal, and grant it access to resources through role-based access control. It shows how to authenticate application with a certificate.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/10/2018
ms.author: tomfitz
---

# Use Azure PowerShell to create a service principal with a certificate

When you have an app or script that needs to access resources, you can set up an identity for the app and authenticate the app with its own credentials. This identity is known as a service principal. This approach enables you to:

* Assign permissions to the app identity that are different than your own permissions. Typically, these permissions are restricted to exactly what the app needs to do.
* Use a certificate for authentication when executing an unattended script.

> [!IMPORTANT]
> Instead of creating a service principal, consider using Azure AD Managed Service Identity for your application identity. Azure AD MSI is a public preview feature of Azure Active Directory that simplifies creating an identity for code. If your code runs on a service that supports Azure AD MSI and accesses resources that support Azure Active Directory authentication, Azure AD MSI is a better option for you. To learn more about Azure AD MSI, including which services currently support it, see [Managed Service Identity for Azure resources](../active-directory/managed-service-identity/overview.md).

This article shows you how to create a service principal that authenticates with a certificate. To set up a service principal with password, see [Create an Azure service principal with Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).

You must have the [latest version](/powershell/azure/get-started-azureps) of PowerShell for this article.

## Required permissions

To complete this article, you must have sufficient permissions in both your Azure Active Directory and Azure subscription. Specifically, you must be able to create an app in the Azure Active Directory, and assign the service principal to a role.

The easiest way to check whether your account has adequate permissions is through the portal. See [Check required permission](resource-group-create-service-principal-portal.md#required-permissions).

## Create service principal with self-signed certificate

The following example covers a simple scenario. It uses [New-​Azure​Rm​AD​Service​Principal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) to create a service principal with a self-signed certificate, and uses [New-​Azure​Rm​Role​Assignment](/powershell/module/azurerm.resources/new-azurermroleassignment) to assign the [Contributor](../role-based-access-control/built-in-roles.md#contributor) role to the service principal. The role assignment is scoped to your currently selected Azure subscription. To select a different subscription, use [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

The example sleeps for 20 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory. If your script doesn't wait long enough, you'll see an error stating: "Principal {ID} does not exist in the directory {DIR-ID}." To resolve this error, wait a moment then run the **New-AzureRmRoleAssignment** command again.

You can scope the role assignment to a specific resource group by using the **ResourceGroupName** parameter. You can scope to a specific resource by also using the **ResourceType** and **ResourceName** parameters. 

If you **do not have Windows 10 or Windows Server 2016**, you need to download the [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from Microsoft Script Center. Extract its contents and import the cmdlet you need.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

In the script, substitute the following two lines to generate the certificate.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### Provide certificate through automated PowerShell script

Whenever you sign in as a service principal, you need to provide the tenant ID of the directory for your AD app. A tenant is an instance of Azure Active Directory.

```powershell
$TenantId = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzureRmADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## Create service principal with certificate from Certificate Authority

The following example uses a certificate issued from a Certificate Authority to create service principal. The assignment is scoped to the specified Azure subscription. It adds the service principal to the [Contributor](../role-based-access-control/built-in-roles.md#contributor) role. If an error occurs during the role assignment, it retries the assignment.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### Provide certificate through automated PowerShell script
Whenever you sign in as a service principal, you need to provide the tenant ID of the directory for your AD app. A tenant is an instance of Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

The application ID and tenant ID aren't sensitive, so you can embed them directly in your script. If you need to retrieve the tenant ID, use:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

If you need to retrieve the application ID, use:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## Change credentials

To change the credentials for an AD app, either because of a security compromise or a credential expiration, use the [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) and [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.

To remove all the credentials for an application, use:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | Remove-AzureRmADAppCredential
```

To add a certificate value, create a self-signed certificate as shown in this article. Then, use:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | New-AzureRmADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## Debug

You may get the following errors when creating a service principal:

* **"Authentication_Unauthorized"** or **"No subscription found in the context."** - You see this error when your account does not have the [required permissions](#required-permissions) on the Azure Active Directory to register an app. Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator to either assign you to an administrator role, or to enable users to register apps.

* Your account **"does not have authorization to perform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions to assign a role to an identity. Ask your subscription administrator to add you to User Access Administrator role.

## Next steps
* To set up a service principal with password, see [Create an Azure service principal with Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).
* For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide to authorization with the Azure Resource Manager API](resource-manager-api-authentication.md).
* For a more detailed explanation of applications and service principals, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md). 
* For more information about Azure Active Directory authentication, see [Authentication Scenarios for Azure AD](../active-directory/active-directory-authentication-scenarios.md).
