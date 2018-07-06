---
title: Azure Quickstart - Create a Key Vault Portal | Microsoft Docs
description: Quickstart showing how to create an Azure Key Vault using the portal
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager

ms.assetid: 98cf8387-34de-468e-ac8f-5c02c9e83e68
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/10/2018
ms.author: barclayn
#Customer intent:As a security admin who is new to Azure, I want to use Key Vault to securely store keys and passwords in Azure
---
# Quickstart: Create a Key Vault using the Azure portal

Azure Key Vault is a cloud service that works as a secure secrets store. You can securely store keys, passwords, certificates and other secrets. For more information on Key Vault you may review the [Overview](key-vault-overview.md). Azure key vaults may be created and managed through the Azure portal. In this quickstart, you create a key vault. Once that you have completed that, you will store a secret.

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Log into Azure

Log in to the Azure portal at http://portal.azure.com.

## Create a vault

1. Select the **Create a resource** option on the upper left-hand corner of the Azure portal

    ![Output after Key Vault creation completes](./media/quick-create-portal/search-services.png)
2. In the Search box enter **Key Vault**.
3. From the results list, choose **Key Vault**.
4. On the Key Vault section, choose **Create**.
5. On the **Create key vault** section provide the following information:
    - **Name**: For this quickstart we use **Contoso-vault2**. You must provide a unique name in your testing.
    - **Subscription**: Choose the subscription that you will be using for this quickstart.
    - Under **Resource Group** choose **Create new** and enter a resource group name.
    - In the **Location** pull-down menu choose a location.
    - Check the **Pin to dashboard** checkbox.
    - Leave the other options to their defaults.
6. After providing the information above, select **Create**.

Take note of the two properties listed below:

* **Vault Name**: In the example, this is **Contoso-Vault2**. You will use this name for other steps.
* **Vault URI**: In the example, this is https://contoso-vault2.vault.azure.net/. Applications that use your vault through its REST API must use this URI.

At this point, your Azure account is the only one authorized to perform any operations on this new vault.

![Output after Key Vault creation completes](./media/quick-create-portal/vault-properties.png)

## Add a secret to Key Vault

To add a secret to the vault, you just need to take a couple of additional steps. In this case we add a password that could be used by an application. The password is called **ExamplePassword** and we store the value of **Pa$$w0rd** in it.

1. On the Key Vault properties page select **Secrets**.
2. Click on **Generate/Import**.
3. On the **Create a secret** screen choose the following:
    - **Upload options**: Manual.
    - **Name**: ExamplePassword.
    - **Value**: Pa$$w0rd.
    - Leave the other values to their defaults. Click **Create**.

Once that you receive the message that the secret has been successfully created you may click on it on the list. You can then see some of the properties. If you click on the current version you can see the value you specified in the previous step.

![Secret properties](./media/quick-create-portal/version.png)

## Clean up resources

Other Key Vault quickstarts and tutorials build upon this quickstart. If you plan to continue on to work with subsequent quickstarts and tutorials, you may wish to leave these resources in place.
When no longer needed, delete the resource group and this deletes the Key Vault and related resources. To delete the resource group through the portal:

1. Enter the name of your resource group in the Search box at the top of the portal. When you see the resource group used in this QuickStart in the search results, select it.
2. Select **Delete resource group**.
3. In the **TYPE THE RESOURCE GROUP NAME:** box type in the name of the resource group and select **Delete**.


## Next steps

In this quickstart, you have created a Key Vault and stored a secret. To learn more about Key Vault and how you can use it with your applications continue to the tutorial for web applications working with Key Vault.

> [!div class="nextstepaction"]
> To learn how to read a secret from Key Vault from a web application using managed service identities, continue with the following tutorial [Configure an Azure web application to read a secret from Key vault](tutorial-web-application-keyvault.md).
