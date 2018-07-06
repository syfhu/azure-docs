---
title: "Azure App Service IP Restrictions | Microsoft Docs" 
description: "How to use IP restrictions with Azure App Service" 
author: btardif
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''

ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal

---
# Azure App Service Static IP Restrictions #

IP Restrictions allow you to define a list of IP addresses that are allowed to access your app. The allow list can include individual IP addresses or a range of IP addresses defined by a subnet mask.

When a request to the app is generated from a client, the IP address is evaluated against the allow list. If the IP address is not in the list, the app replies with an [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) status code.

IP Restrictions are defined in the web.config that your app consumes at runtime (more exactly, restrictions are inserted in a set of allowed IP addresses in applicationHost.config file, so if you also add a set of allowed IP addresses in web.config file, they will take precedence). Under certain circumstances, some module might be executed before IP restrictions logic in the HTTP pipeline. When this happens, the request fails with a different HTTP error code.

IP Restrictions are evaluated on the same App Service plan instances assigned to your app.

To add an IP restriction rule to your app, use the menu to open **Network**>**IP Restrictions** and click on **Configure IP Restrictions**

![IP restrictions](media/app-service-ip-restrictions/ip-restrictions.png)  

From here, you can review the list of IP restriction rules defined for your app.

![list IP restrictions](media/app-service-ip-restrictions/browse-ip-restrictions.png)

You can click on **[+] Add** to add a new IP restriction rule.

![add IP restrictions](media/app-service-ip-restrictions/add-ip-restrictions.png)
