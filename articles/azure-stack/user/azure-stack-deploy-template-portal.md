---
title: Deploy templates using the portal in Azure Stack | Microsoft Docs
description: Learn how to use the Azure Stack portal to deploy templates.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''

ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer:

---

# Deploy templates using the Azure Stack portal

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

You can use the portal to deploy Azure Resource Manager templates to Azure Stack.

## To deploy a template

1. Sign in to the portal, select **New**, and then select **Custom**.
2. Select **Template deployment**.
3. Select **Edit template**, and then paste your JSON template code into the code window. Select **Save**.
4. Select **Edit parameters**, provide values for the parameters that are shown, and then select **OK**.
5. Select **Subscription**. Choose the subscription you want to use, and then select **OK**.
6. Select **Resource group**. Choose an existing resource group or create a new one, and then select **OK**.
7. Select **Create**. A new tile on the dashboard tracks the progress of your template deployment.

## Next steps

* [Deploy templates with PowerShell](azure-stack-deploy-template-powershell.md)
