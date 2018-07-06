---
title: Configure Deployment Sources for App Services on Azure Stack | Microsoft Docs
description: How a Service Administrator can configure deployment sources (Git, GitHub, BitBucket, DropBox and OneDrive) for App Service on Azure Stack
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''

ms.assetid:
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: brenduns
ms.reviewer: anwestg

---

# Configure deployment sources
*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*


App Service on Azure Stack supports on-demand deployment from multiple Source Control Providers. This feature lets application developers deploy direct from their source control repositories. If users want to configure App Service to connect to their repositories, a cloud operator must first configure the integration between App Service on Azure Stack and the Source Control Provider.  

In addition to local Git, the following Source Control Providers are supported:

* GitHub
* BitBucket
* OneDrive
* DropBox

## View deployment sources in App Service administration

1. Sign in to the Azure Stack Admin Portal (https://adminportal.local.azurestack.external) as the service administrator.
2. Browse to **Resource Providers** and select the **App Service Resource Provider Admin**.
    ![App Service Resource Provider Admin][1]
3. Click **Source control configuration**.  Here you see the list of all Deployment Sources configured.
    ![App Service Resource Provider Admin Source Control Configuration][2]

## Configure GitHub

You must have a GitHub account to complete this task. You might want to use an account for your organization rather than a personal account.

1. Sign in to GitHub, browse to https://www.github.com/settings/developers and click **Register a new application**.
    ![GitHub - Register a new application][3]
2. Enter an **Application name** for example - App Service on Azure Stack.
3. Enter the **Homepage URL**. The Homepage URL must be the Azure Stack Portal address. For example, https://portal.local.azurestack.external.
4. Enter an **Application Description**.
5. Enter the **Authorization callback URL**.  In a default Azure Stack deployment, the Url is in the form https://portal.local.azurestack.external/TokenAuthorize, if you are running under a different domain substitute your domain for local.azurestack.external
6. Click **Register application**.  You will now be presented with a page listing the **Client ID** and **Client Secret** for the application.
    ![GitHub - Completed application registration][5]
7.  In a new browser tab or window Sign in to the Azure Stack Admin Portal (https://adminportal.local.azurestack.external) as the service administrator.
8.  Browse to **Resource Providers** and select the **App Service Resource Provider Admin**.
9. Click **Source control configuration**.
10. Copy and paste the **Client ID** and **Client Secret** into the corresponding input boxes for GitHub.
11. Click **Save**.

## Configure BitBucket

You must have a BitBucket account to complete this task. You might want to use an account for your organization rather than a personal account.

1. Sign in to BitBucket and browse to **Integrations** under your account.
    ![BitBucket Dashboard - Integrations][7]
2. Click **OAuth** under Access Management and **Add consumer**.
    ![BitBucket Add OAuth Consumer][8]
3. Enter a **Name** for the consumer, for example App Service on Azure Stack.
4. Enter a **Description** for the application.
5. Enter the **Callback URL**.  In a default Azure Stack deployment, the Callback Url is in the form https://portal.local.azurestack.external/TokenAuthorize, if you are running under a different domain substitute your domain for azurestack.local.  The Url must follow the capitalization as listed here for BitBucket integration to succeed.
6. Enter the **URL** - this Url should be the Azure Stack Portal URL, for example https://portal.local.azurestack.external.
7. Select the **Permissions** required:
    - **Repositories**: *Read*
    - **Webhooks**: *Read and write*
8. Click **Save**.  You will now see this new application, along with the **Key** and **Secret** under **OAuth consumers**.
    ![BitBucket Application Listing][9]
9.  In a new browser tab or window Sign in to the Azure Stack Admin Portal (https://adminportal.local.azurestack.external) as the service administrator.
10.  Browse to **Resource Providers** and select the **App Service Resource Provider Admin**.
11. Click **Source control configuration**.
12. Copy and paste the **Key** into the **Client ID** input box and **Secret** into the **Client Secret** input box for BitBucket.
13. Click **Save**.


## Configure OneDrive

You must have a Microsoft Account linked to a OneDrive account to complete this task.  You might want to use an account for your organization rather than a personal account.

> [!NOTE]
> OneDrive for Business Accounts are not currently supported.

1. Browse to https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm and Sign in using your Microsoft Account.
2. Under **My applications**, click **Add an app**.
![OneDrive Applications][10]
3. Enter a **Name** for the New Application Registration, enter **App Service on Azure Stack**, and click **Create Application**
4. The next screen lists the properties of your new application. Record the **Application ID**.
![OneDrive Application Properties][11]
5. Under **Application Secrets**, click **Generate New Password**. Make a note of **New password generated**. This is your application secret and is not retrievable after you click **OK** at this stage.
6. Under **Platforms** click **Add Platform** and select **Web**.
7. Enter the **Redirect URI**.  In a default Azure Stack deployment, the Redirect URI is in the form https://portal.local.azurestack.external/TokenAuthorize, if you are running under a different domain substitute your domain for azurestack.local
![OneDrive Application - Add Web Platform][12]
8. Add the **Microsoft Graph Permissions** - **Delegated Permissions**
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive Application - Graph Permissions][13]
9. Click **Save**.
10.  In a new browser tab or window Sign in to the Azure Stack Admin Portal (https://adminportal.local.azurestack.external) as the service administrator.
11.  Browse to **Resource Providers** and select the **App Service Resource Provider Admin**.
12. Click **Source control configuration**.
13. Copy and paste the **Application ID** into the **Client ID** input box and **Password** into the **Client Secret** input box for OneDrive.
14. Click **Save**.

## Configure DropBox

> [!NOTE]
> You need to have a DropBox account to complete this task.  You may wish to use an account for your organization rather than a personal account.

1. Browse to https://www.dropbox.com/developers/apps and Sign in using your DropBox Account.
2. Click **Create app**.

    ![Dropbox applications][14]

3. Select **DropBox API**.
4. Set the access level to **App Folder**.
5. Enter a **Name** for your application.
![Dropbox application registration][15]
6. Click **Create App**.  You will now be presented with a page listing the settings for the App including **App key** and **App secret**.
7. Check the **App folder name** is set to **App Service on Azure Stack**.
8. Set the **OAuth 2 Redirect URI** and click **Add**.  In a default Azure Stack deployment, the Redirect URI is in the form https://portal.local.azurestack.external/TokenAuthorize, if you are running under a different domain substitute your domain for azurestack.local.
![Dropbox application configuration][16]
9.  In a new browser tab or window Sign in to the Azure Stack Admin Portal (https://adminportal.local.azurestack.external) as the service administrator.
10.  Browse to **Resource Providers** and select the **App Service Resource Provider Admin**.
11. Click **Source control configuration**.
12. Copy and paste the **Application Key** into the **Client ID** input box and **App secret** into the **Client Secret** input box for DropBox.
13. Click **Save**.


<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png

## Next steps

Users can now use the deployment sources for things like [continuous deployment](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment), [local Git deployment](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-local-git), and [cloud folder synchronization](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-content-sync).
