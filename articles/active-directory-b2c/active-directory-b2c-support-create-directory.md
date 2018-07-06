---
title: Troubleshoot creating tenants in Azure Active Directory B2C | Microsoft Docs
description: Issues and resolutions for creating an Azure Active Directory or Azure Active Directory B2C tenant.
services: active-directory-b2c
author: davidmu1
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
---

# Troubleshoot creating an Azure Active Directory or Azure Active Directory B2C tenant 

## Create an Azure AD tenant
If you can't create an Azure Active Directory (Azure AD) tenant on the first attempt, try again. If the problem persists, contact Azure Support.

## Create an Azure AD B2C tenant
If you encounter issues when you [create an Azure Active Directory B2C (Azure AD B2C) tenant](active-directory-b2c-get-started.md), try the following options:

* If the Azure AD B2C tenant doesn't show up in your list of tenants, try again to create the tenant.
* If the Azure AD B2C tenant does show up in your list of tenants and you see the following  error message, delete the tenant and create it again:

    "Could not complete the creation of the B2C tenant 'contosob2c'. Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."
* There are known issues when you delete an existing Azure AD B2C tenant and re-create it by using the same domain name. When you create a new Azure AD B2C tenant, you must use a different domain name.
* If these resolutions don't work, contact Azure Support. For more information, see [File support requests for Azure AD B2C](active-directory-b2c-support.md).

