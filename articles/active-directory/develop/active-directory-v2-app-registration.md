---
title: Register an application with the Azure AD v2.0 endpoint using the portal | Microsoft Docs
description: How to register an app with Microsoft for enabling sign-in and accessing Microsoft services using the v2.0 endpoint
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''

ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
---

# How to register an app with the v2.0 endpoint
To build an app that accepts both personal Microsoft account (MSA) & work or school account (Azure AD) sign-in, you'll first need to register an app with Microsoft. At this time, you won't be able to use any existing apps you may have with Azure AD or MSA - you'll need to create a brand new one.

> [!NOTE]
> Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint. To determine if you should use the v2.0 endpoint, read about the [v2.0 limitations](active-directory-v2-limitations.md).


## Visit the Microsoft app registration portal
First, navigate to the Microsoft app registration portal at [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Sign in with either a personal or work or school Microsoft account. If you don't have either, sign up for a new personal account.

Done? You should now be looking at your list of Microsoft apps, which is probably empty. Let's change that.

Click **Add an app**, and give it a name. The portal will assign your app a globally unique Application ID that you'll use later in your code. If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want to create an **Application Secret** here as well.

Next, add the **Platforms** that your app will use.

* For web-based apps, provide a **Redirect URI** where sign-in messages can be sent.
* For mobile apps, copy down the default redirect URI automatically created for you.
* For web APIs, a default scope to access the Web API is automatically created for you. You can choose to add additional scopes using the **Add Scope** button. You can also add any applications that are pre-authorized to use your Web API using the **Pre-authorized applications** form. 

Optionally, you can customize the look and feel of your sign-in page in the **Profile** section. Make sure to click **Save** before moving on.

> [!NOTE]
> When you create an application using [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), the application will be registered in the home tenant of the account that you use to sign into the portal. This means that you can not register an application in your Azure AD tenant using a personal Microsoft account. If you explicitly wish to register an application in a particular tenant, sign in with an account originally created in that tenant.


## Build a quickstart app
Now that you have a Microsoft app, you can complete one of the v2.0 quickstart tutorials. Here are a few recommendations:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

