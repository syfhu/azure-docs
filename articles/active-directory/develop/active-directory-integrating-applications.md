---
title: Integrating Applications with Azure Active Directory 
description: How to add, update, or remove an application in Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''

ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: luleon
---

# Integrating applications with Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Enterprise developers and software-as-a-service (SaaS) providers can develop commercial cloud services or line-of-business applications, that can be integrated with Azure Active Directory (Azure AD) to provide secure sign-in and authorization for their services. To integrate an application or service with Azure AD, a developer must first register the application with Azure AD.

This article shows you how to add, update, or remove an application's registration in Azure AD. You learn about the different types of applications that can be integrated with Azure AD, how to configure your applications to access other resources such as web APIs, and more.

To learn more about the two Azure AD objects that represent a registered application and the relationship between them, see [Application Objects and Service Principal Objects](active-directory-application-objects.md); to learn more about the branding guidelines you should use when developing applications with Azure Active Directory, see [Branding Guidelines for Integrated Apps](active-directory-branding-guidelines.md).

## Adding an application
Any application that wants to use the capabilities of Azure AD must first be registered in an Azure AD tenant. This registration process involves giving Azure AD details about your application, such as the URL where it’s located, the URL to send replies after a user is authenticated, the URI that identifies the app, and so on.

### To register a new application using the Azure portal
1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.
3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, and click **New application registration**.

   ![Register a new application](./media/active-directory-integrating-applications/add-app-registration.png)

