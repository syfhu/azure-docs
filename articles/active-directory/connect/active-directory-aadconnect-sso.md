---
title: 'Azure AD Connect: Seamless Single Sign-On | Microsoft Docs'
description: This topic describes Azure Active Directory (Azure AD) Seamless Single Sign-On and how it allows you to provide true single sign-on for corporate desktop users inside your corporate network.
services: active-directory
keywords: what is Azure AD Connect, install Active Directory, required components for Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
---
# Azure Active Directory Seamless Single Sign-On

## What is Azure Active Directory Seamless Single Sign-On?

Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate devices connected to your corporate network. When enabled, users don't need to type in their passwords to sign in to Azure AD, and usually, even type in their usernames. This feature provides your users easy access to your cloud-based applications without needing any additional on-premises components.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Seamless SSO can be combined with either the [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-hash-synchronization.md) or [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) sign-in methods.

![Seamless Single Sign-On](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Seamless SSO is _not_ applicable to Active Directory Federation Services (ADFS).

## Key benefits

- *Great user experience*
  - Users are automatically signed into both on-premises and cloud-based applications.
  - Users don't have to enter their passwords repeatedly.
- *Easy to deploy & administer*
  - No additional components needed on-premises to make this work.
  - Works with any method of cloud authentication - [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-hash-synchronization.md) or [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md).
  - Can be rolled out to some or all your users using Group Policy.
  - Register non-Windows 10 devices with Azure AD without the need for any AD FS infrastructure. This capability needs you to use version 2.1 or later of the [workplace-join client](https://www.microsoft.com/download/details.aspx?id=53554).

## Feature highlights

- Sign-in username can be either the on-premises default username (`userPrincipalName`) or another attribute configured in Azure AD Connect (`Alternate ID`). Both use cases work because Seamless SSO uses the `securityIdentifier` claim in the Kerberos ticket to look up the corresponding user object in Azure AD.
- Seamless SSO is an opportunistic feature. If it fails for any reason, the user sign-in experience goes back to its regular behavior - i.e, the user needs to enter their password on the sign-in page.
- If an application (for example,  https://myapps.microsoft.com/contoso.com) forwards a `domain_hint` (OpenID Connect) or `whr` (SAML) parameter - identifying your tenant, or `login_hint` parameter - identifying the user, in its Azure AD sign-in request, users are automatically signed in without them entering usernames or passwords.
- Users also get a silent sign-on experience if an application (for example, https://contoso.sharepoint.com) sends sign-in requests to Azure AD's tenanted endpoints - that is, https://login.microsoftonline.com/contoso.com/<..> or https://login.microsoftonline.com/<tenant_ID>/<..> - instead of Azure AD's common endpoint - that is, https://login.microsoftonline.com/common/<...>.
- Sign out is supported. This allows users to choose another Azure AD account to sign in with, instead of being automatically signed in using Seamless SSO automatically.
- Office 365 clients (16.0.8730.xxxx and above) are supported using a non-interactive flow.
- It can be enabled via Azure AD Connect.
- It is a free feature, and you don't need any paid editions of Azure AD to use it.
- It is supported on web browser-based clients and Office clients that support [modern authentication](https://aka.ms/modernauthga) on platforms and browsers capable of Kerberos authentication:

| OS\Browser |Internet Explorer|Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Yes|No|Yes|Yes\*|N/A
|Windows 8.1|Yes|N/A|Yes|Yes\*|N/A
|Windows 8|Yes|N/A|Yes|Yes\*|N/A
|Windows 7|Yes|N/A|Yes|Yes\*|N/A
|Mac OS X|N/A|N/A|Yes\*|Yes\*|Yes\*

\*Requires [additional configuration](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>For Windows 10, the recommendation is to use [Azure AD Join](../active-directory-azureadjoin-overview.md) for the optimal single sign-on experience with Azure AD.

## Next steps

- [**Quick Start**](active-directory-aadconnect-sso-quick-start.md) - Get up and running Azure AD Seamless SSO.
- [**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.
- [**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers to frequently asked questions.
- [**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how to resolve common issues with the feature.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.
