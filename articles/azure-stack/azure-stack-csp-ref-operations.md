---
title: Register tenants for usage tracking in Azure Stack | Microsoft Docs
description: Details about operations used to manage  tenant registrations and how tenant usage is tracked in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: mabrigg
ms.reviewer: alfredo

---

# Manage tenant registration in Azure Stack

*Applies to: Azure Stack integrated systems*

This article contains details about operations you can use to manage your tenant registrations, and how tenant usage is tracked. You can find details about how to add, list, or remove tenant mappings. You can use PowerShell or the Billing API endpoints to manage your use tracking.

## Add tenant to registration

You use this operation when you want to add a new tenant to your registration, so that their usage is reported under an Azure subscription connected with their Azure Active Directory (Azure AD) tenant.

You can also use this operation if you want to change the subscription associated with a tenant, you can call PUT/New-AzureRMResource again. The old mapping is overwritten.

Note that only one Azure subscription can be associated with a tenant. If you try to add a second subscription to an existing tenant, the first subscription is over-written. 


| Parameter                  | Description |
|---                         | --- |
| registrationSubscriptionID | The Azure subscription that was used for the initial registration. |
| customerSubscriptionID     | The  Azure subscription (not Azure Stack) belonging to the customer to be registered. Must be created in the Cloud Service Provider (CSP) offer. In practice, this means through Partner Center. If a customer has more than one tenant, this subscription must be created in the tenant that will be used to log into Azure Stack. |
| resourceGroup              | The resource group in Azure in which your registration is stored. |
| registrationName           | The name of the registration of your Azure Stack. It is an object stored in Azure. The name is usually in the form azurestack-CloudID, where CloudID is the Cloud ID of your Azure Stack deployment. |

> [!Note]  
> Tenants need to be registered with each Azure Stack they use. If a tenant uses more than one Azure Stack, you need to update the initial registrations of each deployment with the tenant subscription.

### PowerShell

Use the New-AzureRmResource cmdlet to update the registration resource. Sign in to Azure (`Add-AzureRmAccount`) using the account you used for the initial registration. Here is an example of how to add a tenant:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### API call

**Operation**: PUT  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Response**: 201 Created  
**Response Body**: Empty  

## List all registered tenants

Get a list of all tenants that have been added to a registration.

 > [!Note]  
 > If no tenants have been registered, you won't receive a response.

### Parameters

| Parameter                  | Description          |
|---                         | ---                  |
| registrationSubscriptionId | The Azure subscription that was used for the initial registration.   |
| resourceGroup              | The resource group in Azure in which your registration is stored.    |
| registrationName           | The name of the registration of your Azure Stack. It is an object stored in Azure. The name is usually in the form of **azurestack**-***CloudID***, where ***CloudID*** is the Cloud ID of your Azure Stack deployment.   |

### PowerShell

Use the Get-AzureRmResovurce cmdlet to list all registered tenants. Sign in to Azure (`Add-AzureRmAccount`) using the account you used for the initial registration. Here is an example of how to add a tenant:

```powershell
  Get-AzureRmResovurce -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### API Call

You can get a list of all tenant mappings using the GET operation

**Operation**: GET  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Response**: 200  
**Response Body**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## Remove a tenant mapping

You can remove a tenant that has been added to a registration. If that tenant is still using resources on Azure Stack, their usage is charged to the subscription used in the initial Azure Stack registration.

### Parameters

| Parameter                  | Description          |
|---                         | ---                  |
| registrationSubscriptionId | Subscription ID for the registration.   |
| resourceGroup              | The resource group for the registration.   |
| registrationName           | The name of the registration.  |
| customerSubscriptionId     | The customer subscription ID.  |

### PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### API Call

You can remove tenant mappings using the DELETE operation.

**Operation**: DELETE  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Response**: 204 No Content  
**Response Body**: Empty

## Next steps

 - To learn more about how to retrieve resource usage information from Azure Stack, see [Usage and billing in Azure Stack](/azure-stack-billing-and-chargeback.md).