4. When the **Create** page appears, enter your application's registration information: 

  - **Name:** Enter a meaningful application name
  - **Application type:** 
    - Select "Native" for [client applications](active-directory-dev-glossary.md#client-application) that are installed locally on a device. This setting is used for OAuth public [native clients](active-directory-dev-glossary.md#native-client).
    - Select "Web app / API" for [client applications](active-directory-dev-glossary.md#client-application) and [resource/API applications](active-directory-dev-glossary.md#resource-server) that are installed on a secure server. This setting is used for OAuth confidential [web clients](active-directory-dev-glossary.md#web-client) and public [user-agent-based clients](active-directory-dev-glossary.md#user-agent-based-client). The same application can also expose both a client and resource/API.
  - **Sign-On URL:** For "Web app / API" applications, provide the base URL of your app. For example, `http://localhost:31544` might be the URL for a web app running on your local machine. Users would use this URL to sign in to a web client application. 
  - **Redirect URI:** For "Native" applications, provide the URI used by Azure AD to return token responses. Enter a value specific to your application, for example `http://MyFirstAADApp`

   ![Register a new application - create](./media/active-directory-integrating-applications/add-app-registration-create.png)

   If you'd like specific examples for web applications or native applications, check out our [quickstarts](active-directory-developers-guide.md#get-started).

5. When finished, click **Create**. Azure AD assigns a unique Application ID to your application, and you're taken to your application's main registration page. Depending on whether your application is a web or native application, different options are provided to add additional capabilities to your application. See the next section for an overview of consent, and details on enabling additional configuration features in your application registration (credentials, permissions, enable sign-in for users from other tenants.)

  > [!NOTE]
  > By default, a newly registered web application is configured to allow **only** users from the same tenant to sign in to your application.
  > 
  > 

## Updating an application
Once your application has been registered with Azure AD, it may need to be updated to provide access to web APIs, be made available in other organizations, and more. This section describes various ways in which you can configure your application further. First we start with an overview of the consent framework, which is important to understand when building applications that need to be used by other users or applications.

### Overview of the consent framework

The Azure AD consent framework makes it easy to develop multi-tenant web and native client applications. These applications allow sign-in by user accounts from an Azure AD tenant, different from the one where the application is registered. They may also need to access web APIs such as the Microsoft Graph API (to access Azure Active Directory, Intune, and services in Office 365) and other Microsoft services' APIs, in addition to your own web APIs. The framework is based on a user or an administrator giving consent to an application that asks to be registered in their directory, which may involve accessing directory data.

For example, if a web client application needs to read calendar information about the user from Office 365, that user is required to consent to the client application first. After consent is given, the client application will be able to call the Microsoft Graph API on behalf of the user, and use the calendar information as needed. The [Microsoft Graph API](https://graph.microsoft.io) provides access to data in Office 365 (like calendars and messages from Exchange, sites and lists from SharePoint, documents from OneDrive, notebooks from OneNote, tasks from Planner, workbooks from Excel, etc.), as well as users and groups from Azure AD and other data objects from more Microsoft cloud services. 

The consent framework is built on OAuth 2.0 and its various flows, such as authorization code grant and client credentials grant, using public or confidential clients. By using OAuth 2.0, Azure AD makes it possible to build many different types of client applications, such as on a phone, tablet, server, or a web application, and gain access to the required resources.

For more information about using the consent framework with OAuth2.0 authorization grants, see [Authorize access to web applications using OAuth 2.0 and Azure AD](active-directory-protocols-oauth-code.md) and [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md). For information about getting authorized access to Office 365 via Microsoft Graph, see [App authentication with Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### Example of the consent experience

The following steps show you how the consent experience works for both the application developer and user.

1. Assume you have a web client application that needs to request specific permissions to access a resource/API. You'll learn how to do this configuration in the next section, but essentially the Azure portal is used to declare permission requests at configuration time. Like other configuration settings, they become part of the application's Azure AD registration:
   
  ![Permissions to other applications](./media/active-directory-integrating-applications/requiredpermissions.png)
    
2. Consider that your application’s permissions have been updated, the application is running, and a user is about to use it for the first time. First the application needs to obtain an authorization code from Azure AD’s `/authorize` endpoint. The authorization code can then be used to acquire a new access and refresh token.

3. If the user is not already authenticated, Azure AD's `/authorize` endpoint prompts for sign-in.
   
  ![User or administrator sign in to Azure AD](./media/active-directory-integrating-applications/usersignin.png)

4. After the user has signed in, Azure AD will determine if the user needs to be shown a consent page. This determination is based on whether the user (or their organization’s administrator) has already granted the application consent. If consent has not already been granted, Azure AD prompts the user for consent and displays the required permissions it needs to function. The set of permissions that are displayed in the consent dialog match the ones selected in the Delegated Permissions in the Azure portal.
   
  ![User consent experience](./media/active-directory-integrating-applications/consent.png)

5. After the user grants consent, an authorization code is returned to your application, which is redeemed to acquire an access token and refresh token. For more information about this flow, see the [web Application to web API section in Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md#web-application-to-web-api).

6. As an administrator, you can also consent to an application's delegated permissions on behalf of all the users in your tenant. Administrative consent prevents the consent dialog from appearing for every user in the tenant, and can be done in the [Azure portal](https://portal.azure.com) by users with the administrator role. From the **Settings** page for your application, click **Required Permissions** and click on the **Grant Permissions** button. 

  ![Grant permissions for explicit admin consent](./media/active-directory-integrating-applications/grantpermissions.png)
    
  > [!NOTE]
  > Granting explicit consent using the **Grant Permissions** button is currently required for single page applications (SPA) that use ADAL.js. Otherwise, the application fails when the access token is requested. 

### Configure a client application to access web APIs
In order for a web/confidential client application to be able to participate in an authorization grant flow that requires authentication (and obtain an access token), it must establish secure credentials. The default authentication method supported by the Azure portal is client ID + secret key. This section covers the configuration steps required to provide the secret key with your client's credentials.

Additionally, before a client can access a web API exposed by a resource application (such as the Microsoft Graph API), the consent framework ensures the client obtains the permission grant required, based on the permissions requested. By default, all applications can choose permissions from "Windows Azure Active Directory" (Graph API) and "Windows Azure Service Management API." The [Graph API “Sign-in and read user profile” permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) is also selected by default. If your client is being registered in a tenant that has accounts subscribed to Office 365, web APIs and permissions for SharePoint and Exchange Online are available for selection. You can select from [two types of permissions](active-directory-dev-glossary.md#permissions) for each desired web API:

- Application Permissions: Your client application needs to access the web API directly as itself (no user context). This type of permission requires administrator consent and is also not available for native client applications.

- Delegated Permissions: Your client application needs to access the web API as the signed-in user, but with access limited by the selected permission. This type of permission can be granted by a user unless the permission requires administrator consent. 

  > [!NOTE]
  > Adding a delegated permission to an application does not automatically grant consent to the users within the tenant. Users must still manually consent for the added delegated permissions at runtime, unless the administrator clicks the **Grant Permissions** button from the **Required Permissions** section of the application page in the Azure portal. 

#### To add application credentials, or permissions to access web APIs
1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.
3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, then find/click the application you want to configure.

   ![Update an application's registration](./media/active-directory-integrating-applications/update-app-registration.png)

4. You are taken to the application's main registration page, which opens up the **Settings** page for the application. To add a secret key for your web application's credentials:
  - Click the **Keys** section on the **Settings** page. 
  - Add a description for your key.
  - Select either a one or two year duration.
  - Click **Save**. The right-most column will contain the key value, after you save the configuration changes. **Be sure to copy the key** for use in your client application code, as it is not accessible once you leave this page.

  ![Update an application's registration - keys](./media/active-directory-integrating-applications/update-app-registration-settings-keys.png)

5. To add permission(s) to access resource APIs from your client
  - Click the **Required Permissions** section on the **Settings** page. 
  - Click the **Add** button.
  - Click **Select an API** to select the type of resources you want to pick from.
  - Browse through the list of available APIs or use the search box to select from the available resource applications in your directory that expose a web API. Click the resource you are interested in, then click **Select**.
  - You're taken to the **Enable Access** page. Select the Application Permissions and/or Delegated Permissions your application needs when accessing the API.
   
  ![Update an application's registration - permissions api](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-api.png)

  ![Update an application's registration - permissions perms](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms.png)

6. When finished, click the **Select** button on **Enable Access** page, then the  **Done** button on the **Add API access** page. You are returned to the **Required permissions** page, where the new resource is added to the list of APIs.

  > [!NOTE]
  > Clicking the **Done** button also automatically sets the permissions for your application in your directory based on the permissions to other applications that you configured. You can view these application permissions by looking at the application **Settings** page.
  > 
  > 

### Configuring a resource application to expose web APIs

You can develop a web API and make it available to client applications by exposing access [scopes](active-directory-dev-glossary.md#scopes) and [roles](active-directory-dev-glossary.md#roles). A correctly configured web API is made available just like the other Microsoft web APIs, including the Graph API and the Office 365 APIs. Access scopes and roles are exposed through your [application's manifest](active-directory-dev-glossary.md#application-manifest), which is a JSON file that represents your application’s identity configuration. 

The following section shows you how to expose access scopes, by modifying the resource application's manifest.

#### Adding access scopes to your resource application

1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.

3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, then find/click the application you want to configure.

   ![Update an application's registration](./media/active-directory-integrating-applications/update-app-registration.png)

4. You are taken to the application's main registration page, which opens up the **Settings** page for the application. Switch to the **Edit manifest** page, by clicking **Manifest** from the application's registration page. A web-based manifest editor opens, allowing you to **Edit** the manifest within the portal. Optionally, you can click **Download** and edit locally, then use **Upload** to reapply it to your application.

5. In this example, we will expose a new scope called `Employees.Read.All` on our resource/API, by adding the following JSON element to the `oauth2Permissions` collection. The existing `user_impersonation` scope is provided by default during registration. `user_impersonation` allows a client application to request permission to access the resource, under the identity of the signed-in user. Be sure to add the comma after the existing `user_impersonation` scope element, and change the property values to suit your resource's needs. 

  ```json
  {
    "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
    "adminConsentDisplayName": "Read-only access to Employee records",
    "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
    "isEnabled": true,
    "type": "User",
    "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
    "userConsentDisplayName": "Read-only access to your Employee records",
    "value": "Employees.Read.All"
  }
  ```
  > [!NOTE]
  > The "id" value must be generated using a GUID generation tool such as [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) or programmatically. It represents a unique identifier for the scope as exposed by the web API. Once a client is appropriately configured with permissions to access your web API, it is issued an OAuth2.0 access token by Azure AD. When the client calls the web API, it presents the access token that has the scope (scp) claim set to the permissions requested in its application registration.
  >
  > You can expose additional scopes later as necessary. Consider that your web API might expose multiple scopes associated with a variety of different functions. Your resource can control access to the web API at runtime, by evaluating the scope (`scp`) claim(s) in the received OAuth 2.0 access token.
  > 

6. When finished, click **Save**. Now your web API is configured for use by other applications in your directory. 

  ![Update an application's registration](./media/active-directory-integrating-applications/update-app-registration-manifest.png)

#### Verify the web API is exposed to other applications in your tenant
1. Go back to your Azure AD tenant, click **App registrations** again, then find/click the client application you want to configure.

   ![Update an application's registration](./media/active-directory-integrating-applications/update-app-registration.png)

2. Repeat step 5 as you did in [Configure a client application to access web APIs](#configure-a-client-application-to-access-web-apis). When you get to the **Select an API** step, search for your resource by entering its application name in the search field, and click **Select**. 

3. On the **Enable Access** page you should see the new scope, available for client permission requests.

  ![New permissions are shown](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms-newscopes.png)

#### More on the application manifest

The application manifest actually serves as a mechanism for updating the Application entity, which defines all attributes of an Azure AD application's identity configuration, including the API access scopes we discussed. For more information on the Application entity and its schema, see the [Graph API Application entity documentation](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). The article contains complete reference information on the Application entity members used to specify permissions for your API, including:  

- The appRoles member, which is a collection of [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entities, used to define [application permissions](active-directory-dev-glossary.md#permissions) for a web API. 
- The oauth2Permissions member, which is a collection of [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entities, used to define [delegated permissions](active-directory-dev-glossary.md#permissions) for a web API.

For more information on application manifest concepts in general, see [Understanding the Azure Active Directory application manifest](active-directory-application-manifest.md).

### Accessing the Azure AD Graph and Office 365 via Microsoft Graph APIs  

As mentioned earlier, in addition to exposing/accessing APIs for your own applications, you can register your client application to access APIs exposed by Microsoft resources. The Microsoft Graph API, referred to as “Microsoft Graph” in the portal's resource/API list, is available to all applications that are registered with Azure AD. If you are registering your client application in a tenant containing accounts that have signed up for an Office 365 subscription, you can also access the scopes exposed by the various Office 365 resources.

For a complete discussion on scopes exposed by Microsoft Graph API, see the [Microsoft Graph permissions reference](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference) article.

> [!NOTE]
> Due to a current limitation, native client applications can only call into the Azure AD Graph API if they use the “Access your organization's directory” permission. This restriction does not apply for web applications.
> 
> 

### Configuring multi-tenant applications

When registering an application in Azure AD, you may want your application to be accessed only by users in your organization. Alternatively, you may also want your application to be accessible by users in external organizations. These two application types are called single-tenant and multi-tenant applications. This section discusses how to modify the configuration of a single-tenant application to make it a multi-tenant application.

It’s important to note the differences between a single-tenant and multi-tenant application:  

- A single-tenant application is intended for use in one organization. It's typically a line-of-business (LoB) application written by an enterprise developer. A single-tenant application can only be accessed by users with accounts in the same tenant as the application registration. As a result, it only needs to be provisioned in one directory.
- A multi-tenant application is intended for use in many organizations. Referred to as a software-as-a-service (SaaS) web application, it's typically written by an independent software vendor (ISV). Multi-tenant applications must be provisioned in each tenant where users need access. For tenants other than the one where the application is registered, user or administrator consent is required in order to register them. Note that native client applications are multi-tenant by default as they are installed on the resource owner's device. See the preceding [Overview of the consent framework](#overview-of-the-consent-framework) section for details on the consent framework.

Making an application multi-tenant requires both application registration changes, as well as changes to the web application itself. The following sections cover both.

#### Changing the application registration to support multi-tenant

If you are writing an application that you want to make available to your customers or partners outside of your organization, you need to update the application definition in the Azure portal.

> [!IMPORTANT]
> Azure AD requires the App ID URI of multi-tenant applications to be globally unique. The App ID URI is one of the ways an application is identified in protocol messages. For a single tenant application, it is sufficient for the App ID URI to be unique within that tenant. For a multi-tenant application, it must be globally unique so Azure AD can find the application across all tenants. 
> Global uniqueness is enforced by requiring the App ID URI to have a host name that matches a verified domain of the Azure AD tenant. For example, if the name of your tenant is contoso.onmicrosoft.com then a valid App ID URI would be https://contoso.onmicrosoft.com/myapp. If your tenant has a verified domain of contoso.com, then a valid App ID URI would also be https://contoso.com/myapp. If the App ID URI doesn’t follow this pattern, setting an application as multi-tenant fails.
> 

To give external users the ability to access your application: 

1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.
3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, then find/click the application you want to configure. You are taken to the application's main registration page, which opens up the **Settings** page for the application.
4. From the **Settings** page, click **Properties** and change the **Multi-tenanted** switch to **Yes**.

After you have made the changes, users and administrators in other organizations are able to grant their users the ability to sign in to your application, allowing your application to access resources secured by their tenant.

#### Changing the application to support multi-tenant

Support for multi-tenant applications relies heavily on the Azure AD consent framework. Consent is the mechanism that allows a user from another tenant to grant the application access to resources secured by the user's tenant. This experience is referred to as "user consent."

Your web application may also offer:

- The ability for administrators to "sign up my company." This experience, referred to as "admin consent", gives an administrator the capability to grant consent on behalf *all users* in their organization. Only a user that authenticates with an account that belongs to the Global Admin role can provide admin consent; others receive an error.

- A sign-up experience for users. It is expected that the user is provided a "sign-up" button that will redirect the browser to the Azure AD OAuth2.0 `/authorize` endpoint or an OpenID Connect `/userinfo` endpoint. These endpoints allow the application to get information about the new user by inspecting the id_token. Following the sign-up phase the user is presented with a consent prompt, similar to the one shown in the [Overview of the consent framework](#overview-of-the-consent-framework) section.

For more information on the application changes required to support multi-tenanted access and sign-in/sign-up experiences, see:

- [How to sign in any Azure Active Directory (AD) user using the multi-tenant application pattern](active-directory-devhowto-multi-tenant-overview.md)
- The list of [Multi-tenant code samples](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant). 
- [Quickstart: Add company branding to your sign-in page in Azure AD](../fundamentals/customize-branding.md)

### Enabling OAuth 2.0 implicit grant for Single Page Applications

Single Page Application’s (SPAs) are typically structured with a JavaScript-heavy front end that runs in the browser, which calls the application’s web API back-end to perform its business logic. For SPAs hosted in Azure AD, you use OAuth 2.0 Implicit Grant to authenticate the user with Azure AD and obtain a token that you can use to secure calls from the application's JavaScript client to its back-end web API. 

After the user has granted consent, this same authentication protocol can be used to obtain tokens to secure calls between the client and other web API resources configured for the application. To learn more about the implicit authorization grant, and help you decide whether it's right for your application scenario, see [Understanding the OAuth2 implicit grant flow in Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md).

By default, OAuth 2.0 implicit Grant is disabled for applications. You can enable OAuth 2.0 Implicit Grant for your application by setting the `oauth2AllowImplicitFlow` value in its [application manifest](active-directory-application-manifest.md).

#### To enable OAuth 2.0 implicit grant

> [!NOTE]
> For details on how to edit the application manifest, be sure to first review the preceding section, [Configuring a resource application to expose web APIs](#configuring-a-resource-application-to-expose-web-apis).
>

1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.
3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, then find/click the application you want to configure. You are taken to the application's main registration page, which opens up the **Settings** page for the application.
4. Switch to the **Edit manifest** page, by clicking **Manifest** from the application's registration page. A web-based manifest editor opens, allowing you to **Edit** the manifest within the portal. Locate and set the "oauth2AllowImplicitFlow" value to "true." By default, it is set to "false."
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Save the updated manifest. Once saved, your web API is now configured to use OAuth 2.0 Implicit Grant to authenticate users.

## Removing an application
This section describes how to remove an application's registration from your Azure AD tenant.

### Removing an application authored by your organization
Applications that your organization has registered appear under the "My apps" filter on your tenant's main "App registrations" page. These applications are ones you registered manually via the Azure portal, or programmatically via PowerShell or the Graph API. More specifically, they are represented by both an Application and Service Principal object in your tenant. For more information, see [Application Objects and Service Principal Objects](active-directory-application-objects.md).

#### To remove a single-tenant application from your directory
1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.
3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, then find/click the application you want to configure. You are taken to the application's main registration page, which opens up the **Settings** page for the application.
4. From the application's main registration page, click **Delete**.
5. Click **Yes** in the confirmation message.

#### To remove a multi-tenant application from its home directory
1. Sign in to the [Azure portal](https://portal.azure.com).
2. If your account gives you access to more than one, click your account in the top right corner, and set your portal session to the desired Azure AD tenant.
3. In the left-hand navigation pane, click the **Azure Active Directory** service, click **App registrations**, then find/click the application you want to configure. You are taken to the application's main registration page, which opens up the **Settings** page for the application.
4. From the **Settings** page, choose **Properties** and change the **Multi-tenanted** switch to **No**, to first change your application to single-tenant, then click **Save**. The application's service principal objects remain in the tenants of all organizations that have already consented to it.
5. Click on the **Delete** button from the application's main registration page.
6. Click **Yes** in the confirmation message.

### Removing a multi-tenant application authorized by another organization
A subset of the applications that appear under the "All apps" filter (excluding the "My apps" registrations) on your tenant's main "App registrations" page, are multi-tenant applications. In technical terms, these multi-tenant applications are from another tenant, and were registered into your tenant during the consent process. More specifically, they are represented by only a service principal object in your tenant, with no corresponding application object. For more information on the differences between application and service principal objects, see [Application and service principal objects in Azure AD](active-directory-application-objects.md).

In order to remove a multi-tenant application’s access to your directory (after having granted consent), the company administrator must remove its service principal. The administrator must have global admin access and can remove it through the Azure portal or use the [Azure AD PowerShell Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151).

## Next steps
- For more information on how authentication works in Azure AD, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).
- See the [Branding Guidelines for Integrated Apps](active-directory-branding-guidelines.md) for tips on visual guidance for your app.
- For more information on the relationship between an application's Application and Service Principal object(s), see [Application Objects and Service Principal Objects](active-directory-application-objects.md).
- To learn more about the role the app manifest plays, see [Understanding the Azure Active Directory application manifest](active-directory-application-manifest.md)
- See the [Azure AD developer glossary](active-directory-dev-glossary.md) for definitions of some of the core Azure AD developer concepts.
- Visit the [Active Directory developer's guide](active-directory-developers-guide.md) for an overview of all developer-related content.

