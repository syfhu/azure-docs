﻿---
title: Integrate logs from Azure Key Vault by using Event Hubs | Microsoft Docs
description: Tutorial that provides the necessary steps to make Key Vault logs available to a SIEM by using Azure Log Integration
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder

ms.assetid:
ms.service: security
ms.topic: article
ms.date: 06/07/2018
ms.author: Barclayn
ms.custom: AzLog
---
# Azure Log Integration tutorial: Process Azure Key Vault events by using Event Hubs

>[!IMPORTANT]
> The Azure Log integration feature will be deprecated by 06/01/2019. AzLog downloads will be disabled by Jun 27, 2018. For guidance on what to do moving forward review the post [Use Azure monitor to integrate with SIEM tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

You can use Azure Log Integration to retrieve logged events and make them available to your security information and event management (SIEM) system. This tutorial shows an example of how Azure Log Integration can be used to process logs that are acquired through Azure Event Hubs.

The preferred method for integrating Azure logs is by using your SIEM vendor’s Azure Monitor connector and following these [instructions](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). However, if your SIEM vendor doesn’t provide a connector to Azure Monitor, you may be able to use Azure Log Integration as a temporary solution (if your SIEM is supported by Azure Log Integration) until such a connector is available.

 
Use this tutorial to get acquainted with how Azure Log Integration and Event Hubs work together by following the example steps and understanding how each step supports the solution. Then you can take what you’ve learned here to create your own steps to support your company’s unique requirements.

>[!WARNING]
The steps and commands in this tutorial are not intended to be copied and pasted. They're examples only. Do not use the PowerShell commands “as is” in your live environment. You must customize them based on your unique environment.


This tutorial walks you through the process of taking Azure Key Vault activity logged to an event hub and making it available as JSON files to your SIEM system. You can then configure your SIEM system to process the JSON files.

>[!NOTE]
>Most of the steps in this tutorial involve configuring key vaults, storage accounts, and event hubs. The specific Azure Log Integration steps are at the end of this tutorial. Do not perform these steps in a production environment. They are intended for a lab environment only. You must customize the steps before using them in production.

Information provided along the way helps you understand the reasons behind each step. Links to other articles give you more detail on certain topics.

For more information about the services that this tutorial mentions, see: 

- [Azure Key Vault](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure Log Integration](security-azure-log-integration-overview.md)


## Initial setup

Before you can complete the steps in this article, you need the following:

1. An Azure subscription and account on that subscription with administrator rights. If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/).
 
2. A system with access to the internet that meets the requirements for installing Azure Log Integration. The system can be on a cloud service or hosted on-premises.

3. [Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) installed. To install it:

   a. Use Remote Desktop to connect to the system mentioned in step 2.   
   b. Copy the Azure Log Integration installer to the system. You can [download the installation files](https://www.microsoft.com/download/details.aspx?id=53324).   
   c. Start the installer and accept the Microsoft Software License Terms.   
   d. If you will provide telemetry information, leave the check box selected. If you'd rather not send usage information to Microsoft, clear the check box.
   
   For more information about Azure Log Integration and how to install it, see [Azure Log Integration with Azure Diagnostics logging and Windows Event Forwarding](security-azure-log-integration-get-started.md).

4. The latest PowerShell version.
 
   If you have Windows Server 2016 installed, then you have at least PowerShell 5.0. If you're using any other version of Windows Server, you might have an earlier version of PowerShell installed. You can check the version by entering ```get-host``` in a PowerShell window. If you don't have PowerShell 5.0 installed, you can [download it](https://www.microsoft.com/download/details.aspx?id=50395).

   After you have at least PowerShell 5.0, you can proceed to install the latest version:
   
   a. In a PowerShell window, enter the ```Install-Module Azure``` command. Complete the installation steps.    
   b. Enter the ```Install-Module AzureRM``` command. Complete the installation steps.

   For more information, see [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## Create supporting infrastructure elements

1. Open an elevated PowerShell window and go to **C:\Program Files\Microsoft Azure Log Integration**.
2. Import the AzLog cmdlets by running the script LoadAzLogModule.ps1. Enter the `.\LoadAzLogModule.ps1` command. (Notice the “.\” in that command.)
You should see something like this:</br>

   ![Loaded modules list](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Enter the `Connect-AzureRmAccount` command. In the login window, enter the credential information for the subscription that you will use for this tutorial.

   >[!NOTE]
   >If this is the first time that you're logging in to Azure from this machine, you will see a message about allowing Microsoft to collect PowerShell usage data. We recommend that you enable this data collection because it will be used to improve Azure PowerShell.

4. After successful authentication, you're logged in and you see the information in the following screenshot. Take note of the subscription ID and subscription name, because you'll need them to complete later steps.

   ![PowerShell window](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Create variables to store values that will be used later. Enter each of the following PowerShell lines. You might need to adjust the values to match your environment.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Your subscription name might be different. You can see it as part of the output of the previous command.)
    - ```$location = 'West US'``` (This variable will be used to pass the location where resources should be created. You can change this variable to be any location of your choosing.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random``` (The name can be anything, but it should include only lowercase letters and numbers.)
    - ``` $storageName = $name``` (This variable will be used for the storage account name.)
    - ```$rgname = $name ``` (This variable will be used for the resource group name.)
    - ``` $eventHubNameSpaceName = $name``` (This is the name of the event hub namespace.)
6. Specify the subscription that you will be working with:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Create a resource group:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   If you enter `$rg` at this point, you should see output similar to this screenshot:

   ![Output after creation of a resource group](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Create a storage account that will be used to keep track of state information:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Create the event hub namespace. This is required to create an event hub.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. Get the rule ID that will be used with the insights provider:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Get all possible Azure locations and add the names to a variable that can be used in a later step:
    
    a. ```$locationObjects = Get-AzureRMLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    If you enter `$locations` at this point, you see the location names without the additional information returned by Get-AzureRmLocation.
12. Create an Azure Resource Manager log profile: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    For more information about the Azure log profile, see [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

> [!NOTE]
> You might get an error message when you try to create a log profile. You can then review the documentation for Get-AzureRmLogProfile and Remove-AzureRmLogProfile. If you run Get-AzureRmLogProfile, you see information about the log profile. You can delete the existing log profile by entering the ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` command.
>
>![Resource Manager profile error](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## Create a key vault

1. Create the key vault:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Configure logging for the key vault:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## Generate log activity

Requests need to be sent to Key Vault to generate log activity. Actions like key generation, storing secrets, or reading secrets from Key Vault will create log entries.

1. Display the current storage keys:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Generate a new **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. Display the keys again and see that **key2** holds a different value:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Set and read a secret to generate additional log entries:
    
   a. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)```
   b. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Returned secret](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## Configure Azure Log Integration

Now that you have configured all the required elements to have Key Vault logging to an event hub, you need to configure Azure Log Integration:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Run the AzLog command for each event hub:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

After a minute or so of running the last two commands, you should see JSON files being generated. You can confirm that by monitoring the directory **C:\users\AzLog\EventHubJson**.

## Next steps

- [Azure Log Integration FAQ](security-azure-log-integration-faq.md)
- [Get started with Azure Log Integration](security-azure-log-integration-get-started.md)
- [Integrate logs from Azure resources into your SIEM systems](security-azure-log-integration-overview.md)
