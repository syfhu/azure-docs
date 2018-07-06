---
title: Credentials in Azure Content Moderator | Microsoft Docs
description: Manage Content Moderator credentials to use with the APIs.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
---
# Manage credentials

Your Content Moderator credentials are created in the following locations:

- [The Azure portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [The Content Moderator review tool](http://contentmoderator.cognitive.microsoft.com/)

This article explains where to find them and how they relate to each other.

## The Azure portal

On the Azure portal dashboard, select your Content Moderator account. Under **Resource Management**, select **Keys**. To copy the key, select the icon to the right of the key.

![Content Moderator keys in the Azure portal](images/credentials-azure-portal-keys.PNG)

### How to use your Azure account with the review tool
To use your Azure key with the review APIs, copy the Resource ID listed on the **Properties** screen in the following screenshot, and enter it on the review tool's credentials screen in the **Whitelisted Resource Id(s)** fields as shown in the following **Resource ID** section. 

To use your Azure key for the workflows available within Content Moderator, enter it in the **Ocp-Apim-Subscription-Key** field in the **Workflow Settings** section as shown in the following **Workflows** section.

> [!NOTE]
> Once you replace the two places in the review tool with the key and the Resource ID from your Azure subscription,
> your **Trial Ocp-Apim-Subscription-Key** displayed on the Credentials screen is no longer used, but is always available.
> The trial key limits you to maximum 5,000 transactions per month at 1 request per second (RPS).

![Content Moderator Resource ID in the Azure portal](images/credentials-azure-portal-resourceid.PNG)


## The Review tool

On the Review tool Dashboard, on the **Settings** tab, select **Credentials**.

![Content Moderator credentials in the review tool](images/credentials-trial-resource-workflow.PNG)

The following section examines the preceding image in more detail:


### API

The first part lists your **review API endpoint**, **team ID**, and the **Ocp-Apim-Subscription-Key (Content Moderator trial key)** generated as part of your review team creation. Use them to call all Content Moderator APIs, including the review API.

Also note your region identifier for your API endpoint. For example, **westus** is the region in "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Content Moderator key in the review tool](images/credentials-trialkey.PNG)


### Resource ID

The second part starts out as empty with no resource ID. **To use your Azure subscription key with the review API, navigate to the resource ID screen as shown previously, and copy it into the field shown**. Hit the **'+'** to save your resource ID.

> [!NOTE]
> Your Content Moderator subscription's region should match the review team's region for it to recognize your team and access the team data. For example, in the images on this page, The **West US** region **(4)** contains the Content Moderator Azure subscription and your review team.

![Content Moderator resource ID in the review tool](images/credentials-resourceids.PNG)


### Workflows

The third part shows the information used for running workflows. It starts out showing the auto-generated trial key by default. 

**Update it with your Azure key when you get an Azure subscription**. The other two fields allow using term and image lists in the Screen Text and Evaluate Image operations respectively.

![Content Moderator workflow credentials in the review tool](images/credentials-workflow.PNG)


## Next steps

* Learn how to use the Content Moderator credentials in your [workflows](workflows.md).
