---
title: Azure Government Compliance | Microsoft Docs
description: Provides and overview of the available compliance services for Azure Government
services: azure-government
cloud: gov
documentationcenter: ''
author: jomolesk
manager: zakramer

ms.assetid: 1d2e0938-482f-4f43-bdf6-0a5da2e9a185
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/09/2018
ms.author: jomolesk

---
# Azure Government compliance

## Azure Security and Compliance Blueprint

Azure Security and Compliance Blueprints are designed to facilitate the secure and compliant use of Azure for government agencies and third-party providers building on behalf of government. Azure Government customers may leverage Azure Government’s FedRAMP JAB Provisional Authority to Operate (P-ATO) or DoD Provisional Authorization (PA), reducing the scope of customer-responsibility security controls in Azure-based systems. Inheriting security control implementations from Azure Government allows customers to focus on control implementations specific to their IaaS, PaaS, or SaaS environments built in Azure.

> [!NOTE]
> Within the context of Azure Security and Compliance Blueprints, "customer" references the organization building directly within Azure. Azure customers may include third-party ISVs building on behalf of government or government agencies building directly in Azure.

## Azure Security and Compliance Blueprint Customer Responsibilities Matrix

The Azure Security and Compliance Blueprint Customer Responsibilities Matrix (CRM) is designed to aid Azure Government customers implementing and documenting system-specific security controls implemented within Azure. The CRM explicitly lists all [NIST SP 800-53](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r4.pdf) security control requirements for FedRAMP and DISA baselines that include a customer implementation requirement. This includes controls with a shared responsibility between Azure and Azure customers, as well as controls that must be fully implemented by Azure customers. Where appropriate, controls are delineated at a control sub-requirement granularity to provide more specific guidance.

The CRM format is designed for utility and is conducive to focused documentation of only the customer portions of implemented security controls.

For example, control AC-1 requires documented access control policies and procedures for the system seeking an ATO. For this control, Microsoft has internal Azure-specific policies and procedures regarding access control mechanisms used to manage the Azure infrastructure and platform. Customers must also create their own access control policies and procedures used within their specific system built in Azure. The CRM documents control parts AC-1a, which requires the policies and procedures to include specific content, as well as AC-1b, which requires customers to review and update these documents on an annual basis.

The CRM is available as Microsoft Excel workbook for the FedRAMP Moderate and High baselines, the DISA Cloud Computing SRG L4 and L5 baselines, and the NIST Cybersecurity Framework (CSF).

The CRM is available for download from the [Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/Blueprint).

## Azure Security and Compliance Blueprint System Security Plan

The Azure Security and Compliance Blueprint System Security Plan (SSP) template is customer-focused and designed for use in developing an SSP that documents both customer security control implementations as well as controls inherited from Azure. Controls which include a customer responsibility, contain guidance on documenting control implementation with a thorough and compliant response. Azure inheritance sections document how security controls are implemented by Azure on behalf of the customer.

The SSP is available for the FedRAMP Moderate and High baselines, and the DISA Cloud Computing SRG L4 and L5 baselines. 

The SSP is available for download from the [Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/Blueprint).

## Azure Security and Compliance Blueprint implementation guidance

Azure Security and Compliance Blueprint implementation guidance is designed to help cloud solution architects and security personnel understand how Azure Government services and features can be deployed to implement a subset of customer-responsibility FedRAMP and DoD security controls. 
An array of documentation, tools, templates, and other resources are available to guide the secure deployment of Azure services and features. Azure resources can be deployed using Azure Resource Manager template [building blocks](https://github.com/mspnp/template-building-blocks), community-contributed Azure [Quickstart Templates](https://azure.microsoft.com/resources/templates/), or through use of [customer-authored](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) JSON-based Resource Manager templates. The architecture of a basic deployment in Azure includes compute, networking, and storage resources. This implementation guidance addresses how these resources can be deployed in ways that help meet security control implementation requirements.

The NIST SP 800-53 implementation guidance is available for download from the [Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/Blueprint).

## General Data Protection Regulation (GDPR) Data Subject Requests (DSR) on Azure Government

Azure government customers can submit DSR starting May 25, 2018 by going to [Azure Portal Help + Support](https://portal.azure.us/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) and submit a **New Support Request** with the following information:

* **Issue Type:** Subscription Management
* **Problem Type:** Security and Compliance Request
* **Category:** Privacy Blade and GDPR requests
* **Description:** *{Include list of all users within the tenant}*

For more information about how Microsoft can help you with the GDPR, see [Get Started: Support for GDPR Accountability](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted) in the Service Trust Portal.

## Next steps

For inquiries related to Azure Security and Compliance Blueprints, FedRAMP, DoD, or Agency ATO processes, or other compliance assistance; or to provide feedback, email [AzureBlueprint@microsoft.com](mailto:AzureBlueprint@microsoft.com).

[Microsoft Trust Center](https://microsoft.com/en-us/trustcenter)

[Microsoft Azure Government Blog](https://blogs.msdn.microsoft.com/azuregov/)
