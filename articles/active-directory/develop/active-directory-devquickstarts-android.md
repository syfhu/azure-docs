---
title: Azure AD Android getting started | Microsoft Docs
description: How to build an Android application that integrates with Azure AD for sign-in and calls Azure AD protected APIs using OAuth2.0.
services: active-directory
documentationcenter: android
author: CelesteDG
manager: mtillman
editor: ''

ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
---

# Azure AD Android getting started
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

If you're developing an Android application, Microsoft makes it simple and straightforward to sign in Azure Active Directory (Azure AD) users. Azure AD enables your application to access user data through the Microsoft Graph or your own protected web API. 

The Azure AD Authentication Library (ADAL) Android library gives your app the ability to begin using the
[Microsoft Azure Cloud](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) by supporting [Microsoft Azure Active Directory accounts](https://azure.microsoft.com/services/active-directory/) using industry standard OAuth2 and OpenID Connect. This sample demonstrates all the normal lifecycles your application should experience, including:

* Get a token for the Microsoft Graph
* Refresh a token
* Call the Microsoft Graph
* Sign out the user

To get started, you'll need an Azure AD tenant where you can create users and register an application. If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).

## Scenario: Sign in users and call the Microsoft Graph

![Topology](./media/active-directory-android-topology.png)

This app can be used for all Azure AD accounts. It supports both single and multi-organizational scenarios (discussed in steps). It demonstrates how a developer can build apps to connect with enterprise users and access their Azure + O365 data via the Microsoft Graph. During the auth flow, end users will be required to sign in and consent to the permissions of the application, and in some cases may require an admin to consent to the app. The majority of the logic in this sample shows how to auth an end user and make a basic call to the Microsoft Graph.

## Example code

You can find the full sample code [on Github](https://github.com/Azure-Samples/active-directory-android). 

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this, 
        AUTHORITY, 
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(), 
    RESOURCE_ID, 
    CLIENT_ID, 
    REDIRECT_URI,  
    PromptBehavior.Auto, 
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## Steps to run

### Register and configure your app 
You will need to have a native client application registered with Microsoft using the 
[Azure portal](https://portal.azure.com). 

1. Getting to app registration
    - Navigate to the [Azure portal](https://aad.portal.azure.com). 
    - Select ***Azure Active Directory*** > ***App Registrations***. 

2. Create the app
    - Select **New application registration**. 
    - Enter an app name in the **Name** field. 
    - In **Application type** select **Native**. 
    - In **Redirect URI**, enter `http://localhost`. 

3. Configure Microsoft Graph
    - Select **Settings > Required Permissions**.
    - Select **Add**, inside **Select an API** select ***Microsoft Graph***. 
    - Select the permission **Sign in and read user profile**, then hit **Select** to save. 
        - This permission maps to the `User.Read` scope. 
    - Optional: Inside **Required permissions > Windows Azure Active Directory**, remove the selected permission **Sign in and read user profile**. This will avoid the user consent page listing the permission twice. 

4. Congrats! Your app is successfully configured. In the next section, you'll need:
    - `Application ID`
    - `Redirect URI`

### Get the sample code

1. Clone the code.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Open the sample in Android Studio.
    - Select **Open an existing Android Studio project**.

### Configure your code

You can find all the configuration for this code sample in the ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** file. 

1. Replace the constant `CLIENT_ID` with the `ApplicationID`.
2. Replace the constant `REDIRECT URI` with the `Redirect URI` you configured earlier (`http://localhost`). 

### Run the sample

1. Select **Build > Clean Project**. 
2. Select **Run > Run app**. 
3. The app should build and show some basic UX. When you click the `Call Graph API` button, it will prompt for a sign in, and then silently call the Microsoft Graph API with the new token. 

## Important info

1. Checkout the [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) for more info on the library mechanics and how to configure new scenarios and capabilities. 
2. In Native scenarios, the app will use an embedded Webview and will not leave the app. The `Redirect URI` can be arbitrary. 
3. Find any problems or have requests? You can create an issue or post on Stackoverflow with the tag `azure-active-directory`. 

### Cross-app SSO
Learn [how to enable cross-app SSO on Android by using ADAL](active-directory-sso-android.md). 

### Auth telemetry
the ADAL library exposes auth telemetry to help app developers understand how their apps are behaving and build better experiences. This allows you to capture sign in success, active users, and several other interesting insights. Using auth telemetry does require app developers to establish a telemetry service to aggregate and store events.

To learn more about auth telemetry, checkout [ADAL Android auth telemetry](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
