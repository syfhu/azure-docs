---
title: Azure Active Directory v2.0 scopes, permissions, and consent | Microsoft Docs
description: A description of authorization in the Azure AD v2.0 endpoint, including scopes, permissions, and consent.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''

ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev

---
# Scopes, permissions, and consent in the Azure Active Directory v2.0 endpoint
Apps that integrate with Azure Active Directory (Azure AD) follow an authorization model that gives users control over how an app can access their data. The v2.0 implementation of the authorization model has been updated, and it changes how an app must interact with Azure AD. This article covers the basic concepts of this authorization model, including scopes, permissions, and consent.

> [!NOTE]
> The v2.0 endpoint does not support all Azure Active Directory scenarios and features. To determine whether you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).
>
>

## Scopes and permissions
Azure AD implements the [OAuth 2.0](active-directory-v2-protocols.md) authorization protocol. OAuth 2.0 is a method through which a third-party app can access web-hosted resources on behalf of a user. Any web-hosted resource that integrates with Azure AD has a resource identifier, or *Application ID URI*. For example, some of Microsoft's web-hosted resources include:

* The Office 365 Unified Mail API: `https://outlook.office.com`
* The Azure AD Graph API: `https://graph.windows.net`
* Microsoft Graph: `https://graph.microsoft.com`

