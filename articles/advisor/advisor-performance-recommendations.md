﻿---
title: Azure Advisor Performance recommendations | Microsoft Docs
description: Use Advisor to optimize the performance of your Azure deployments.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''

ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
---

# Advisor Performance recommendations

Azure Advisor performance recommendations help improve the speed and responsiveness of your business-critical applications. You can get performance recommendations from Advisor on the **Performance** tab of the Advisor dashboard.

## Improve database performance with SQL DB Advisor

Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources. It integrates with SQL Database Advisor to bring you recommendations for improving the performance of your SQL Azure database. SQL Database Advisor assesses the performance of your SQL Azure databases by analyzing your usage history. It then offers recommendations that are best suited for running the database’s typical workload. 

> [!NOTE]
> To get recommendations, a database must have about a week of usage, and within that week there must be some consistent activity. SQL Database Advisor can optimize more easily for consistent query patterns than for random bursts of activity.

For more information about SQL Database Advisor, see [SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## Improve Redis Cache performance and reliability

Advisor identifies Redis Cache instances where performance may be adversely affected by high memory usage, server load, network bandwidth, or a large number of client connections. Advisor also provides best practices recommendations to help you avoid potential issues. For more information about Redis Cache recommendations, see [Redis Cache Advisor](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## Improve App Service performance and reliability

Azure Advisor integrates best practices recommendations for improving your App Services experience and discovering relevant platform capabilities. Examples of App Services recommendations are:
* Detection of instances where memory or CPU resources are exhausted by app runtimes with mitigation options.
* Detection of instances where collocating resources like web apps and databases can improve performance and lower cost. 

For more information about App Services recommendations, see [Best Practices for Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## How to access Performance recommendations in Advisor

1. Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).

2.	On the Advisor dashboard, click the **Performance** tab.

## Next steps

To learn more about Advisor recommendations, see:

* [Introduction to Advisor](advisor-overview.md)
* [Get started with Advisor](advisor-get-started.md)
* [Advisor Cost recommendations](advisor-performance-recommendations.md)
* [Advisor High Availability recommendations](advisor-high-availability-recommendations.md)
* [Advisor Security recommendations](advisor-security-recommendations.md)

