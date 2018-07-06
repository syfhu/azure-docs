---
title: Frequently Asked Questions (FAQ) about Bing Web Search API on Azure | Microsoft Docs
description: Get answers to common questions about Microsoft Cognitive Services Bing Web Search API on Azure.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
---

# Frequently asked questions (FAQ) about Bing Web Search API (Cognitive Services)
 
 Find answers to commonly asked questions about concepts, code, and scenarios related to the Bing Web Search API for Microsoft Cognitive Services on Azure.

## Response headers in JavaScript

The following headers may occur in responses from the Bing Web Search API.

|||
|-|-|
|`X-MSEdge-ClientID`|The unique ID that Bing has assigned to the user|
|`BingAPIs-Market`|The market that was used to fulfill the request|
|`BingAPIs-TraceId`|The log entry on the Bing API server for this request (for support)|

It is particularly important to persist the client ID and return it with subsequent requests. When you do this, the search will use past context in ranking search results and also provide a consistent user experience.

However, when you call the Bing Web Search API from JavaScript, your browser's built-in security features (CORS) might prevent you from accessing the values of these headers.

To gain access to the headers, you can make the Bing Web Search API request through a CORS proxy. The response from such a proxy has an `Access-Control-Expose-Headers` header that whitelists response headers and makes them available to JavaScript.

It's easy to install a CORS proxy to allow our [tutorial app](tutorial-bing-web-search-single-page-app.md) to access the optional client headers. First, if you don't already have it, [install Node.js](https://nodejs.org/en/download/). Then enter the following command at a command prompt.

    npm install -g cors-proxy-server

Next, change the Bing Web Search API endpoint in the HTML file to:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Finally, start the CORS proxy with the following command:

    cors-proxy-server

Leave the command window open while you use the tutorial app; closing the window stops the proxy. In the expandable HTTP Headers section below the search results, you can now see the `X-MSEdge-ClientID` header (among others) and verify that it is the same for each request.

## Response headers in production

The CORS proxy approach described in the previous answer is appropriate for development, testing, and learning. 

In a production environment, however, you should host a server-side script on the same domain as the Web page that uses the Bing Web Search API. This script should actually do the API calls upon request from the Web page JavaScript and pass all results, including headers, back to the client. Since the two resources (page and script) share an origin, CORS does not come into play and the special headers are acessible to the JavaScript on the Web page. 

This approach also protects your API key from exposure to the public, since only the server-side script needs it. The script can use another method to make sure the request is authorized.

## Next steps

Is your question about a missing feature or functionality? Consider requesting or voting for it on our [User Voice web site](https://cognitive.uservoice.com/forums/555907-bing-search).

## See also

 [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/bing-api)