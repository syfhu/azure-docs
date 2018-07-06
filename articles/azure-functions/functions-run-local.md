---
title: Work with Azure Functions Core Tools | Microsoft Docs
description: Learn how to code and test Azure functions from the command prompt or terminal on your local computer before you run them on Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''

ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/26/2018
ms.author: glenga

---
# Work with Azure Functions Core Tools

Azure Functions Core Tools lets you develop and test your functions on your local computer from the command prompt or terminal. Your local functions can connect to live Azure services, and you can debug your functions on your local computer using the full Functions runtime. You can even deploy a function app to your Azure subscription.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## Core Tools versions

There are two versions of Azure Functions Core Tools. The version you use depends on your local development environment, choice of language, and level of support required:

+ [Version 1.x](#v1): supports version 1.x of the runtime, which is Generally Available (GA). This version of the tools is only supported on Windows computers and is installed from an [npm package](https://docs.npmjs.com/getting-started/what-is-npm). With this version, you can create functions in experimental languages that are not officially supported. For more information, see [Supported languages in Azure Functions](supported-languages.md)

+ [Version 2.x](#v2): supports version 2.x of the runtime. This version supports [Windows](#windows-npm), [macOS](#brew), and [Linux](#linux). Uses platform-specific package managers or npm for installation. Like the 2.x runtime, this version of the core tools is currently in preview.

Unless otherwise noted, the examples in this article are for version 2.x.

## Install the Azure Functions Core Tools

[Azure Functions Core Tools] includes a version of the same runtime that powers Azure Functions runtime that you can run on your local development computer. It also provides commands to create functions, connect to Azure, and deploy function projects.

### <a name="v1"></a>Version 1.x

The original version of the tools uses the Functions 1.x runtime. This version uses the .NET Framework (4.7.1) and is only supported on Windows computers. Before you install the version 1.x tools, you must [install NodeJS](https://docs.npmjs.com/getting-started/installing-node), which includes npm.

Use the following command to install the version 1.x tools:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Version 2.x

>[!NOTE]
> Azure Functions runtime 2.0 is in preview, and currently not all features of Azure Functions are supported. For more information, see [Azure Functions versions](functions-versions.md) 

Version 2.x of the tools uses the Azure Functions runtime 2.x that is built on .NET Core. This version is supported on all platforms .NET Core 2.x supports, including [Windows](#windows-npm), [macOS](#brew), and [Linux](#linux).

#### <a name="windows-npm"></a>Windows

The following steps use npm to install Core Tools on Windows. You can also use [Chocolatey](https://chocolatey.org/). For more information, see the [Core Tools readme](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Install [.NET Core 2.0 for Windows](https://www.microsoft.com/net/download/windows).

2. Install [Node.js], which includes npm. For version 2.x of the tools, only Node.js 8.5 and later versions are supported.

3. Install the Core Tools package:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>MacOS with Homebrew

The following steps use Homebrew to install the Core Tools on macOS.

1. Install [.NET Core 2.0 for macOS](https://www.microsoft.com/net/download/macos).

2. Install [Homebrew](https://brew.sh/), if it's not already installed.

3. Install the Core Tools package:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) with APT

The following steps use [APT](https://wiki.debian.org/Apt) to install Core Tools on your Ubuntu/Debian Linux distribution. For other Linux distributions, see the [Core Tools readme](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Install [.NET Core 2.0 for Linux](https://www.microsoft.com/net/download/linux).

2. Register the Microsoft product key as trusted:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Verify your Ubuntu server is running one of the appropriate versions from the table below. To add the apt source, run:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux distribution | Version |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Mint 18    | `xenial`  |

4. Install the Core Tools package:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## Create a local Functions project

A functions project directory contains the files [host.json](functions-host-json.md) and [local.settings.json](#local-settings-file), along subfolders that contain the code for individual functions. This directory is the equivalent of a function app in Azure. To learn more about the Functions folder structure, see the [Azure Functions developers guide](functions-reference.md#folder-structure).

Version 2.x requires you to select a default language for your project when it is initialized, and all functions added use default language templates. In version 1.x, you specify the language each time you create a function.

In the terminal window or from a command prompt, run the following command to create the project and local Git repository:

```bash
func init MyFunctionProj
```

In version 2.x, when you run the command you must choose a runtime for your project. If you plan to develop JavaScript functions, choose **node**:

```output
Select a worker runtime:
dotnet
node
```

Use the up/down arrow keys to choose a language, then press Enter. The output looks like the following example for a JavaScript project:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

To create the project without a local Git repository, use the `--no-source-control [-n]` option.

## Register extensions

In version 2.x of the Azure Functions runtime, you have to explicitly register the binding extensions (binding types) that you use in your function app.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

For more information, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md#register-binding-extensions).

## Local settings file

The file local.settings.json stores app settings, connection strings, and settings for Azure Functions Core Tools. It has the following structure:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*"
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```

| Setting      | Description                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | When set to **true**, all values are encrypted using a local machine key. Used with `func settings` commands. Default value is **false**. |
| **Values** | Collection of application settings and connection strings used when running locally. These values correspond to app settings in your function app in Azure, such as **AzureWebJobsStorage** and **AzureWebJobsDashboard**. Many triggers and bindings have a property that refers to a connection string app setting, such as **Connection** for the [Blob storage trigger](functions-bindings-storage-blob.md#trigger---configuration). For such properties, you need an application setting defined in the **Values** array. <br/>**AzureWebJobsStorage** is a required app setting for triggers other than HTTP. When you have the [Azure storage emulator](../storage/common/storage-use-emulator.md) installed locally, you can set **AzureWebJobsStorage** to `UseDevelopmentStorage=true` and Core Tools uses the emulator. This is useful during development, but you should test with an actual storage connection before deployment. |
| **Host** | Settings in this section customize the Functions host process when running locally. |
| **LocalHttpPort** | Sets the default port used when running the local Functions host (`func host start` and `func run`). The `--port` command-line option takes precedence over this value. |
| **CORS** | Defines the origins allowed for [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Origins are supplied as a comma-separated list with no spaces. The wildcard value (\*) is supported, which allows requests from any origin. |
| **ConnectionStrings** | Do not use this collection for the connection strings used by your function bindings. This collection is only used by frameworks that must get connection strings from the **ConnectionStrings** section of a configuration file, such as [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Connection strings in this object are added to the environment with the provider type of [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Items in this collection are not published to Azure with other app settings. You must explicitly add these values to the **Connection strings** section of the **Application settings** for your function app. |

The function app settings values can also be read in your code as environment variables. For more information, see the Environment variables section of these language-specific reference topics:

+ [C# precompiled](functions-dotnet-class-library.md#environment-variables)
+ [C# script (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Settings in the local.settings.json file are only used by Functions tools when running locally. By default, these settings are not migrated automatically when the project is published to Azure. Use the `--publish-local-settings` switch [when you publish](#publish) to make sure these settings are added to the function app in Azure. Values in **ConnectionStrings** are never published.

When no valid storage connection string is set for **AzureWebJobsStorage** and the emulator isn't being used, the following error message is shown:  

>Missing value for AzureWebJobsStorage in local.settings.json. This is required for all triggers other than HTTP. You can run 'func azure functionapp fetch-app-settings <functionAppName>' or specify a connection string in local.settings.json.

### Get your storage connection strings

Even when using the storage emulator for development, you may want to test with an actual storage connection. Assuming you have already [created a storage account](../storage/common/storage-create-storage-account.md), you can get a valid storage connection string in one of the following ways:

+ From the [Azure portal]. Navigate to your storage account, select **Access keys** in **Settings**, then copy one of the **Connection string** values.

  ![Copy connection string from Azure portal](./media/functions-run-local/copy-storage-connection-portal.png)

+ Use [Azure Storage Explorer](http://storageexplorer.com/) to connect to your Azure account. In the **Explorer**, expand your subscription, select your storage account, and copy the primary or secondary connection string. 

  ![Copy connection string from Storage Explorer](./media/functions-run-local/storage-explorer.png)

+ Use Core Tools to download the connection string from Azure with one of the following commands:

    + Download all settings from an existing function app:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + Get the Connection string for a specific storage account:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    When you are not already signed in to Azure, you are prompted to do so.

## <a name="create-func"></a>Create a function

To create a function, run the following command:

```bash
func new
```

In version 2.x, when you run `func new` you are prompted to choose a template in the default language of your function app, then you are also prompted to choose a name for your function. In version 1.x, you are also prompted to choose the language.

```output
Select a language: Select a template:
Blob trigger
Cosmos DB trigger
Event Grid trigger
HTTP trigger
Queue trigger
SendGrid
Service Bus Queue trigger
Service Bus Topic trigger
Timer trigger
```

Function code is generated in a subfolder with the provided function name, as you can see in the following queue trigger output:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

You can also specify these options in the command using the following arguments:

| Argument     | Description                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| The template programming language, such as C#, F#, or JavaScript. This option is required in version 1.x. In version 2.x, do not use this option or choose the default language of your project. |
| **`--template -t`** | The template name, which can be one of the  values:<br/><ul><li>`Blob trigger`</li><li>`Cosmos DB trigger`</li><li>`Event Grid trigger`</li><li>`HTTP trigger`</li><li>`Queue trigger`</li><li>`SendGrid`</li><li>`Service Bus Queue trigger`</li><li>`Service Bus Topic trigger`</li><li>`Timer trigger`</li></ul> |
| **`--name -n`** | The function name. |

For example, to create a JavaScript HTTP trigger in a single command, run:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

To create a queue-triggered function in a single command, run:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>Run functions locally

To run a Functions project, run the Functions host. The host enables triggers for all functions in the project:

```bash
func host start
```

`func host start` supports the following options:

| Option     | Description                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | The local port to listen on. Default value: 7071. |
| **`--debug <type>`** | Starts the host with the debug port open so that you can attach to the **func.exe** process from [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) or [Visual Studio 2017](functions-dotnet-class-library.md). The *\<type\>* options are `VSCode` and `VS`.  |
| **`--cors`** | A comma-separated list of CORS origins, with no spaces. |
| **`--nodeDebugPort -n`** | The port for the node debugger to use. Default: A value from launch.json or 5858. |
| **`--debugLevel -d`** | The console trace level (off, verbose, info, warning, or error). Default: Info.|
| **`--timeout -t`** | The timeout for the Functions host to start, in seconds. Default: 20 seconds.|
| **`--useHttps`** | Bind to `https://localhost:{port}` rather than to `http://localhost:{port}`. By default, this option creates a trusted certificate on your computer.|
| **`--pause-on-error`** | Pause for additional input before exiting the process. Used when launching Core Tools from Visual Studio or VS Code.|

When the Functions host starts, it outputs the URL of HTTP-triggered functions:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### Passing test data to a function

To test your functions locally, you [start the Functions host](#start) and call endpoints on the local server using HTTP requests. The endpoint you call depends on the type of function.

>[!NOTE]  
> Examples in this topic use the cURL tool to send HTTP requests from the terminal or a command prompt. You can use a tool of your choice to send HTTP requests to the local server. The cURL tool is available by default on Linux-based systems. On Windows, you must first download and install the [cURL tool](https://curl.haxx.se/).

For more general information on testing functions, see [Strategies for testing your code in Azure Functions](functions-test-a-function.md).

#### HTTP and webhook triggered functions

You call the following endpoint to locally run HTTP and webhook triggered functions:

    http://localhost:{port}/api/{function_name}

Make sure to use the same server name and port that the Functions host is listening on. You see this in the output generated when starting the Function host. You can call this URL using any HTTP method supported by the trigger. 

The following cURL command triggers the `MyHttpTrigger` quickstart function from a GET request with the _name_ parameter passed in the query string. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
The following example is the same function called from a POST request passing _name_ in the request body:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

You can make GET requests from a browser passing data in the query string. For all other HTTP methods, you must use cURL, Fiddler, Postman, or a similar HTTP testing tool.  

#### Non-HTTP triggered functions

For all kinds of functions other than HTTP triggers and webhooks, you can test your functions locally by calling an administration endpoint. Calling this endpoint with an HTTP POST request on the local server triggers the function. You can optionally pass test data to the execution in the body of the POST request. This functionality is similar to the **Test** tab in the Azure portal.  

You call the following administrator endpoint to trigger non-HTTP functions:

    http://localhost:{port}/admin/functions/{function_name}

To pass test data to the administrator endpoint of a function, you must supply the data in the body of a POST request message. The message body is required to have the following JSON format:

```JSON
{
    "input": "<trigger_input>"
}
````

The `<trigger_input>` value contains data in a format expected by the function. The following cURL example is a POST to a `QueueTriggerJS` function. In this case, the input is a string that is equivalent to the message expected to be found in the queue.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### Using the `func run` command in version 1.x

>[!IMPORTANT]  
> The `func run` command is not supported in version 2.x of the tools. For more information, see the topic [How to target Azure Functions runtime versions](set-runtime-version.md).

You can also invoke a function directly by using `func run <FunctionName>` and provide input data for the function. This command is similar to running a function using the **Test** tab in the Azure portal. 

`func run` supports the following options:

| Option     | Description                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Inline content. |
| **`--debug -d`** | Attach a debugger to the host process before running the function.|
| **`--timeout -t`** | Time to wait (in seconds) until the local Functions host is ready.|
| **`--file -f`** | The file name to use as content.|
| **`--no-interactive`** | Does not prompt for input. Useful for automation scenarios.|

For example, to call an HTTP-triggered function and pass content body, run the following command:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### Viewing log files locally

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Publish to Azure

To publish a Functions project to a function app in Azure, use the `publish` command:

```bash
func azure functionapp publish <FunctionAppName>
```

You can use the following options:

| Option     | Description                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publish settings in local.settings.json to Azure, prompting to overwrite if the setting already exists. If you are using the storage emulator, you change the app setting to an [actual storage connection](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | Must be used with `-i`. Overwrites AppSettings in Azure with local value if different. Default is prompt.|

This command publishes to an existing function app in Azure. An error occurs when the `<FunctionAppName>` doesn't exist in your subscription. To learn how to create a function app from the command prompt or terminal window using the Azure CLI, see [Create a Function App for serverless execution](./scripts/functions-cli-create-serverless.md).

The `publish` command uploads the contents of the Functions project directory. If you delete files locally, the `publish` command does not delete them from Azure. You can delete files in Azure by using the [Kudu tool](functions-how-to-use-azure-function-app-settings.md#kudu) in the [Azure portal].  

>[!IMPORTANT]  
> When you create a function app in Azure, it uses version 1.x of the Function runtime by default. To make the function app use version 2.x of the runtime, add the application setting `FUNCTIONS_EXTENSION_VERSION=beta`.  
Use the following Azure CLI code to add this setting to your function app:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## Next steps

Azure Functions Core Tools is [open source and hosted on GitHub](https://github.com/azure/azure-functions-cli).  
To file a bug or feature request, [open a GitHub issue](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
