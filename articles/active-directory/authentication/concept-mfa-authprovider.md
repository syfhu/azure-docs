---
title: Get started Azure Multi-Factor Auth Provider | Microsoft Docs
description: Learn how to create an Azure Multi-Factor Auth Provider.

services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 12/08/2017

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi

---

# Getting started with an Azure Multi-Factor Authentication Provider
Two-step verification is available by default for global administrators who have Azure Active Directory, and Office 365 users. However, if you wish to take advantage of [advanced features](howto-mfa-mfasettings.md) then you should purchase the full version of Azure Multi-Factor Authentication (MFA).

An Azure Multi-Factor Auth Provider is used to take advantage of features provided by the full version of Azure MFA. It is for users who **do not have licenses through Azure MFA, Azure AD Premium, or Enterprise Mobility + Security (EMS)**.  Azure MFA, Azure AD Premium, and EMS include the full version of Azure MFA by default. If you have licenses, then you do not need an Azure Multi-Factor Auth Provider.

An Azure Multi-Factor Auth provider is required to download the SDK.

> [!IMPORTANT]
> The deprecation of the Azure Multi-Factor Authentication Software Development Kit (SDK) has been announced. This feature is no longer supported for new customers. Current customers can continue using the SDK until November 14, 2018. After that time, calls to the SDK will fail.

> [!IMPORTANT]
>To download the SDK, you need to create an Azure Multi-Factor Auth Provider even if you have Azure MFA, AAD Premium, or EMS licenses.  If you create an Azure Multi-Factor Auth Provider for this purpose and already have licenses, be sure to create the Provider with the **Per Enabled User** model. Then, link the Provider to the directory that contains the Azure MFA, Azure AD Premium, or EMS licenses. This configuration ensures that you are only billed if you have more unique users performing two-step verification than the number of licenses you own. 

## What is an MFA Provider?

If you don't have licenses for Azure Multi-Factor Authentication, you can create an auth provider to require two-step verification for your users.

There are two types of auth providers, and the distinction is around how your Azure subscription is charged. The per-authentication option calculates the number of authentications performed against your tenant in a month. This option is best if you have a number of users authenticating only occasionally. The per-user option calculates the number of individuals in your tenant who perform two-step verification in a month. This option is best if you have some users with licenses but need to extend MFA to more users beyond your licensing limits.

## Create an MFA Provider

Use the following steps to create an Azure Multi-Factor Authentication Provider in the Azure portal:

1. Sign in to the [Azure portal](https://portal.azure.com) as a global administrator. 
2. Select **Azure Active Directory** > **MFA Server** > **Providers**.

   ![Providers][Providers]

3. Select **Add**.
4. Fill in the following fields and then select **Add**:
   - **Name** – The name of the Provider.
   - **Usage Model** – Choose one of two options:
      * Per Authentication – purchasing model that charges per authentication. Typically used for scenarios that use Azure Multi-Factor Authentication in a consumer-facing application.
      * Per Enabled User – purchasing model that charges per enabled user. Typically used for employee access to applications such as Office 365. Choose this option if you have some users that are already licensed for Azure MFA.
   - **Subscription** – The Azure subscription that is charged for two-step verification activity through the Provider. 
   - **Directory** – The Azure Active Directory tenant that the Provider is associated with.
      * You do not need an Azure AD directory to create a Provider. Leave this box blank if you are only planning to download the Azure Multi-Factor Authentication Server.
      * The Provider must be associated with an Azure AD directory to take advantage of the advanced features.
      * Only one Provider can be associated with any one Azure AD directory.

## Manage your MFA Provider

You cannot change the usage model (per enabled user or per authentication) after an MFA provider is created. However, you can delete the MFA provider and then create one with a different usage model.

If the current Multi-Factor Auth Provider is associated with an Azure AD directory (also known as an Azure AD tenant), you can safely delete the MFA provider and create one that is linked to the same Azure AD tenant. Alternatively, if you purchased enough MFA, Azure AD Premium, or Enterprise Mobility + Security (EMS) licenses to cover all users that are enabled for MFA, you can delete the MFA provider altogether.

If your MFA provider is not linked to an Azure AD tenant, or you link the new MFA provider to a different Azure AD tenant, user settings and configuration options are not transferred. Also, existing Azure MFA Servers need to be reactivated using activation credentials generated through the new MFA Provider. Reactivating the MFA Servers to link them to the new MFA Provider doesn't impact phone call and text message authentication, but mobile app notifications will stop working for all users until they reactivate the mobile app.

## Next steps

[Configure Multi-Factor Authentication settings](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "Add MFA Providers"
