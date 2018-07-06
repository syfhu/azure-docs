---
title: What is Azure DNS?
description: Overview of DNS hosting service on Microsoft Azure. Host your domain on Microsoft Azure.
author: KumudD
manager: jeconnoc

ms.service: dns
ms.topic: overview
ms.date: 6/7/2018
ms.author: kumud
#As an administrator, I want to evaluate Azure DNS so I can determine if I want to use it instead of my current DNS service.
---

# What is Azure DNS?

Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure. By hosting your domains in Azure, you can manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services.

You can't use Azure DNS to buy a domain name. For an annual fee,  you can buy a domain name using [Azure Web Apps](https://docs.microsoft.com/en-us/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) or a third-party domain name registrar. Your domains can then be hosted in Azure DNS for record management. See [Delegate a Domain to Azure DNS](dns-domain-delegation.md) for details.

The following features are included with Azure DNS:

## Reliability and performance

DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers. Azure DNS uses anycast networking so that each DNS query is answered by the closest available DNS server. This provides both fast performance and high availability for your domain.

## Security

The Azure DNS service is based on Azure Resource Manager. So, you get Resource Manager features such as:

* [role-based access control](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#access-control) - to control who has access to specific actions for your organization.

* [activity logs](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#activity-logs) - to monitor how a user in your organization modified a resource or to find an error when troubleshooting.

* [resource locking](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-lock-resources) - to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.

For more information, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md). 


## Ease of use

The Azure DNS service can manage DNS records for your Azure services, and can provide DNS for your external resources as well. Azure DNS is integrated in the Azure portal and uses the same credentials, support contract, and billing as your other Azure services. 

DNS billing is based on the number of DNS zones hosted in Azure and by the number of DNS queries. To learn more about pricing, see [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).

Your domains and records can be managed using the Azure portal, Azure PowerShell cmdlets, and the cross-platform Azure CLI. Applications requiring automated DNS management can integrate with the service using the REST API and SDKs.

## Customizable virtual networks with private domains

Azure DNS also supports private DNS domains, which is now in public preview. This allows you to use your own custom domain names in your private virtual networks rather than the Azure-provided names available today.

For more information, see [Using Azure DNS for private domains](private-dns-overview.md).


## Next steps

* Learn about DNS zones and records: [DNS zones and records overview](dns-zones-records.md).

* Learn how to create a zone in Azure DNS: [Create a DNS zone](./dns-getstarted-create-dnszone-portal.md).

* For frequently asked questions about Azure DNS, see the [Azure DNS FAQ](dns-faq.md).

