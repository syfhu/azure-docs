---
title: Azure Advisor Cost recommendations | Microsoft Docs
description: Use Azure Advisor to optimize the cost of your Azure deployments.
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

# Advisor Cost recommendations

Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources. You can get cost recommendations from the **Cost** tab on the Advisor dashboard.

## Optimize virtual machine spend by resizing or shutting down underutilized instances 
Although certain application scenarios can result in low utilization by design, you can often save money by managing the size and number of your virtual machines. Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines. Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.

Advisor shows you the estimated cost of continuing to run your virtual machine, so that you can choose to shut it down or resize it.

If you want to be more aggressive at identifying underutilized virtual machines, you can adjust the average CPU utilization rule on a per subscription basis.

## Reduce costs by eliminating unprovisioned ExpressRoute circuits
Advisor identifies ExpressRoute circuits that have been in the provider status of *Not Provisioned* for more than one month, and recommends deleting the circuit if you aren't planning to provision the circuit with your connectivity provider.

## Buy virtual machine reserved instances to save money over pay-as-you-go costs
Advisor will review your virtual machine usage over the last 30 days and determine if you could save money by purchasing reserved instances. Advisor will show you the regions and sizes where you potentially have the most savings and will show you the estimated savings from purchasing reserved instances. 

With reserved instances, you can pre-purchase the base costs for your virtual machines. Discounts will automatically apply to new or existing VMs that have the same size and region as your reserved instances. [Learn more about Azure Reserved VM Instances.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

## How to access Cost recommendations in Azure Advisor

1. Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).

2.	On the Advisor dashboard, click the **Cost** tab.

## Next steps

To learn more about Advisor recommendations, see:
* [Introduction to Advisor](advisor-overview.md)
* [Get Started](advisor-get-started.md)
* [Advisor Performance recommendations](advisor-cost-recommendations.md)
* [Advisor High Availability recommendations](advisor-cost-recommendations.md)
* [Advisor Security recommendations](advisor-cost-recommendations.md)
