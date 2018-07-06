---
title: Create a Video Indexer account connected to Azure | Microsoft Docs
description: This article shows how to create a Video Indexer account connected to Azure.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre

ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako

---
# Create a Video Indexer account connected to Azure

When using a Video Indexer free trial account, you're limited by the quota and number of videos you can index. This article shows how to create a Video Indexer account that's linked to an Azure subscription, which frees you from these limits and uses pay-as-you-go pricing.

## Prerequisites

* An Azure subscription. 

    If you don't have an Azure subscription yet, sign up for [Azure Free Trial](https://azure.microsoft.com/free/).

* An Azure Active Directory (AD) domain. 

    If you don't have an Azure AD domain, create this domain with your Azure subscription.

* A user and member in your Azure AD domain. You'll use this member when connecting your Video Indexer account to Azure.

    This user should meet these criteria:

    * Be an Azure AD user with a work or school account, not a personal account, such as outlook.com, live.com, or hotmail.com.
        
        ![all AAD users](./media/create-account/all-aad-users.png)

    *  Be a member in your Azure subscription with either an Owner role, or both Contributor and User Access Administrator roles.

        ![access control](./media/create-account/access-control-iam.png)

## Connect to Azure

1. Sign in with that user and click on the **Connect to Azure** button:

    ![connect to Azure](./media/create-account/connect-to-azure.png)

2. When the subscriptions list appears, select the subscription you want to use. 

    ![connect Video Indexer to Azure](./media/create-account/connect-vi-to-azure-subscription.png)

3. Select an Azure region from the supported locations: West US 2, North Europe, or East Asia.
4. Under **Azure Media Services account**, choose one of these options:

    * To create a new Media Services account, select **Create new resource group**. Provide a name for your resource group.

        Azure will create your new account in your subscription, including a new Azure Storage account. Your new Media Services account has a default initial configuration with a Streaming Endpoint and 10 S3 Reserved Units.
    * To use an existing Media Services account, select **Use existing resource**. From the accounts list, select your account.

        Your Media Services account must have the same region as your Video Indexer account. To minimize indexing duration and low throughput, adjust the type and number of Reserved Units to **10 S3 Reserved Units** in your Media Services account.
    * To manually configure your connection, click the **Switch to manual configuration** link, and provide the necessary information:

    ![connect Video Indexer to Azure](./media/create-account/connect-vi-to-azure-subscription-2.png)

5. When you're done, choose **Connect**. This operation might take up to a few minutes. 

    After you're connected to Azure, your new Video Indexer account appears in the account list:

    ![new account](./media/create-account/new-account.png)

6. Browse to your new account: 

    ![Video Indexer account](./media/create-account/vi-account.png)

## Considerations

The following Azure Media Services related considerations apply:

* If you connected to a new Media Services account, you will see a new Resource Group, Media Services account, and a Storage account in your Azure subscription.
* If you connected to a new Media Services account, Video Indexer will set the media **Reserved Units** to 10 S3 units:

    ![Media Services reserved units](./media/create-account/ams-reserved-units.png)

* If you connected to an existing Media Services account, Video Indexer does not change the existing media **Reserved Units** configuration.

    You might need to adjust the type and number of media **Reserved Units**, according to your planned load. Keep in mind that if your load is high and you don’t have enough units or speed, videos processing can result in timeout failures.

* If you connected to a new Media Services account, Video Indexer automatically starts a **Streaming Endpoint** in it:

    ![Media Services streaming endpoint](./media/create-account/ams-streaming-endpoint.png)

* If you connected to an existing Media Services account, Video Indexer does not change the streaming endpoints configuration. If there is no running **Streaming Endpoint**, you will not be able to watch videos from this Media Services account or in Video Indexer.

## Use Video Indexer APIs v2

You can programmatically interact with your trial account and/or with your Video Indexer accounts that are connected to azure by following the instructions in: [Use APIs](video-indexer-use-apis.md).

You should use the same Azure AD user you used when connecting to Azure.

## Next steps

[Examine details of the output JSON](video-indexer-output-json-v2.md).

