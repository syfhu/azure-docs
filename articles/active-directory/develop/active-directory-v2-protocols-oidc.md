---
title: Azure Active Directory v2.0 and the OpenID Connect protocol | Microsoft Docs
description: Build web applications by using the Azure AD v2.0 implementation of the OpenID Connect authentication protocol.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''

ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
---

# Azure Active Directory v2.0 and the OpenID Connect protocol
OpenID Connect is an authentication protocol built on OAuth 2.0 that you can use to securely sign in a user to a web application. When you use the v2.0 endpoint's implementation of OpenID Connect, you can add sign-in and API access to your web-based apps. In this article, we show you how to do this independent of language. We describe how to send and receive HTTP messages without using any Microsoft open-source libraries.

> [!NOTE]
> The v2.0 endpoint does not support all Azure Active Directory scenarios and features. To determine whether you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).
> 
> 

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) extends the OAuth 2.0 *authorization* protocol to use as an *authentication* protocol, so that you can perform single sign-on using OAuth. OpenID Connect introduces the concept of an *ID token*, which is a security token that allows the client to verify the identity of the user. The ID token also gets basic profile information about the user. Because OpenID Connect extends OAuth 2.0, apps can securely acquire *access tokens*, which can be used to access resources that are secured by an [authorization server](active-directory-v2-protocols.md#the-basics). The v2.0 endpoint also allows third party apps that are registered with Azure AD to issue access tokens for secured resources such as Web APIs. For more information about how to setup an application to issue access tokens, please see [How to register an app with the v2.0 endpoint](active-directory-v2-app-registration.md). We recommend that you use OpenID Connect if you are building a [web application](active-directory-v2-flows.md#web-apps) that is hosted on a server and accessed via a browser.

## Protocol diagram: Sign-in
The most basic sign-in flow has the steps shown in the next diagram. We describe each step in detail in this article.

![OpenID Connect protocol: Sign-in](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## Fetch the OpenID Connect metadata document
OpenID Connect describes a metadata document that contains most of the information required for an app to perform sign-in. This includes information such as the URLs to use and the location of the service's public signing keys. For the v2.0 endpoint, this is the OpenID Connect metadata document you should use:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```
> [!TIP] 
> Try it! Click [https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration](https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) to see the `common` tenants configuration. 
>

The `{tenant}` can take one of four values:

| Value | Description |
| --- | --- |
| `common` |Users with both a personal Microsoft account and a work or school account from Azure Active Directory (Azure AD) can sign in to the application. |
| `organizations` |Only users with work or school accounts from Azure AD can sign in to the application. |
| `consumers` |Only users with a personal Microsoft account can sign in to the application. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` or `contoso.onmicrosoft.com` |Only users with a work or school account from a specific Azure AD tenant can sign in to the application. Either the friendly domain name of the Azure AD tenant or the tenant's GUID identifier can be used. |

The metadata is a simple JavaScript Object Notation (JSON) document. See the following snippet for an example. The snippet's contents are fully described in the [OpenID Connect specification](https://openid.net/specs/openid-connect-discovery-1_0.html#rfc.section.4.2).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

Typically, you would use this metadata document to configure an OpenID Connect library or SDK; the library would use the metadata to do its work. However, if you're not using a pre-build OpenID Connect library, you can follow the steps in the remainder of this article to perform sign-in in a web app by using the v2.0 endpoint.

## Send the sign-in request
When your web app needs to authenticate the user, it can direct the user to the `/authorize` endpoint. This request is similar to the first leg of the [OAuth 2.0 authorization code flow](active-directory-v2-protocols-oauth-code.md), with these important distinctions:

* The request must include the `openid` scope in the `scope` parameter.
* The `response_type` parameter must include `id_token`.
* The request must include the `nonce` parameter.

> [!IMPORTANT]
> In order to succesfully request an ID token, the app registration in the [registration portal](https://apps.dev.microsoft.com) must have the **[Implicit grant](active-directory-v2-protocols-implicit.md)** enabled for the Web client. If it is not enabled, an `unsupported_response` error will be returned: "The provided value for the input parameter 'response_type' is not allowed for this client. Expected value is 'code'"

For example:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Click the following link to execute this request. After you sign in, your browser will be redirected to https://localhost/myapp/, with an ID token in the address bar. Note that this request uses `response_mode=fragment` (for demonstration purposes only). We recommend that you use `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parameter | Condition | Description |
| --- | --- | --- |
| tenant |Required |You can use the `{tenant}` value in the path of the request to control who can sign in to the application. The allowed values are `common`, `organizations`, `consumers`, and tenant identifiers. For more information, see [protocol basics](active-directory-v2-protocols.md#endpoints). |
| client_id |Required |The Application ID that the [Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) assigned to your app. |
| response_type |Required |Must include `id_token` for OpenID Connect sign-in. It might also include other `response_types` values, such as `code`. |
| redirect_uri |Recommended |The redirect URI of your app, where authentication responses can be sent and received by your app. It must exactly match one of the redirect URIs you registered in the portal, except that it must be URL encoded. |
| scope |Required |A space-separated list of scopes. For OpenID Connect, it must include the scope `openid`, which translates to the "Sign you in" permission in the consent UI. You might also include other scopes in this request for requesting consent. |
| nonce |Required |A value included in the request, generated by the app, that will be included in the resulting id_token value as a claim. The app can verify this value to mitigate token replay attacks. The value typically is a randomized, unique string that can be used to identify the origin of the request. |
| response_mode |Recommended |Specifies the method that should be used to send the resulting authorization code back to your app. Can be `form_post` or `fragment`. For web applications, we recommend using `response_mode=form_post`, to ensure the most secure transfer of tokens to your application. |
| state |Recommended |A value included in the request that also will be returned in the token response. It can be a string of any content you want. A randomly generated unique value typically is used to [prevent cross-site request forgery attacks](http://tools.ietf.org/html/rfc6749#section-10.12). The state also is used to encode information about the user's state in the app before the authentication request occurred, such as the page or view the user was on. |
| prompt |Optional |Indicates the type of user interaction that is required. The only valid values at this time are `login`, `none`, and `consent`. The `prompt=login` claim forces the user to enter their credentials on that request, which negates single sign-on. The `prompt=none` claim is the opposite. This claim ensures that the user is not presented with any interactive prompt whatsoever. If the request cannot be completed silently via single sign-on, the v2.0 endpoint returns an error. The `prompt=consent` claim triggers the OAuth consent dialog after the user signs in. The dialog asks the user to grant permissions to the app. |
| login_hint |Optional |You can use this parameter to pre-fill the username and email address field of the sign-in page for the user, if you know the username ahead of time. Often, apps use this parameter during re-authentication, after already extracting the username from an earlier sign-in by using the `preferred_username` claim. |
| domain_hint |Optional |This value can be `consumers` or `organizations`. If included, it skips the email-based discovery process that the user goes through on the v2.0 sign-in page, for a slightly more streamlined user experience. Often, apps use this parameter during re-authentication by extracting the `tid` claim from the ID token. If the `tid` claim value is `9188040d-6c67-4c5b-b112-36a304b66dad` (the Microsoft Account consumer tenant), use `domain_hint=consumers`. Otherwise, use `domain_hint=organizations`. |

At this point, the user is prompted to enter their credentials and complete the authentication. The v2.0 endpoint verifies that the user has consented to the permissions indicated in the `scope` query parameter. If the user has not consented to any of those permissions, the v2.0 endpoint prompts the user to consent to the required permissions. You can read more about [permissions, consent, and multitenant apps](active-directory-v2-scopes.md).

After the user authenticates and grants consent, the v2.0 endpoint returns a response to your app at the indicated redirect URI by using the method specified in the `response_mode` parameter.

### Successful response
A successful response when you use `response_mode=form_post` looks like this:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Description |
| --- | --- |
| id_token |The ID token that the app requested. You can use the `id_token` parameter to verify the user's identity and begin a session with the user. For more details about ID tokens and their contents, see the [v2.0 endpoint tokens reference](active-directory-v2-tokens.md). |
| state |If a `state` parameter is included in the request, the same value should appear in the response. The app should verify that the state values in the request and response are identical. |

### Error response
Error responses might also be sent to the redirect URI so that the app can handle them. An error response looks like this:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| error |An error code string that you can use to classify types of errors that occur, and to react to errors. |
| error_description |A specific error message that can help you identify the root cause of an authentication error. |

### Error codes for authorization endpoint errors
The following table describes error codes that can be returned in the `error` parameter of the error response:

| Error code | Description | Client action |
| --- | --- | --- |
| invalid_request |Protocol error, such as a missing, required parameter. |Fix and resubmit the request. This is a development error that typically is caught during initial testing. |
| unauthorized_client |The client application cannot request an authorization code. |This usually occurs when the client application is not registered in Azure AD or is not added to the user's Azure AD tenant. The application can prompt the user with instructions to install the application and add it to Azure AD. |
| access_denied |The resource owner denied consent. |The client application can notify the user that it cannot proceed unless the user consents. |
| unsupported_response_type |The authorization server does not support the response type in the request. |Fix and resubmit the request. This is a development error that typically is caught during initial testing. |
| server_error |The server encountered an unexpected error. |Retry the request. These errors can result from temporary conditions. The client application might explain to the user that its response is delayed due to a temporary error. |
| temporarily_unavailable |The server is temporarily too busy to handle the request. |Retry the request. The client application might explain to the user that its response is delayed due to a temporary condition. |
| invalid_resource |The target resource is invalid because either it does not exist, Azure AD cannot find it, or it is not correctly configured. |This indicates that the resource, if it exists, has not been configured in the tenant. The application can prompt the user with instructions for installing the application and adding it to Azure AD. |

## Validate the ID token
Receiving an ID token is not sufficient to authenticate the user. You must also validate the ID token's signature and verify the claims in the token per your app's requirements. The v2.0 endpoint uses [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) and public key cryptography to sign tokens and verify that they are valid.

You can choose to validate the ID token in client code, but a common practice is to send the ID token to a back-end server and perform the validation there. After you've validated the signature of the ID token, you'll need to verify a few claims. For more information, including more about [validating tokens](active-directory-v2-tokens.md#validating-tokens) and [important information about signing key rollover](active-directory-v2-tokens.md#validating-tokens), see the [v2.0 tokens reference](active-directory-v2-tokens.md). We recommend using a library to parse and validate tokens. There's at least one of these libraries available for most languages and platforms.
<!--TODO: Improve the information on this-->

You also might want to validate additional claims, depending on your scenario. Some common validations include:

* Ensure that the user or organization has signed up for the app.
* Ensure that the user has required authorization or privileges.
* Ensure that a certain strength of authentication has occurred, such as multi-factor authentication.

For more information about the claims in an ID token, see the [v2.0 endpoint tokens reference](active-directory-v2-tokens.md).

After you have completely validated the ID token, you can begin a session with the user. Use the claims in the ID token to get information about the user in your app. You can use this information for display, records, authorizations, and so on.

## Send a sign-out request
When you want to sign out the user from your app, it isn't sufficient to clear your app's cookies or otherwise end the user's session. You must also redirect the user to the v2.0 endpoint to sign out. If you don't do this, the user re-authenticates to your app without entering their credentials again, because they will have a valid single sign-in session with the v2.0 endpoint.

You can redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | Condition | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Recommended | The URL that the user is redirected to after successfully signing out. If the parameter is not included, the user is shown a generic message that's generated by the v2.0 endpoint. This URL must match one of the redirect URIs registered for your application in the app registration portal. |

## Single sign-out
When you redirect the user to the `end_session_endpoint`, the v2.0 endpoint clears the user's session from the browser. However, the user may still be signed in to other applications that use Microsoft accounts for authentication. To enable those applications to sign the user out simultaneously, the v2.0 endpoint sends an HTTP GET request to the registered `LogoutUrl` of all the applications that the user is currently signed in to. Applications must respond to this request by clearing any session that identifies the user and returning a `200` response. If you wish to support single sign out in your application, you must implement such a `LogoutUrl` in your application's code. You can set the `LogoutUrl` from the app registration portal.

## Protocol diagram: Access token acquisition
Many web apps need to not only sign the user in, but also to access a web service on behalf of the user by using OAuth. This scenario combines OpenID Connect for user authentication while simultaneously getting an authorization code that you can use to get access tokens if you are using the OAuth authorization code flow.

The full OpenID Connect sign-in and token acquisition flow looks similar to the next diagram. We describe each step in detail in the next sections of the article.

![OpenID Connect  protocol: Token acquisition](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## Get access tokens
To acquire access tokens, modify the sign-in request:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'form_post' or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Click the following link to execute this request. After you sign in, your browser is redirected to https://localhost/myapp/, with an ID token and a code in the address bar. Note that this request uses `response_mode=fragment` (for demonstration purposes only). We recommend that you use `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=fragment&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

By including permission scopes in the request and by using `response_type=id_token code`, the v2.0 endpoint ensures that the user has consented to the permissions indicated in the `scope` query parameter. It returns an authorization code to your app to exchange for an access token.

### Successful response
A successful response from using `response_mode=form_post` looks like this:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Description |
| --- | --- |
| id_token |The ID token that the app requested. You can use the ID token to verify the user's identity and begin a session with the user. You'll find more details about ID tokens and their contents in the [v2.0 endpoint tokens reference](active-directory-v2-tokens.md). |
| code |The authorization code that the app requested. The app can use the authorization code to request an access token for the target resource. An authorization code is very short-lived. Typically, an authorization code expires in about 10 minutes. |
| state |If a state parameter is included in the request, the same value should appear in the response. The app should verify that the state values in the request and response are identical. |

### Error response
Error responses might also be sent to the redirect URI so that the app can handle them appropriately. An error response looks like this:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| error |An error code string that you can use to classify types of errors that occur, and to react to errors. |
| error_description |A specific error message that can help you identify the root cause of an authentication error. |

For a description of possible error codes and recommended client responses, see [Error codes for authorization endpoint errors](#error-codes-for-authorization-endpoint-errors).

When you have an authorization code and an ID token, you can sign the user in and get access tokens on their behalf. To sign the user in, you must validate the ID token [exactly as described](#validate-the-id-token). To get access tokens, follow the steps described in our [OAuth protocol documentation](active-directory-v2-protocols-oauth-code.md#request-an-access-token).
