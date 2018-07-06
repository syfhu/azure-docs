---
title: Application page does not display correctly for an Application Proxy application | Microsoft Docs
description: Guidance when the page isn’t displaying correctly in an Application Proxy Application you have integrated with Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen

---

# Application page does not display correctly for an Application Proxy application

This article helps you troubleshoot issues with Azure Active Directory Application Proxy applications when you navigate to the page, but something on the page doesn't look correct.

## Overview
When you publish an Application Proxy app, only pages under your root are accessible when accessing the application. If the page isn’t displaying correctly, the root internal URL used for the application may be missing some page resources. To resolve, make sure you have published *all* the resources for the page as part of your application.

You can verify if missing resources is the issue by opening your network tracker (such as Fiddler, or F12 tools in Internet Explorer/Edge), loading the page, and looking for 404 errors. That indicates the pages currently cannot be found and that you need to publish them.

As an example of this case, assume you have published an expenses application using the internal URL http://myapps/expenses, but the app uses the stylesheet http://myapps/style.css. In this case, the stylesheet is not published in your application, so loading the expenses app throw a 404 error while trying to load style.css. In this example, the problem is resolved by publishing the application with an internal URL http://myapp/.

## Problems with publishing as one application

If it is not possible to publish all resources within the same application, you need to publish multiple applications and enable links between them.

To do so, we recommend using the [custom domains](manage-apps/application-proxy-configure-custom-domain.md) solution. However, this solution requires that you own the certificate for your domain and your applications use fully qualified domain names (FQDNs). For other options, see the [troubleshoot broken links documentation](application-proxy-page-links-broken-problem.md).

## Next steps
[Publish applications using Azure AD Application Proxy](manage-apps/application-proxy-publish-azure-portal.md)
