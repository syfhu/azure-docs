---
title: Limitations of Azure Active Directory B2B collaboration | Microsoft Docs
description: Current limitations for Azure Active Directory B2B collaboration

services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017

ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram

---

# Limitations of Azure AD B2B collaboration
Azure Active Directory (Azure AD) B2B collaboration is currently subject to the limitations described in this article.

## Possible double multi-factor authentication
With Azure AD B2B, you can enforce multi-factor authentication at the resource organization (the inviting organization). The reasons for this approach are detailed in [Conditional access for B2B collaboration users](conditional-access.md). If a partner already has multi-factor authentication set up and enforced, their users might have to perform the authentication once in their home organization and then again in yours.

## Instant-on
In the B2B collaboration flows, we add users to the directory and dynamically update them during invitation redemption, app assignment, and so on. The updates and writes ordinarily happen in one directory instance and must be replicated across all instances. Replication is completed once all instances are updated. Sometimes when the object is written or updated in one instance and the call to retrieve this object is to another instance, replication latencies can occur. If that happens, refresh or retry to help. If you are writing an app using our API, then retries with some back-off is a good, defensive practice to alleviate this issue.

## Next steps

See the following articles on Azure AD B2B collaboration:

- [What is Azure AD B2B collaboration?](what-is-b2b.md)
- [Delegate B2bB collaboration invitations](delegate-invitations.md)

