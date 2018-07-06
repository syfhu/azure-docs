﻿---
title: WeChat configuration in Azure Active Directory B2C | Microsoft Docs
description: Provide sign-up and sign-in to consumers with WeChat accounts in your applications that are secured by Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
---

# Azure Active Directory B2C: Provide sign-up and sign-in to consumers with WeChat accounts

> [!NOTE]
> This feature is in preview.
> 

## Create a WeChat application

To use WeChat as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a WeChat application and supply it with the right parameters. You need a WeChat account to do this. If you don’t have one, you can get one by signing up through one of their mobile apps or by using your QQ number. After that, get your account registered with the WeChat developer program. You can find more information [here](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### Register a WeChat application

1. Go to [https://open.weixin.qq.com/](https://open.weixin.qq.com/) and log in.
2. Click on **管理中心** (management center).
3. Follow the necessary steps to register a new application.
4. For **授权回调域** (callback URL), enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Find and copy the **APP ID** and **APP KEY**. You will need these later.

## Configure WeChat as an identity provider in your tenant
1. Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.
2. On the B2C features blade, click **Identity providers**.
3. Click **+Add** at the top of the blade.
4. Provide a friendly **Name** for the identity provider configuration. For example, enter "WeChat".
5. Click **Identity provider type**, select **WeChat**, and click **OK**.
6. Click **Set up this identity provider**
7. Enter the **App Key** that you copied earlier as the **Client ID**.
8. Enter the **App Secret** that you copied earlier as the **Client Secret**.
9. Click **OK** and then click **Create** to save your WeChat configuration.

