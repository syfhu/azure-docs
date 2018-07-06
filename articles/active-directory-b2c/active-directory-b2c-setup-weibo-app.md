﻿---
title: Weibo configuration in Azure Active Directory B2C | Microsoft Docs
description: Provide sign-up and sign-in to consumers with Weibo accounts in your applications that are secured by Azure Active Directory B2C.
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

# Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Weibo accounts

> [!NOTE]
> This feature is in preview. Do not use this identity provider in your production environment.
> 

## Create a Weibo application

To use Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Weibo application and supply it with the right parameters. You need a Weibo account to do this. If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

### Register for the Weibo developer program

1. Go to the [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.
2. After signing in, click on your display name in the top-right corner.
3. In the dropdown, select **编辑开发者信息** (edit developer information).
4. Enter the required information into the form and click **提交** (submit).
5. Complete the email verification process.
6. Go to the [identity verification page](http://open.weibo.com/developers/identity/edit).
7. Enter the required information into the form and click **提交** (submit).

### Register a Weibo application

1. Go to the [new Weibo app registration page](http://open.weibo.com/apps/new).
2. Enter the necessary application information.
3. Click **创建** (create).
4. Copy the values of **App Key** and **App Secret**. You will need this later.
5. Upload the required photos and enter the necessary information.
6. Click **保存以上信息** (save).
7. Click **高级信息** (advanced information).
8. Click **编辑** (edit) next to the field for OAuth2.0 **授权设置** (redirect URL).
9. Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL). For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Click **提交** (submit).  

## Configure Weibo as an identity provider in your tenant
1. Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.
2. On the B2C features blade, click **Identity providers**.
3. Click **+Add** at the top of the blade.
4. Provide a friendly **Name** for the identity provider configuration. For example, enter "Weibo".
5. Click **Identity provider type**, select **Weibo**, and click **OK**.
6. Click **Set up this identity provider**
7. Enter the **App Key** that you copied earlier as the **Client ID**.
8. Enter the **App Secret** that you copied earlier as the **Client Secret**.
9. Click **OK** and then click **Create** to save your Weibo configuration.

