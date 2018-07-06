---
title: Azure Active Directory Identity Protection - How to unblock users | Microsoft Docs
description: Learn how unblock users that were blocked by an Azure Active Directory Identity Protection policy.
services: active-directory
keywords: azure active directory identity protection, unblock user
documentationcenter: ''
author: MarkusVi
manager: mtillman

ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu

---
# Azure Active Directory Identity Protection - How to unblock users
With Azure Active Directory Identity Protection, you can configure policies to block users if the configured conditions are satisfied. Typically, a blocked user contacts help desk to become unblocked. This article explains the steps you can perform to unblock a blocked user.

## Determine the reason for blocking
As a first step to unblock a user, you need to determine the type of policy that has blocked the user because your next steps are depending on it.
With Azure Active Directory Identity Protection, a user can be either blocked by a sign-in risk policy or a user risk policy.

You can get the type of policy that has blocked a user from the heading in the dialog that was presented to the user during a sign-in attempt:

| Policy | User dialog |
| --- | --- |
| Sign-in risk |![Blocked sign-in](./media/active-directory-identityprotection-unblock-howto/02.png) |
| User risk |![Blocked account](./media/active-directory-identityprotection-unblock-howto/104.png) |

A user that is blocked by:

* A sign-in risk policy is also known as suspicious sign-in
* A user risk policy is also known as an account at risk

## Unblocking suspicious sign-ins
To unblock a suspicious sign-in, you have the following options:

1. **Sign in from a familiar location or device** - A common reason for blocked suspicious sign-ins are sign-in attempts from unfamiliar locations or devices. Your users can quickly determine whether this is the blocking reason by trying to sign-in from a familiar location or device.
2. **Exclude from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it. For more information, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
3. **Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy. For more information, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

## Unblocking accounts at risk
To unblock an account at risk, you have the following options:

1. **Reset password** - You can reset the user's password. For more information, see [manual secure password reset](active-directory-identityprotection.md#manual-secure-password-reset).
2. **Dismiss all risk events** - The user risk policy blocks a user if the configured user risk level for blocking access has been reached. You can reduce a user's risk level by manually closing reported risk events. For more information, see [closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Exclude from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it. For more information, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
4. **Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy. For more information, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

## Next steps
 Do you want to know more about Azure AD Identity Protection? Check out [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
