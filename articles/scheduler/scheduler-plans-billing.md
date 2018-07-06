---
title: Plans and Billing in Azure Scheduler
description: Plans and Billing in Azure Scheduler
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''

ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli

---
# Plans and Billing in Azure Scheduler
## Job Collection Plans
Job collections are the billable entity in Azure Scheduler. Job collections contain a number of jobs and come in three plans – Standard, P10 Premium, and P20 Premium – that are described below.

| **Job Collection Plan** | **Max # of Jobs per Job Collection** | **Max Recurrence** | **Max Job Collections per Subscription** | **Limits** |
|:--- |:--- |:--- |:--- |:--- |
| **Standard** |50 jobs per job collection |Once per minute. Cannot execute jobs more often than once a minute |A subscription is allowed up to 100 standard job collections |Access to full feature set of Scheduler |
| **P10 Premium** |50 jobs per job collection |Once per minute. Cannot execute jobs more often than once a minute |A subscription is allowed up to 10,000 P10 Premium job collections. <a href="mailto:wapteams@microsoft.com">Contact us</a> for more. |Access to full feature set of Scheduler |
| **P20 Premium** |1000 jobs per job collection |Once per minute. Cannot execute jobs more often than once a minute |A subscription is allowed up to 10,000 P20 Premium job collections. <a href="mailto:wapteams@microsoft.com">Contact us</a> for more. |Access to full feature set of Scheduler |

## Upgrades and Downgrades of Job Collection Plans
You may upgrade or downgrade a job collection plan anytime among the Standard, P10 Premium, and P20 Premium plans.

## Billing and Azure Plans
If you have more than 100 standard job collections (10 standard billing units), then it's a better deal to have all job collections in the premium plan.

If you have one standard job collection and one premium job collection, you are billed one standard billing unit *and* one premium billing unit. The Scheduler service bills based on the number of active job collections that are set to either standard or premium; this is explained further in the next two sections.

## Standard Billable Units
A standard billable unit can include up to 10 standard job collections. Since a standard job collection can have up to 50 jobs per job collection, one standard billing unit allows a subscription to have up to 500 jobs – up to almost 22 million job executions per month.

If you have between 1 and 10 standard job collections, you'll be billed for 1 standard billing unit. If you have between 11 and 20 standard job collections, you'll be billed for 2 standard billing units. If you have between 21 and 30 standard job collections, you'll be billed for 3 standard billing units, and so on.

## P10 Premium Billable Units
A P10 premium billable unit can include up to 10,000 P10 premium job collections. Since a P10 premium job collection can have up to 50 jobs per job collection, one premium billing unit allows a subscription to have up to 500,000 jobs – up to almost 22 billion job executions per month.

If you have between 1 and 10,000 premium job collections, you'll be billed for 1 P10 premium billing unit. If you have between 10,001 and 20,000 premium job collections, you'll be billed for 2 P10 premium billing units, and so on.

Thus, P10 premium job collections have the same functionality as the standard job collections but provide a price break in case your application requires a lot of job collections.

## P20 Premium Billable Units
A P20 premium billable unit can include up to 5,000 P20 premium job collections. Since a P20 premium job collection can have up to 1,000 jobs per job collection, one premium billing unit allows a subscription to have up to 5,000,000 jobs – up to almost 220 billion job executions per month.

P20 premium job collections provides the same capabilities as P10 premium job collections but also supports a greater number jobs per job collection and a greater total number of jobs overall than P10 premium allowing you to have more scalability.

## Billing and Active Status
Job collections are always active unless your entire subscription has gone into some temporary disabled state due to billing issues. The only way to ensure that a job collection is not billed is to either set it to the *Free* plan or to delete the job collection.

Although you may disable all jobs within a job collection in a single operation, it does not change the billing status of the job collection – the job collection will *still* be billed. Similarly, empty job collections are considered active and will be billed.

## Pricing
For pricing details, please see [Scheduler Pricing](https://azure.microsoft.com/pricing/details/scheduler/).

## See Also
 [What is Scheduler?](scheduler-intro.md)

 [Azure Scheduler concepts, terminology, and entity hierarchy](scheduler-concepts-terms.md)

 [Get started using Scheduler in the Azure portal](scheduler-get-started-portal.md)

 [Azure Scheduler REST API reference](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlets reference](scheduler-powershell-reference.md)

 [Azure Scheduler high-availability and reliability](scheduler-high-availability-reliability.md)

 [Azure Scheduler limits, defaults, and error codes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler outbound authentication](scheduler-outbound-authentication.md)

