---
title: Enable secure TLS for Azure Storage client | Microsoft Docs
description: Learn how to enable TLS 1.2 in the client of Azure Storage.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft

ms.assetid:
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/25/2018
ms.author: fryu
---

# Enable secure TLS for Azure Storage client

When you need to audit your services in using Azure Storage based on latest compliance and security requirements, SSL 1.0, 2.0, 3.0 and TLS 1.0 are recognized as non-compliant communication protocols.

SSL 1.0, 2.0 and 3.0 have been found to be vulnerable. They have been prohibited by RFC. TLS 1.0 becomes insecure for using insecure Block cipher (DES CBC and RC2 CBC) and Stream cipher (RC4). PCI council also suggested the migration to higher TLS versions. For more details, you can see [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Azure Storage has stopped SSL 3.0 since 2015 and uses TLS 1.2 on public HTTPs endpoints but TLS 1.0 and TLS 1.1 are still supported for backward compatibility.

In order to ensure secure and compliant connection to Azure Storage, you need to enable TLS 1.2 in client side before sending requests to operate Azure Storage service.

## Enable TLS 1.2 in .NET client

For the client to negotiate TLS 1.2, the OS and the .NET Framework version both need to support TLS 1.2. See more details in [Support for TLS 1.2](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

The following sample shows how to enable TLS 1.2 in your .NET client.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## Enable TLS 1.2 in PowerShell client

The following sample shows how to enable TLS 1.2 in your PowerShell client.

```powershell

# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGropuName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzureStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers

```

## Verify TLS 1.2 connection

You can use Fiddler to verify if TLS 1.2 is used actually. Open Fiddler to start capturing client network traffic then execute above sample. Then you can find the TLS version in the connection that the sample makes.

The following screenshot is a sample for the verification.

![screenshot of verifying TLS version in Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## See also

* [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [Enable TLS in Java client](https://www.java.com/en/configure_crypto.html)
