---
title: Manage usage and billing for Azure Stack as a Cloud Service Provider | Microsoft Docs
description: A walk through registering Azure Stack as a Cloud Provider and adding customers.
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

# Manage usage and billing for Azure Stack as a Cloud Service Provider 

*Applies to: Azure Stack integrated systems*

This article walks you through registering Azure Stack as a Cloud Provider (CSP) and adding customers.

As a CSP, you are likely to have many different customers using your Azure Stack. Each customer has a CSP subscription in Azure, and you will need to direct usage from your Azure Stack to each user’s subscription.

The following diagram shows the steps that you will need to choose your shared services account and register azure account with the account. When this has been done, you can onboard your end customers.

**Steps to add usage tracking as a CSP**

![Process for enabling usage and management as a Cloud Service Provider.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## Create a CSP or CSPSS subscription

### Cloud Service Provider subscription types

You will need to choose the type of shared services account that you use for Azure Stack. The types of subscriptions that can be used for registration of a multitenant Azure Stack are:

 - Cloud Service Provider 
 - Partner Shared Services subscription 

#### CSP Shared Services

Cloud Service Provider Shared Services (CSPSS) subscriptions are the preferred choice for registration when a Direct CSP or a CSP Distributor operates Azure Stack.

CSPSS subscriptions are associated with a shared-services tenant. When you register Azure Stack, you need to provide credentials for an account that is an owner of the subscription. The account you use to register Azure Stack can be different from the administrator account that you use for deployment; the two do *not* need to belong to the same domain. In other words, you may deploy using the tenant that you already use. For example you may use ContosoCSP.onmicrosoft.com, then register using a different tenant, for example IURContosoCSP.onmicrosoft.com. You will need to remember that you sign in using ContosoCSP.onmicrosoft.com when you do day-to-do Azure Stack administration. When you sign in to Azure using IURContosoCSP.onmicrosoft.com when you need to do registration operations.

Refer to the following for a description of CSPSS subscriptions, and instructions on how to create subscription [Add Azure Partner Shared Services](https://msdn.microsoft.com/partner-center/shared-services).

#### CSP subscriptions

Cloud Service Provider (CSP) subscriptions are the preferred choice for registration when a CSP reseller or an end customer operates  Azure Stack.

## Register Azure Stack

To register with Azure Stack, see [Register Azure Stack with your Azure Subscription](azure-stack-registration.md).

## Add end customer

To configure Azure Stack so that when a new tenant uses resources their usage will be reported to their Cloud Service Provider (CSP) subscription, see [Add tenant for usage and billing to Azure Stack](azure-stack-csp-howto-register-tenants.md).

## Charge the right subscriptions

Azure Stack uses a feature called registration. A registration is an object, stored in Azure, which documents which Azure subscription(s) to use to charge for a given Azure Stack. This section addresses the importance of registration.

Using registration Azure Stack can:
 - Forward Azure Stack usage data to Azure Commerce and bill an Azure subscription.
 - Report each customer’s usage on a different subscription with a multitenant Azure Stack deployment. Multitenancy enables Azure Stack to support different organizations on the same Azure Stack instance.

For each Azure Stack, there is one default subscription and as many tenant subscriptions as needed. The default subscription is an Azure subscription that is charged if there is no tenant-specific subscription. It must be the first to be registered. For multi-tenant usage reporting to work, the subscription must be a CSP or CSPSS subscription.

Then, the registration is updated with an Azure subscription for each tenant that is going to use Azure Stack. Tenant subscriptions must be of the CSP type and must roll up to the partner who owns the default subscription. In other words, you cannot register someone else’s customers.

When Azure Stack forwards usage information to global Azure, a service in Azure consults the registration and maps each tenant’s usage to the appropriate tenant subscription. If a tenant has not been registered, that usage goes to the default subscription for the Azure Stack instance from which it originated.

Since tenant subscriptions are CSP subscriptions, their bill is sent to the CSP partner, and usage information is not visible to the end customer.



## Next steps

 - To learn more about the CSP program, see [Cloud Solution Provider program](https://partnercenter.microsoft.com/en-us/partner/programs).
 - To learn more about how to retrieve resource usage information from Azure Stack, see [Usage and billing in Azure Stack](azure-stack-billing-and-chargeback.md).
