---
title: Choose between Azure MFA cloud or server | Microsoft Docs
description: Choose the multi-factor authentication security solution that is right for you by asking, what am I trying to secure and where are my users located.

services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 10/02/2017

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi

---
# Choose the Azure Multi-Factor Authentication solution for you
Because there are several flavors of Azure Multi-Factor Authentication (MFA), we must answer a few questions to figure out which version is the proper one to use.  Those questions are:

* [What am I trying to secure](#what-am-i-trying-to-secure)
* [Where are the users located](#where-are-the-users-located)
* [What features do I need?](#what-features-do-i-need)

The following sections provide guidance on determining each of these answers.

## What am I trying to secure?
To determine the correct two-step verification solution, first we must answer the question of what are you trying to secure with a second method of authentication.  Is it an application that is in Azure?  Or a remote access system?  By determining what we are trying to secure, we can answer the question of where Multi-Factor Authentication needs to be enabled.  

| What are you trying to secure | MFA in the cloud | MFA Server |
| --- |:---:|:---:|
| First-party Microsoft apps |● |● |
| SaaS apps in the app gallery |● |  |
| Web applications published through Azure AD App Proxy |● |  |
| IIS applications not published through Azure AD App Proxy | |● |
| Remote access such as VPN, RDG | ● | ● |

## Where are the users located
Next, looking at where our users are located helps to determine the correct solution to use, whether in the cloud or on-premises using the MFA Server.

| User Location | MFA in the cloud | MFA Server |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD and on-premises AD using federation with AD FS |● |● |
| Azure AD and on-premises AD using DirSync, Azure AD Sync, Azure AD Connect - no password hash sync or pass-through authentication |● |● |
| Azure AD and on-premises AD using DirSync, Azure AD Sync, Azure AD Connect - with password hash sync or pass-through authentication |● | |
| On-premises Active Directory | |● |

## What features do I need?
The following table compares the features that are available with Multi-Factor Authentication in the cloud and the Multi-Factor Authentication Server.

| Feature | MFA in the cloud | MFA Server |
| --- |:---:|:---:|
| Mobile app notification as a second factor | ● | ● |
| Mobile app verification code as a second factor | ● | ● |
| Phone call as second factor | ● | ● |
| One-way SMS as second factor | ● | ● |
| Two-way SMS as second factor | | ●  (Deprecated)| 
| Hardware Tokens as second factor | | ● |
| App passwords for Office 365 clients that don’t support MFA | ● | |
| Admin control over authentication methods | ● | ● |
| PIN mode | | ● |
| Fraud alert |● | ● |
| MFA Reports |● | ● |
| One-Time Bypass | | ● |
| Custom greetings for phone calls | ● | ● |
| Customizable caller ID for phone calls | ● | ● |
| Trusted IPs | ● | ● |
| Remember MFA for trusted devices | ● | |
| Conditional access | ● | ● |
| Cache |  | ● |

## Next steps

Now that you understand the difference between Azure Multi-Factor Authentication in the cloud or the MFA Server on-premises, it's time to set up and use Azure Multi-Factor Authentication. **Select the icon that represents your scenario**

<center>

[![MFA in the cloud](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [![MFA Server](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
