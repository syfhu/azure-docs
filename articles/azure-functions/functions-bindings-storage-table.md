---
title: Azure Table storage bindings for Azure Functions
description: Understand how to use Azure Table storage bindings in Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: azure functions, functions, event processing, dynamic compute, serverless architecture

ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
---
# Azure Table storage bindings for Azure Functions

This article explains how to work with Azure Table storage bindings in Azure Functions. Azure Functions supports input and output bindings for Azure Table storage.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## Packages - Functions 1.x

The Table storage bindings are provided in the [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet package, version 2.x. Source code for the package is in the [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub repository.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## Packages - Functions 2.x

The Table storage bindings are provided in the [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet package, version 3.x. Source code for the package is in the [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub repository.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## Input

Use the Azure Table storage input binding to read a table in an Azure Storage account.

## Input - example

See the language-specific example:

* [C# read one entity](#input---c-example-1)
* [C# read multiple entities](#input---c-example-2)
* [C# script - read one entity](#input---c-script-example-1)
* [C# script - read multiple entities](#input---c-script-example-2)
* [F#](#input---f-example-2)
* [JavaScript](#input---javascript-example)

### Input - C# example 1

The following example shows a [C# function](functions-dotnet-class-library.md) that reads a single table row. 

The row key value "{queueTrigger}" indicates that the row key comes from the queue message string.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        TraceWriter log)
    {
        log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### Input - C# example 2

The following example shows a [C# function](functions-dotnet-class-library.md) that reads multiple table rows. Note that the `MyPoco` class derives from `TableEntity`.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        TraceWriter log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

  > [!NOTE]
  > `IQueryable` isn't supported in the [Functions v2 runtime](functions-versions.md). An alternative is to [use a CloudTable paramName method parameter](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) to read the table by using the Azure Storage SDK. If you try to bind to `CloudTable` and get an error message, make sure that you have a reference to [the correct Storage SDK version](#azure-storage-sdk-version-in-functions-1x).

### Input - C# script example 1

The following example shows a table input binding in a *function.json* file and [C# script](functions-reference-csharp.md) code that uses the binding. The function uses a queue trigger to read a single table row. 

The *function.json* file specifies a `partitionKey` and a `rowKey`. The `rowKey` value "{queueTrigger}" indicates that the row key comes from the queue message string.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

The [configuration](#input---configuration) section explains these properties.

Here's the C# script code:

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### Input - C# script example 2

The following example shows a table input binding in a *function.json* file and [C# script](functions-reference-csharp.md) code that uses the binding. The function reads entities for a partition key that is specified in a queue message.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

The [configuration](#input---configuration) section explains these properties.

The C# script code adds a reference to the Azure Storage SDK so that the entity type can derive from `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### Input - F# example

The following example shows a table input binding in a *function.json* file and [F# script](functions-reference-fsharp.md) code that uses the binding. The function uses a queue trigger to read a single table row. 

The *function.json* file specifies a `partitionKey` and a `rowKey`. The `rowKey` value "{queueTrigger}" indicates that the row key comes from the queue message string.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

The [configuration](#input---configuration) section explains these properties.

Here's the F# code:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

### Input - JavaScript example

The following example shows a  table input binding in a *function.json* file and [JavaScript code] (functions-reference-node.md) that uses the binding. The function uses a queue trigger to read a single table row. 

The *function.json* file specifies a `partitionKey` and a `rowKey`. The `rowKey` value "{queueTrigger}" indicates that the row key comes from the queue message string.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

The [configuration](#input---configuration) section explains these properties.

Here's the JavaScript code:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

## Input - attributes
 
In [C# class libraries](functions-dotnet-class-library.md), use the following attributes to configure a table input binding:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs)

  The attribute's constructor takes the table name, partition key, and row key. It can be used on an out parameter or on the return value of the function, as shown in the following example:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      TraceWriter log)
  {
      ...
  }
  ```

  You can set the `Connection` property to specify the storage account to use, as shown in the following example:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      TraceWriter log)
  {
      ...
  }
  ```

  For a complete example, see [Input - C# example](#input---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Provides another way to specify the storage account to use. The constructor takes the name of an app setting that contains a storage connection string. The attribute can be applied at the parameter, method, or class level. The following example shows class level and method level:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

The storage account to use is determined in the following order:

* The `Table` attribute's `Connection` property.
* The `StorageAccount` attribute applied to the same parameter as the `Table` attribute.
* The `StorageAccount` attribute applied to the function.
* The `StorageAccount` attribute applied to the class.
* The default storage account for the function app ("AzureWebJobsStorage" app setting).

## Input - configuration

The following table explains the binding configuration properties that you set in the *function.json* file and the `Table` attribute.

|function.json property | Attribute property |Description|
|---------|---------|----------------------|
|**type** | n/a | Must be set to `table`. This property is set automatically when you create the binding in the Azure portal.|
|**direction** | n/a | Must be set to `in`. This property is set automatically when you create the binding in the Azure portal. |
|**name** | n/a | The name of the variable that represents the table or entity in function code. | 
|**tableName** | **TableName** | The name of the table.| 
|**partitionKey** | **PartitionKey** |Optional. The partition key of the table entity to read. See the [usage](#input---usage) section for guidance on how to use this property.| 
|**rowKey** |**RowKey** | Optional. The row key of the table entity to read. See the [usage](#input---usage) section for guidance on how to use this property.| 
|**take** |**Take** | Optional. The maximum number of entities to read in JavaScript. See the [usage](#input---usage) section for guidance on how to use this property.| 
|**filter** |**Filter** | Optional. An OData filter expression for table input in JavaScript. See the [usage](#input---usage) section for guidance on how to use this property.| 
|**connection** |**Connection** | The name of an app setting that contains the Storage connection string to use for this binding. If the app setting name begins with "AzureWebJobs", you can specify only the remainder of the name here. For example, if you set `connection` to "MyStorage", the Functions runtime looks for an app setting that is named "AzureWebJobsMyStorage." If you leave `connection` empty, the Functions runtime uses the default Storage connection string in the app setting that is named `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## Input - usage

The Table storage input binding supports the following scenarios:

* **Read one row in C# or C# script**

  Set `partitionKey` and `rowKey`. Access the table data by using a method parameter `T <paramName>`. In C# script, `paramName` is the value specified in the `name` property of *function.json*. `T` is typically a type that implements `ITableEntity` or derives from `TableEntity`. The `filter` and `take` properties are not used in this scenario. 

* **Read one or more rows in C# or C# script**

  Access the table data by using a method parameter `IQueryable<T> <paramName>`. In C# script, `paramName` is the value specified in the `name` property of *function.json*. `T` must be a type that implements `ITableEntity` or derives from `TableEntity`. You can use `IQueryable` methods to do any filtering required. The `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario.  

  > [!NOTE]
  > `IQueryable` isn't supported in the [Functions v2 runtime](functions-versions.md). An alternative is to [use a CloudTable paramName method parameter](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) to read the table by using the Azure Storage SDK. If you try to bind to `CloudTable` and get an error message, make sure that you have a reference to [the correct Storage SDK version](#azure-storage-sdk-version-in-functions-1x).

* **Read one or more rows in JavaScript**

  Set the `filter` and `take` properties. Don't set `partitionKey` or `rowKey`. Access the input table entity (or entities) using `context.bindings.<name>`. The deserialized objects have `RowKey` and `PartitionKey` properties.

## Output

Use an Azure Table storage output binding to write entities to a table in an Azure Storage account.

> [!NOTE]
> This output binding does not support updating existing entities. Use the `TableOperation.Replace` operation [from the Azure Storage SDK](https://docs.microsoft.com/azure/cosmos-db/table-storage-how-to-use-dotnet#replace-an-entity) to update an existing entity.   

## Output - example

See the language-specific example:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### Output - C# example

The following example shows a [C# function](functions-dotnet-class-library.md) that uses an HTTP trigger to write a single table row. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### Output - C# script example

The following example shows a table output binding in a *function.json* file and [C# script](functions-reference-csharp.md) code that uses the binding. The function writes multiple table entities.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

The [configuration](#output---configuration) section explains these properties.

Here's the C# script code:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### Output - F# example

The following example shows a table output binding in a *function.json* file and [F# script](functions-reference-fsharp.md) code that uses the binding. The function writes multiple table entities.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

The [configuration](#output---configuration) section explains these properties.

Here's the F# code:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### Output - JavaScript example

The following example shows a  table output binding in a *function.json* file and a [JavaScript function](functions-reference-node.md) that uses the binding. The function writes multiple table entities.

Here's the *function.json* file:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

The [configuration](#output---configuration) section explains these properties.

Here's the JavaScript code:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## Output - attributes

In [C# class libraries](functions-dotnet-class-library.md), use the [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs).

The attribute's constructor takes the table name. It can be used on an `out` parameter or on the return value of the function, as shown in the following example:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
{
    ...
}
```

You can set the `Connection` property to specify the storage account to use, as shown in the following example:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
{
    ...
}
```

For a complete example, see [Output - C# example](#output---c-example).

You can use the `StorageAccount` attribute to specify the storage account at class, method, or parameter level. For more information, see [Input - attributes](#input---attributes).

## Output - configuration

The following table explains the binding configuration properties that you set in the *function.json* file and the `Table` attribute.

|function.json property | Attribute property |Description|
|---------|---------|----------------------|
|**type** | n/a | Must be set to `table`. This property is set automatically when you create the binding in the Azure portal.|
|**direction** | n/a | Must be set to `out`. This property is set automatically when you create the binding in the Azure portal. |
|**name** | n/a | The variable name used in function code that represents the table or entity. Set to `$return` to reference the function return value.| 
|**tableName** |**TableName** | The name of the table.| 
|**partitionKey** |**PartitionKey** | The partition key of the table entity to write. See the [usage section](#output---usage) for guidance on how to use this property.| 
|**rowKey** |**RowKey** | The row key of the table entity to write. See the [usage section](#output---usage) for guidance on how to use this property.| 
|**connection** |**Connection** | The name of an app setting that contains the Storage connection string to use for this binding. If the app setting name begins with "AzureWebJobs", you can specify only the remainder of the name here. For example, if you set `connection` to "MyStorage", the Functions runtime looks for an app setting that is named "AzureWebJobsMyStorage." If you leave `connection` empty, the Functions runtime uses the default Storage connection string in the app setting that is named `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## Output - usage

The Table storage output binding supports the following scenarios:

* **Write one row in any language**

  In C# and C# script, access the output table entity by using a method parameter such as `out T paramName` or the function return value. In C# script, `paramName` is the value specified in the `name` property of *function.json*. `T` can be any serializable type if the partition key and row key are provided by the *function.json* file or the `Table` attribute. Otherwise, `T` must be a type that includes `PartitionKey` and `RowKey` properties. In this scenario, `T` typically implements `ITableEntity` or derives from `TableEntity`, but it doesn't have to.

* **Write one or more rows in C# or C#**

  In C# and C# script, access the output table entity by using a method parameter `ICollector<T> paramName` or `IAsyncCollector<T> paramName`. In C# script, `paramName` is the value specified in the `name` property of *function.json*. `T` specifies the schema of the entities you want to add. Typically, `T` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to. The partition key and row key values in *function.json* or the `Table` attribute constructor are not used in this scenario.

  An alternative is to use a `CloudTable paramName` method parameter to write to the table by using the Azure Storage SDK. If you try to bind to `CloudTable` and get an error message, make sure that you have a reference to [the correct Storage SDK version](#azure-storage-sdk-version-in-functions-1x).

* **Write one or more rows in JavaScript**

  In JavaScript functions, access the table output using `context.bindings.<name>`.

## Exceptions and return codes

| Binding | Reference |
|---|---|
| Table | [Table Error Codes](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Blob, Table, Queue | [Storage Error Codes](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, Table, Queue | [Troubleshooting](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## Next steps

> [!div class="nextstepaction"]
> [Learn more about Azure functions triggers and bindings](functions-triggers-bindings.md)
