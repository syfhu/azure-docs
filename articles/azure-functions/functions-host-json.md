---
title: host.json reference for Azure Functions
description: Reference documentation for the Azure Functions host.json file.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords:
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/12/2018
ms.author: tdykstra
---

# host.json reference for Azure Functions

The *host.json* metadata file contains global configuration options that affect all functions for a function app. This article lists the settings that are available. The JSON schema is at http://json.schemastore.org/host.

There are other global configuration options in [app settings](functions-app-settings.md) and in the [local.settings.json](functions-run-local.md#local-settings-file) file.

## Sample host.json file

The following sample *host.json* file has all possible options specified.

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

The following sections of this article explain each top-level property. All are optional unless otherwise indicated.

## aggregator

Specifies how many function invocations are aggregated when [calculating metrics for Application Insights](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Property |Default  | Description |
|---------|---------|---------| 
|batchSize|1000|Maximum number of requests to aggregate.| 
|flushTimeout|00:00:30|Maximum time period to aggregate.| 

Function invocations are aggregated when the first of the two limits are reached.

## applicationInsights

Controls the [sampling feature in Application Insights](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Property  |Default | Description |
|---------|---------|---------| 
|isEnabled|true|Enables or disables sampling.| 
|maxTelemetryItemsPerSecond|5|The threshold at which sampling begins.| 

## durableTask

Configuration settings for [Durable Functions](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Task hub names must start with a letter and consist of only letters and numbers. If not specified, the default task hub name for a function app is **DurableFunctionsHub**. For  more information, see [Task hubs](durable-functions-task-hubs.md).

|Property  |Default | Description |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternate [task hub](durable-functions-task-hubs.md) names can be used to isolate multiple Durable Functions applications from each other, even if they are using the same storage backend.|
|ControlQueueBatchSize|32|The number of messages to pull from the control queue at a time.|
|PartitionCount |4|The partition count for the control queue. May be a positive integer between 1 and 16.|
|ControlQueueVisibilityTimeout |5 minutes|The visibility timeout of dequeued control queue messages.|
|WorkItemQueueVisibilityTimeout |5 minutes|The visibility timeout of dequeued work item  queue messages.|
|MaxConcurrentActivityFunctions |10X the number of processors on the current machine|The maximum number of activity functions that can be processed concurrently on a single host instance.|
|MaxConcurrentOrchestratorFunctions |10X the number of processors on the current machine|The maximum number of activity functions that can be processed concurrently on a single host instance.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|The name of the app setting that has the Azure Storage connection string used to manage the underlying Azure Storage resources.|
|TraceInputsAndOutputs |false|A value indicating whether to trace the inputs and outputs of function calls. The default behavior when tracing function execution events is to include the number of bytes in the serialized inputs and outputs for function calls. This provides minimal information about what the inputs and outputs look like without bloating the logs or inadvertently exposing sensitive information to the logs. Setting this property to true causes the default function logging to log the entire contents of function inputs and outputs.|
|LogReplayEvents|false|A value indicating whether to write orchestration replay events to Application Insights.|
|EventGridTopicEndpoint ||The URL of an Azure Event Grid custom topic endpoint. When this property is set, orchestration life cycle notification events are published to this endpoint. This property supports App Settings resolution.|
|EventGridKeySettingName ||The name of the app setting containing the key used for authenticating with the Azure Event Grid custom topic at `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|The number of times to retry if publishing to the Event Grid Topic fails.|
|EventGridPublishRetryInterval|5 minutes|The Event Grid publish retry interval in the *hh:mm:ss* format.|

Many of these are for optimizing performance. For more information, see [Performance and scale](durable-functions-perf-and-scale.md).

## eventHub

Configuration settings for [Event Hub triggers and bindings](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## functions

A list of functions that the job host will run. An empty array means run all functions. Intended for use only when [running locally](functions-run-local.md). In function apps, use the *function.json* `disabled` property rather than this property in *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## functionTimeout

Indicates the timeout duration for all functions. In Consumption plans, the valid range is from 1 second to 10 minutes, and the default value is 5 minutes. In App Service plans, there is no limit and the default value is null, which indicates no timeout.

```json
{
    "functionTimeout": "00:05:00"
}
```

## healthMonitor

Configuration settings for [Host health monitor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Property  |Default | Description |
|---------|---------|---------| 
|enabled|true|Whether the feature is enabled. | 
|healthCheckInterval|10 seconds|The time interval between the periodic background health checks. | 
|healthCheckWindow|2 minutes|A sliding time window used in conjunction with the `healthCheckThreshold` setting.| 
|healthCheckThreshold|6|Maximum number of times the health check can fail before a host recycle is initiated.| 
|counterThreshold|0.80|The threshold at which a performance counter will be considered unhealthy.| 

## http

Configuration settings for [http triggers and bindings](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## id

The unique ID for a job host. Can be a lower case GUID with dashes removed. Required when running locally. When running in Azure Functions, an ID is generated automatically if `id` is omitted.

If you share a Storage account across multiple function apps, make sure that each function app has a different `id`. You can omit the `id` property or manually set each function app's `id` to a different value. The timer trigger uses a storage lock to ensure that there will be only one timer instance when a function app scales out to multiple instances. If two function apps share the same `id` and each uses a timer trigger, only one timer will run.


```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## logger

Controls filtering for logs written by an [ILogger object](functions-monitoring.md#write-logs-in-c-functions) or by [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Property  |Default | Description |
|---------|---------|---------| 
|categoryFilter|n/a|Specifies filtering by category| 
|defaultLevel|Information|For any categories not specified in the `categoryLevels` array, send logs at this level and above to Application Insights.| 
|categoryLevels|n/a|An array of categories that specifies the minimum log level to send to Application Insights for each category. The category specified here controls all categories that begin with the same value, and longer values take precedence. In the preceding sample *host.json* file, all categories that begin with "Host.Aggregator" log at `Information` level. All other categories that begin with "Host", such as "Host.Executor", log at `Error` level.| 

## queues

Configuration settings for [Storage queue triggers and bindings](functions-bindings-storage-queue.md).

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## serviceBus

Configuration setting for [Service Bus triggers and bindings](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## singleton

Configuration settings for Singleton lock behavior. For more information, see [GitHub issue about singleton support](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Property  |Default | Description |
|---------|---------|---------| 
|lockPeriod|00:00:15|The period that function level locks are taken for. The locks auto-renew.| 
|listenerLockPeriod|00:01:00|The period that listener locks are taken for.| 
|listenerLockRecoveryPollingInterval|00:01:00|The time interval used for listener lock recovery if a listener lock couldn't be acquired on startup.| 
|lockAcquisitionTimeout|00:01:00|The maximum amount of time the runtime will try to acquire a lock.| 
|lockAcquisitionPollingInterval|n/a|The interval between lock acquisition attempts.| 

## tracing

Configuration settings for logs that you create by using a `TraceWriter` object. See [C# Logging](functions-reference-csharp.md#logging) and [Node.js Logging](functions-reference-node.md#writing-trace-output-to-the-console). 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Property  |Default | Description |
|---------|---------|---------| 
|consoleLevel|info|The tracing level for console logging. Options are: `off`, `error`, `warning`, `info`, and `verbose`.|
|fileLoggingMode|debugOnly|The tracing level for file logging. Options are `never`, `always`, `debugOnly`.| 

## watchDirectories

A set of [shared code directories](functions-reference-csharp.md#watched-directories) that should be monitored for changes.  Ensures that when code in these directories is changed, the changes are picked up by your functions.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## Next steps

> [!div class="nextstepaction"]
> [Learn how to update the host.json file](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [See global settings in environment variables](functions-app-settings.md)
