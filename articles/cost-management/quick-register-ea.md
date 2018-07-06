---

title: Register your Azure Enterprise Agreement with Azure Cost Management | Microsoft Docs
description: Use your Enterprise Agreement to register with Azure Cost Management.
services: cost-management
keywords:
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: quickstart
ms.custom:
ms.service: cost-management
manager: dougeby
---


# Register an Azure Enterprise Agreement and view cost data

You use your Azure Enterprise Agreement to register with Azure Cost Management. Your registration provides access to the Cloudyn portal. This quickstart details the registration process needed to create a Cloudyn trial subscription and sign in to the Cloudyn portal. It also shows you how to start viewing cost data right away.

## Sign in to Azure

- Sign in to the Azure portal at http://portal.azure.com.

## Register with Azure Cost Management

1. In the Azure portal, click **Cost Management + Billing** in the list of services.
2. Under **Overview**, click **Cost Management**  
    ![Cost Management page](./media/quick-register-ea/cost-mgt-billing-service.png)
3. On the **Cost Management** page, **Go to Cost Management** to open the Cloudyn registration page in a new window.
4. On the Cloudyn portal trial registration page, type your company name and then select **Azure Enterprise Enrollment Administrator**.  
    ![trial registration](./media/quick-register-ea/trial-reg.png)
5. Enter your Enterprise Portal enrollment API key. If you don't have your key handy, click the [Enterprise Portal](https://ea.azure.com) link and do the following steps:
  1. Sign in to the Azure Enterprise website and click **Reports**, click **API Access Key** and then copy your primary key.  
    ![EA API key](./media/quick-register-ea/ea-key.png)
  3. Go back to the registration page and paste in your API key.
6. Agree to the Terms of Use then validate your key. Click **Next** to authorize Cloudyn to collect Azure resource data. Data collected includes usage, performance, billing, and tag data from your subscriptions.  
    ![key validation](./media/quick-register-ea/ea-key-validated.png)
7. Under **Invite other stakeholders**, you can add users by typing their email addresses. When complete, click **Next**. Depending on the size of your Azure enrollment, it can take up to 24 hours for all of your billing data to get added to Cloudyn.
8. Click **Go to Cloudyn** to open the Cloudyn portal and then on the **Cloud Accounts Management** page, you should see your registered EA account information.

To watch a tutorial video about registering your Enterprise Agreement, see [How to Find Your EA Enrollment ID and API Key for use in Azure Cost Management](https://youtu.be/u_phLs_udig).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## Next steps

In this quickstart, you used your Azure Enterprise Agreement information to register with Cost Management. You also signed into the Cloudyn portal and started viewing cost data. To learn more about Azure Cost Management, continue to the tutorial for Cost Management.

> [!div class="nextstepaction"]
> [Review usage and costs](./tutorial-review-usage.md)
