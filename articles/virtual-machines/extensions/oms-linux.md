---
title: Azure Log Analytics virtual machine extension for Linux | Microsoft Docs
description: Deploy the Log Analytics agent on Linux virtual machine using a virtual machine extension.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager

ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/21/2018
ms.author: danis

---
# Log Analytics virtual machine extension for Linux

## Overview

Log Analytics provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets. The Log Analytics Agent virtual machine extension for Linux is published and supported by Microsoft. The extension installs the Log Analytics agent on Azure virtual machines, and enrolls virtual machines into an existing Log Analytics workspace. This document details the supported platforms, configurations, and deployment options for the Log Analytics virtual machine extension for Linux.

## Prerequisites

### Operating system

The Log Analytics Agent extension can be run against these Linux distributions.

| Distribution | Version |
|---|---|
| CentOS Linux | 5, 6, and 7 (x86/x64) |
| Oracle Linux | 5, 6, and 7 (x86/x64) |
| Red Hat Enterprise Linux Server | 5, 6 and 7 (x86/x64) |
| Debian GNU/Linux | 6, 7, 8, and 9 (x86/x64) |
| Ubuntu | 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64) |
| SUSE Linux Enterprise Server | 11 and 12 (x86/x64) |

### Agent and VM Extension version
The following table provides a mapping of the version of the Log Analytics VM extension and Log Analytics Agent bundle for each release. A link to the release notes for the Log Analytics agent bundle version is included. Release notes include details on bug fixes and new features available for a given agent release.  

| Log Analytics Linux VM extension version | Log Analytics Agent bundle version | 
|--------------------------------|--------------------------|
| 1.6.42.0 | [1.6.0-42](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
| 1.4.60.2 | [1.4.4-210](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.4-210)| 
| 1.4.59.1 | [1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14.2-125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2-124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1-123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|
| 1.4.45.2 | [1.4.0-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.0-45)|
| 1.3.127.5 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)| 
| 1.3.127.7 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)|
| 1.3.18.7 | [1.3.4-15](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201704-v1.3.4-15)|  

### Azure Security Center

Azure Security Center automatically provisions the Log Analytics agent and connects it to a default Log Analytics workspace created by ASC in your Azure subscription. If you are using Azure Security Center, do not run through the steps in this document. Doing so overwrites the configured workspace and breaks the connection with Azure Security Center.

### Internet connectivity

The Log Analytics Agent extension for Linux requires that the target virtual machine is connected to the internet. 

## Extension schema

The following JSON shows the schema for the Log Analytics Agent extension. The extension requires the workspace ID and workspace key from the target Log Analytics workspace; these values can be [found in your Log Analytics workspace](../../log-analytics/log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key) in the Azure portal. Because the workspace key should be treated as sensitive data, it should be stored in a protected setting configuration. Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine. Note that **workspaceId** and **workspaceKey** are case-sensitive.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### Property values

| Name | Value / Example |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.6 |
| workspaceId (e.g) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (e.g) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## Template deployment

Azure VM extensions can be deployed with Azure Resource Manager templates. Templates are ideal when deploying one or more virtual machines that require post deployment configuration such as onboarding to Log Analytics. A sample Resource Manager template that includes the Log Analytics Agent VM extension can be found on the [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

The JSON configuration for a virtual machine extension can be nested inside the virtual machine resource, or placed at the root or top level of a Resource Manager JSON template. The placement of the JSON configuration affects the value of the resource name and type. For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources). 

The following example assumes the VM extension is nested inside the virtual machine resource. When nesting the extension resource, the JSON is placed in the `"resources": []` object of the virtual machine.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

When placing the extension JSON at the root of the template, the resource name includes a reference to the parent virtual machine, and the type reflects the nested configuration.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## Azure CLI deployment

The Azure CLI can be used to deploy the Log Analytics Agent VM extension to an existing virtual machine. Replace the *workspaceId* and *workspaceKey* with those from your Log Analytics workspace. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.6 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## Troubleshoot and support

### Troubleshoot

Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure CLI. To see the deployment state of extensions for a given VM, run the following command using the Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extension execution output is logged to the following file:

```
/opt/microsoft/omsagent/bin/stdout
```

### Error codes and their meanings

| Error Code | Meaning | Possible Action |
| :---: | --- | --- |
| 9 | Enable called prematurely | [Update the Azure Linux Agent](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) to the latest available version. |
| 10 | VM is already connected to a Log Analytics workspace | To connect the VM to the workspace specified in the extension schema, set stopOnMultipleConnections to false in public settings or remove this property. This VM gets billed once for each workspace it is connected to. |
| 11 | Invalid config provided to the extension | Follow the preceding examples to set all property values necessary for deployment. |
| 12 | The dpkg package manager is locked | Make sure all dpkg update operations on the machine have finished and retry. |
| 17 | OMS package installation failure | 
| 19 | OMI package installation failure | 
| 20 | SCX package installation failure |
| 51 | This extension is not supported on the VM's operation system | |
| 55 | Cannot connect to the Microsoft Operations Management Suite service | Check that the system either has Internet access, or that a valid HTTP proxy has been provided. Additionally, check the correctness of the workspace ID. |

Additional troubleshooting information can be found on the [OMS-Agent-for-Linux Troubleshooting Guide](../../log-analytics/log-analytics-azure-vmext-troubleshoot.md).

### Support

If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/). Alternatively, you can file an Azure support incident. Go to the [Azure support site](https://azure.microsoft.com/support/options/) and select Get support. For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/support/faq/).