The same is true for any third-party resources that have integrated with Azure AD. Any of these resources also can define a set of permissions that can be used to divide the functionality of that resource into smaller chunks. As an example, [Microsoft Graph](https://graph.microsoft.io) has defined permissions to do the following tasks, among others:

* Read a user's calendar
* Write to a user's calendar
* Send mail as a user

By defining these types of permissions, the resource has fine-grained control over its data and how the data is exposed. A third-party app can request these permissions from an app user. The app user must approve the permissions before the app can act on the user's behalf. By chunking the resource's functionality into smaller permission sets, third-party apps can be built to request only the specific permissions that they need to perform their function. App users can know exactly how an app will use their data, and they can be more confident that the app is not behaving with malicious intent.

In Azure AD and OAuth, these types of permissions are called *scopes*. They also sometimes are referred to as *oAuth2Permissions*. A scope is represented in Azure AD as a string value. Continuing with the Microsoft Graph example, the scope value for each permission is:

* Read a user's calendar by using `Calendars.Read`
* Write to a user's calendar by using `Calendars.ReadWrite`
* Send mail as a user using by `Mail.Send`

An app can request these permissions by specifying the scopes in requests to the v2.0 endpoint.

## OpenID Connect scopes
The v2.0 implementation of OpenID Connect has a few well-defined scopes that do not apply to a specific resource: `openid`, `email`, `profile`, and `offline_access`.

### openid
If an app performs sign-in by using [OpenID Connect](active-directory-v2-protocols.md), it must request the `openid` scope. The `openid` scope shows on the work account consent page as the "Sign you in" permission, and on the personal Microsoft account consent page as the "View your profile and connect to apps and services using your Microsoft account" permission. With this permission, an app can receive a unique identifier for the user in the form of the `sub` claim. It also gives the app access to the UserInfo endpoint. The `openid` scope can be used at the v2.0 token endpoint to acquire ID tokens, which can be used to secure HTTP calls between different components of an app.

### email
The `email` scope can be used with the `openid` scope and any others. It gives the app access to the user's primary email address in the form of the `email` claim. The `email` claim is included in a token only if an email address is associated with the user account, which is not always the case. If it uses the `email` scope, your app should be prepared to handle a case in which the `email` claim does not exist in the token.

### profile
The `profile` scope can be used with the `openid` scope and any others. It gives the app access to a substantial amount of information about the user. The information it can access includes, but is not limited to, the user's given name, surname, preferred username, and object ID. For a complete list of the profile claims available in the id_tokens parameter for a specific user, see the [v2.0 tokens reference](active-directory-v2-tokens.md).

### offline_access
The [`offline_access` scope](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) gives your app access to resources on behalf of the user for an extended time. On the work account consent page, this scope appears as the "Access your data anytime" permission. On the personal Microsoft account consent page, it appears as the "Access your info anytime" permission. When a user approves the `offline_access` scope, your app can receive refresh tokens from the v2.0 token endpoint. Refresh tokens are long-lived. Your app can get new access tokens as older ones expire.

If your app does not request the `offline_access` scope, it won't receive refresh tokens. This means that when you redeem an authorization code in the [OAuth 2.0 authorization code flow](active-directory-v2-protocols.md), you'll receive only an access token from the `/token` endpoint. The access token is valid for a short time. The access token usually expires in one hour. At that point, your app needs to redirect the user back to the `/authorize` endpoint to get a new authorization code. During this redirect, depending on the type of app, the user might need to enter their credentials again or consent again to permissions.

For more information about how to get and use refresh tokens, see the [v2.0 protocol reference](active-directory-v2-protocols.md).

## Requesting individual user consent
In an [OpenID Connect or OAuth 2.0](active-directory-v2-protocols.md) authorization request, an app can request the permissions it needs by using the `scope` query parameter. For example, when a user signs in to an app, the app sends a request like the following example (with line breaks added for legibility):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

The `scope` parameter is a space-separated list of scopes that the app is requesting. Each scope is indicated by appending the scope value to the resource's identifier (the Application ID URI). In the request example, the app needs permission to read the user's calendar and send mail as the user.

After the user enters their credentials, the v2.0 endpoint checks for a matching record of *user consent*. If the user has not consented to any of the requested permissions in the past, the v2.0 endpoint asks the user to grant the requested permissions.

![Work account consent](../../media/active-directory-v2-flows/work_account_consent.png)

When the user approves the permission, the consent is recorded so that the user doesn't have to consent again on subsequent account sign-ins.

## Requesting consent for an entire tenant
Often, when an organization purchases a license or subscription for an application, the organization wants to fully provision the application for its employees. As part of this process, an administrator can grant consent for the application to act on behalf of any employee. If the admin grants consent for the entire tenant, the organization's employees won't see a consent page for the application.

To request consent for all users in a tenant, your app can use the admin consent endpoint.

## Admin-restricted scopes
Some high-privilege permissions in the Microsoft ecosystem can be set to *admin-restricted*. Examples of these kinds of scopes include the following permissions:

* Read an organization's directory data by using `Directory.Read`
* Write data to an organization's directory by using `Directory.ReadWrite`
* Read security groups in an organization's directory by using `Groups.Read.All`

Although a consumer user might grant an application access to this kind of data, organizational users are restricted from granting access to the same set of sensitive company data. If your application requests access to one of these permissions from an organizational user, the user receives an error message that says they are not authorized to consent to your app's permissions.

If your app requires access to admin-restricted scopes for organizations, you should request them directly from a company administrator, also by using the admin consent endpoint, described next.

When an administrator grants these permissions via the admin consent endpoint, consent is granted for all users in the tenant.

## Using the admin consent endpoint
If you follow these steps, your app can gather permissions for all users in a tenant, including admin-restricted scopes. To see a code sample that implements the steps, see the [admin-restricted scopes sample](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### Request the permissions in the app registration portal
1. Go to your application in the [Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or [create an app](active-directory-v2-app-registration.md) if you haven't already.
2. Locate the **Microsoft Graph Permissions** section, and then add the permissions that your app requires.
3. Make sure you **Save** the app registration.

### Recommended: Sign the user in to your app
Typically, when you build an application that uses the admin consent endpoint, the app needs a page or view in which the admin can approve the app's permissions. This page can be part of the app's sign-up flow, part of the app's settings, or it can be a dedicated "connect" flow. In many cases, it makes sense for the app to show this "connect" view only after a user has signed in with a work or school Microsoft account.

When you sign the user in to your app, you can identify the organization to which the admin belongs before asking them to approve the necessary permissions. Although not strictly necessary, it can help you create a more intuitive experience for your organizational users. To sign the user in, follow our [v2.0 protocol tutorials](active-directory-v2-protocols.md).

### Request the permissions from a directory admin
When you're ready to request permissions from your organization's admin, you can redirect the user to the v2.0 *admin consent endpoint*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Condition | Description |
| --- | --- | --- |
| tenant |Required |The directory tenant that you want to request permission from. Can be provided in GUID or friendly name format OR generically referenced with "common" as seen in the example. |
| client_id |Required |The Application ID that the [Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) assigned to your app. |
| redirect_uri |Required |The redirect URI where you want the response to be sent for your app to handle. It must exactly match one of the redirect URIs that you registered in the app registration portal. |
| state |Recommended |A value included in the request that will also be returned in the token response. It can be a string of any content you want. Use the state to encode information about the user's state in the app before the authentication request occurred, such as the page or view they were on. |

At this point, Azure AD requires a tenant administrator to sign in to complete the request. The administrator is asked to approve all the permissions that you have requested for your app in the app registration portal.

#### Successful response
If the admin approves the permissions for your app, the successful response looks like this:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Description |
| --- | --- | --- |
| tenant |The directory tenant that granted your application the permissions it requested, in GUID format. |
| state |A value included in the request that also will be returned in the token response. It can be a string of any content you want. The state is used to encode information about the user's state in the app before the authentication request occurred, such as the page or view they were on. |
| admin_consent |Will be set to **true**. |

#### Error response
If the admin does not approve the permissions for your app, the failed response looks like this:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Description |
| --- | --- | --- |
| error |An error code string that can be used to classify types of errors that occur, and can be used to react to errors. |
| error_description |A specific error message that can help a developer identify the root cause of an error. |

After you've received a successful response from the admin consent endpoint, your app has gained the permissions it requested. Next, you can request a token for the resource you want.

## Using permissions
After the user consents to permissions for your app, your app can acquire access tokens that represent your app's permission to access a resource in some capacity. An access token can be used only for a single resource, but encoded inside the access token is every permission that your app has been granted for that resource. To acquire an access token, your app can make a request to the v2.0 token endpoint, like this:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

You can use the resulting access token in HTTP requests to the resource. It reliably indicates to the resource that your app has the proper permission to perform a specific task. 

For more information about the OAuth 2.0 protocol and how to get access tokens, see the [v2.0 endpoint protocol reference](active-directory-v2-protocols.md).
