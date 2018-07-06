---
title: Portals for creating and editing log queries in Azure Log Analytics | Microsoft Docs
description: This article describes the portals that you can use in Azure Log Analytics to create and edit log searches.  
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: magoedte; bwren
ms.component: na
---

# Portals for creating and editing log queries in Azure Log Analytics

You use log searches in a variety of places throughout Log Analytics to retrieve data from the workspace.  For actually creating and editing queries in addition to working interactively with returned data though, you have two options that are described below.  

## Log search 
The Log Search page is accessible from the Azure portal.  It's suitable for creating basic queries that can be created on a single line.  Log Search can be used without launching an external portal, and you can use it to perform a variety of functions with log searches including creating alert rules, creating computer groups, and exporting the results of the query.  

Log Search provides multiple features for editing the query without having a full knowledge of the query language.  You can get a summary of these features in [Create log searches in Azure Log Analytics using Log Search](log-analytics-log-search-log-search-portal.md).


![Log Search page](media/log-analytics-log-search-portals/log-search-portal.png)

## Advanced Analytics portal
The Advanced Analytics portal is a dedicated portal that provides advanced functionality not available in Log Search from the Azure portal.  Features include the ability to edit a query on multiple lines, selectively execute code, context sensitive Intellisense, and Smart Analytics.  The Advanced Analytics portal is most suitable for designing complex queries that are either saved as a log search or copied and pasted into other Log Analytics elements.  You launch the Advanced Analytics portal from a link on the Log Search page.

![Advanced Analytics portal](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Because of its advanced features, you'll usually use the Advanced Analytics portal as your primary tool for creating and editing queries.  Once you've determined that the query works as expected, then you'll copy and paste it elsewhere such as Log Search page or View Designer.  

### Firewall requirements
Your browser requires access to the following addresses to access the Advanced Analytics portal.  If your browser is accessing the Azure portal through a firewall, you must enable access to these addresses.

| Uri | IP | Ports |
|:---|:---|:---|
| portal.loganalytics.io | Dynamic | 80,443 |
| api.loganalytics.io    | Dynamic | 80,443 |
| docs.loganalytics.io   | Dynamic | 80,443 |


## Next steps

- Walk through a tutorial on using [Log Search](log-analytics-tutorial-viewdata.md) to learn how to create queries using the query language
- Check out the [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) to create sophisticated queries and use as a development environment for your log searches.

