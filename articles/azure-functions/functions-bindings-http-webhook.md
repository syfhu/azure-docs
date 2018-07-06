---
title: Azure Functions HTTP and webhook bindings
description: Understand how to use HTTP and webhook triggers and bindings in Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: azure functions, functions, event processing, webhooks, dynamic compute, serverless architecture, HTTP, API, REST

ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: tdykstra
---

# Azure Functions HTTP and webhook bindings

This article explains how to work with HTTP bindings in Azure Functions. Azure Functions supports HTTP triggers and output bindings.

An HTTP trigger can be customized to respond to [webhooks](https://en.wikipedia.org/wiki/Webhook). A webhook trigger accepts only a  JSON payload and validates the JSON. There are special versions of the webhook trigger that make it easier to handle webhooks from certain providers, such as GitHub and Slack.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## Packages - Functions 1.x

The HTTP bindings are provided in the [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet package, version 1.x. Source code for the package is in the [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) GitHub repository.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## Packages - Functions 2.x

The HTTP bindings are provided in the [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet package, version 3.x. Source code for the package is in the [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub repository.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## Trigger

The HTTP trigger lets you invoke a function with an HTTP request. You can use an HTTP trigger to build serverless APIs and respond to webhooks. 

By default, an HTTP trigger returns HTTP 200 OK with an empty body in Functions 1.x, or HTTP 204 No Content with an empty body in Functions 2.x. To modify the response, configure an [HTTP output binding](#http-output-binding).

## Trigger - example

See the language-specific example:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### Trigger - C# example

The following example shows a [C# function](functions-dotnet-class-library.md) that looks for a `name` parameter either in the query string or the body of the HTTP request. Notice that the return value is used for the output binding, but a return value attribute isn't required.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

### Trigger - C# script example

The following example shows a trigger binding in a *function.json* file and a [C# script function](functions-reference-csharp.md) that uses the binding. The function looks for a `name` parameter either in the query string or the body of the HTTP request.

Here's the *function.json* file:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

The [configuration](#trigger---configuration) section explains these properties.

Here's C# script code that binds to `HttpRequestMessage`:

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

You can bind to a custom object instead of `HttpRequestMessage`. This object is created from the body of the request, parsed as JSON. Similarly, a type can be passed to the HTTP response output binding and returned as the response body, along with a 200 status code.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
```

### Trigger - F# example

The following example shows a trigger binding in a *function.json* file and an [F# function](functions-reference-fsharp.md) that uses the binding. The function looks for a `name` parameter either in the query string or the body of the HTTP request.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

The [configuration](#trigger---configuration) section explains these properties.

Here's the F# code:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

You need a `project.json` file that uses NuGet to reference the `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, as shown in the following example:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

### Trigger - JavaScript example

The following example shows a trigger binding in a *function.json* file and a [JavaScript function](functions-reference-node.md) that uses the binding. The function looks for a `name` parameter either in the query string or the body of the HTTP request.

Here's the *function.json* file:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

The [configuration](#trigger---configuration) section explains these properties.

Here's the JavaScript code:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
## Trigger - webhook example

See the language-specific example:

* [C#](#webhook---c-example)
* [C# script (.csx)](#webhook---c-script-example)
* [F#](#webhook---f-example)
* [JavaScript](#webhook---javascript-example)

### Webhook - C# example

The following example shows a [C# function](functions-dotnet-class-library.md) that sends an HTTP 200 in response to a generic JSON request.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### Webhook - C# script example

The following example shows a webhook trigger binding in a *function.json* file and a [C# script function](functions-reference-csharp.md) that uses the binding. The function logs GitHub issue comments.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

The [configuration](#trigger---configuration) section explains these properties.

Here's the C# script code:

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

### Webhook - F# example

The following example shows a webhook trigger binding in a *function.json* file and an [F# function](functions-reference-fsharp.md) that uses the binding. The function logs GitHub issue comments.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

The [configuration](#trigger---configuration) section explains these properties.

Here's the F# code:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

### Webhook - JavaScript example

The following example shows a webhook trigger binding in a *function.json* file and a [JavaScript function](functions-reference-node.md) that uses the binding. The function logs GitHub issue comments.

Here's the binding data in the *function.json* file:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

The [configuration](#trigger---configuration) section explains these properties.

Here's the JavaScript code:

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## Trigger - attributes

In [C# class libraries](functions-dotnet-class-library.md), use the [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) attribute.

You can set the authorization level and allowable HTTP methods in attribute constructor parameters, and there are properties for webhook type and route template. For more information about these settings, see [Trigger - configuration](#trigger---configuration). Here's an `HttpTrigger` attribute in a method signature:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

For a complete example, see [Trigger - C# example](#trigger---c-example).

## Trigger - configuration

The following table explains the binding configuration properties that you set in the *function.json* file and the `HttpTrigger` attribute.

|function.json property | Attribute property |Description|
|---------|---------|----------------------|
| **type** | n/a| Required - must be set to `httpTrigger`. |
| **direction** | n/a| Required - must be set to `in`. |
| **name** | n/a| Required - the variable name used in function code for the request or request body. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Determines what keys, if any, need to be present on the request in order to invoke the function. The authorization level can be one of the following values: <ul><li><code>anonymous</code>&mdash;No API key is required.</li><li><code>function</code>&mdash;A function-specific API key is required. This is the default value if none is provided.</li><li><code>admin</code>&mdash;The master key is required.</li></ul> For more information, see the section about [authorization keys](#authorization-keys). |
| **methods** |**Methods** | An array of the HTTP methods to which the function  responds. If not specified, the function responds to all HTTP methods. See [customize the http endpoint](#customize-the-http-endpoint). |
| **route** | **Route** | Defines the route template, controlling to which request URLs your function responds. The default value if none is provided is `<functionname>`. For more information, see [customize the http endpoint](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |Configures the HTTP trigger to act as a [webhook](https://en.wikipedia.org/wiki/Webhook) receiver for the specified provider. Don't set the `methods` property if you set this property. The webhook type can be one of the following values:<ul><li><code>genericJson</code>&mdash;A general-purpose webhook endpoint without logic for a specific provider. This setting restricts requests to only those using HTTP POST and with the `application/json` content type.</li><li><code>github</code>&mdash;The function responds to [GitHub webhooks](https://developer.github.com/webhooks/). Do not use the  _authLevel_ property with GitHub webhooks. For more information, see the GitHub webhooks section later in this article.</li><li><code>slack</code>&mdash;The function responds to [Slack webhooks](https://api.slack.com/outgoing-webhooks). Do not use the _authLevel_ property with Slack webhooks. For more information, see the Slack webhooks section later in this article.</li></ul>|

## Trigger - usage

For C# and F# functions, you can declare the type of your trigger input to be either `HttpRequestMessage` or a custom type. If you choose `HttpRequestMessage`, you get full access to the request object. For a custom type, Functions tries to parse the JSON request body to set the object properties. 

For JavaScript functions, the Functions runtime provides the request body instead of the request object. For more information, see the [JavaScript trigger example](#trigger---javascript-example).

### GitHub webhooks

To respond to GitHub webhooks, first create your function with an HTTP Trigger, and set the **webHookType** property to `github`. Then copy its URL and API key into the **Add webhook** page of your GitHub repository. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

For an example, see [Create a function triggered by a GitHub webhook](functions-create-github-webhook-triggered-function.md).

### Slack webhooks

The Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with the token from Slack. See [Authorization keys](#authorization-keys).

### Customize the HTTP endpoint

By default when you create a function for an HTTP trigger, or WebHook, the function is addressable with a route of the form:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

You can customize this route using the optional `route` property on the HTTP trigger's input binding. As an example, the following *function.json* file defines a `route` property for an HTTP trigger:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Using this configuration, the function is now addressable with the following route instead of the original route.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

This allows the function code to support two parameters in the address, _category_ and _id_. You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters. The following C# function code makes use of both parameters.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Here is Node.js function code that uses the same route parameters.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

By default, all function routes are prefixed with *api*. You can also customize or remove the prefix using the `http.routePrefix` property in your [host.json](functions-host-json.md) file. The following example removes the *api* route prefix by using an empty string for the prefix in the *host.json* file.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### Authorization keys

HTTP triggers let you use keys for added security. A standard HTTP trigger can use these as an API key, requiring the key to be present on the request. Webhooks can use keys to authorize requests in a variety of ways, depending on what the provider supports.

> [!NOTE]
> When running functions locally, authorization is disabled no matter the `authLevel` set in `function.json`. As soon as you publish to Azure Functions, the `authLevel` immediately takes effect.

Keys are stored as part of your function app in Azure and are encrypted at rest. To view your keys, create new ones, or roll keys to new values, navigate to one of your functions in the portal and select "Manage." 

There are two types of keys:

- **Host keys**: These keys are shared by all functions within the function app. When used as an API key, these allow access to any function within the function app.
- **Function keys**: These keys apply only to the specific functions under which they are defined. When used as an API key, these only allow access to that function.

Each key is named for reference, and there is a default key (named "default") at the function and host level. Function keys take precedence over host keys. When two keys are defined with the same name, the function key is always used.

The **master key** is a default host key named "_master" that is defined for each function app. This key cannot be revoked. It provides administrative access to the runtime APIs. Using `"authLevel": "admin"` in the binding JSON requires this key to be presented on the request; any other key results in authorization failure.

> [!IMPORTANT]  
> Due to the elevated permissions granted by the master key, you should not share this key with third parties or distribute it in native client applications. Use caution when choosing the admin authorization level.

### API key authorization

By default, an HTTP trigger requires an API key in the HTTP request. So your HTTP request normally looks like the following:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

The key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header. The value of the key can be any function key defined for the function, or any host key.

You can allow anonymous requests, which do not require keys. You can also require that the master key be used. You change the default authorization level by using the `authLevel` property in the binding JSON. For more information, see [Trigger - configuration](#trigger---configuration).

### Keys and webhooks

Webhook authorization is handled by the webhook receiver component, part of the HTTP trigger, and the mechanism varies based on the webhook type. Each mechanism does, however rely on a key. By default, the function key named "default" is used. To use a different key, configure the webhook provider to send the key name with the request in one of the following ways:

- **Query string**: The provider passes the key name in the `clientid` query string parameter, such as `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Request header**: The provider passes the key name in the `x-functions-clientid` header.

## Trigger - limits

The HTTP request length is limited to 100MB (104,857,600 bytes), and the URL length is limited to 4KB (4,096 bytes). These limits are specified by the `httpRuntime` element of the runtime's [Web.config file](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

If a function that uses the HTTP trigger doesn't complete within about 2.5 minutes, the gateway will timeout and return an HTTP 502 error. The function will continue running but will be unable to return an HTTP response. For long-running functions, we recommend that you follow async patterns and return a location where you can ping the status of the request. For information about how long a function can run, see [Scale and hosting - Consumption plan](functions-scale.md#consumption-plan). 

## Trigger - host.json properties

The [host.json](functions-host-json.md) file contains settings that control HTTP trigger behavior.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## Output

Use the HTTP output binding to respond to the HTTP request sender. This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request. If an HTTP output binding is not provided, an HTTP trigger returns HTTP 200 OK with an empty body in Functions 1.x, or HTTP 204 No Content with an empty body in Functions 2.x.

## Output - configuration

The following table explains the binding configuration properties that you set in the *function.json* file. For C# class libraries there are no attribute properties that correspond to these *function.json* properties. 

|Property  |Description  |
|---------|---------|
| **type** |Must be set to `http`. |
| **direction** | Must be set to `out`. |
|**name** | The variable name used in function code for the response, or `$return` to use the return value. |

## Output - usage

To send an HTTP response, use the language-standard response patterns. In C# or C# script, make the function return type `HttpResponseMessage` or `Task<HttpResponseMessage>`. In C#, a return value attribute isn't required.

For example responses, see the [trigger example](#trigger---example) and the [webhook example](#trigger---webhook-example).

## Next steps

[Learn more about Azure functions triggers and bindings](functions-triggers-bindings.md)
