---
title: Send custom events for Azure Event Grid to hybrid connection | Microsoft Docs
description: Use Azure Event Grid and Azure CLI to publish a topic, and subscribe to that event. A hybrid connection is used for the endpoint. 
services: event-grid 
keywords: 
author: tfitzmac
ms.author: tomfitz
ms.date: 06/29/2018
ms.topic: tutorial
ms.service: event-grid
---
# Route custom events to Azure Relay Hybrid Connections with Azure CLI and Event Grid

Azure Event Grid is an eventing service for the cloud. Azure Relay Hybrid Connections is one of the supported event handlers. You use hybrid connections as the event handler when you need to process events from applications that don't have a public endpoint. These applications might be within your corporate enterprise network. In this article, you use the Azure CLI to create a custom topic, subscribe to the topic, and trigger the event to view the result. You send the events to the hybrid connection.

## Prerequisites

This article assumes you already have a hybrid connection and a listener application. To get started with hybrid connections, see [Get started with Relay Hybrid Connections - .NET](../service-bus-relay/relay-hybrid-connections-dotnet-get-started.md) or [Get started with Relay Hybrid Connections - Node](../service-bus-relay/relay-hybrid-connections-node-get-started.md).

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## Create a resource group

Event Grid topics are Azure resources, and must be placed in an Azure resource group. The resource group is a logical collection into which Azure resources are deployed and managed.

Create a resource group with the [az group create](/cli/azure/group#az_group_create) command. 

The following example creates a resource group named *gridResourceGroup* in the *westus2* location.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## Create a custom topic

An event grid topic provides a user-defined endpoint that you post your events to. The following example creates the custom topic in your resource group. Replace `<topic_name>` with a unique name for your topic. The topic name must be unique because it's represented by a DNS entry.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## Subscribe to a topic

You subscribe to a topic to tell Event Grid which events you want to track. The following example subscribes to the topic you created, and passes the resource ID of the hybrid connection for the endpoint. The hybrid connection ID is in the format:

`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Relay/namespaces/<relay-namespace>/hybridConnections/<hybrid-connection-name>`

The following script gets the resource ID of the relay namespace. It constructs the ID for the hybrid connection, and subscribes to an event grid topic. The script sets the endpoint type to `hybridconnection` and uses the hybrid connection ID for the endpoint.

```azurecli-interactive
relayname=<namespace-name>
relayrg=<resource-group-for-relay>
hybridname=<hybrid-name>

relayid=$(az resource show --name $relayname --resource-group $relayrg --resource-type Microsoft.Relay/namespaces --query id --output tsv)
hybridid="$relayid/hybridConnections/$hybridname"

az eventgrid event-subscription create \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --name <event_subscription_name> \
  --endpoint-type hybridconnection \
  --endpoint $hybridid
```

## Create application to process events

You need an application that can retrieve events from the hybrid connection. The [Microsoft Azure Event Grid Hybrid Connection Consumer sample for C#](https://github.com/Azure-Samples/event-grid-dotnet-hybridconnection-destination) performs that operation. You've already finished the prerequisite steps.

1. Make sure you have Visual Studio 2017 Version 15.5 or later.

1. Clone the repository to your local machine.

1. Load HybridConnectionConsumer project in Visual Studio.

1. In Program.cs, replace `<relayConnectionString>` and `<hybridConnectionName>` with the relay connection string and hybrid connection name that you created.

1. Compile and run the application from Visual Studio.

## Send an event to your topic

Let's trigger an event to see how Event Grid distributes the message to your endpoint. This article shows how to use Azure CLI to trigger the event. Alternatively, you can use [Event Grid publisher application](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridPublisher).

First, let's get the URL and key for the custom topic. Again, use your topic name for `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

To simplify this article, you use sample event data to send to the topic. Typically, an application or Azure service would send the event data. CURL is a utility that sends HTTP requests. In this article, use CURL to send the event to the topic.  The following example sends three events to the event grid topic:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Your listener application should receive the event message.

## Clean up resources
If you plan to continue working with this event, don't clean up the resources created in this article. Otherwise, use the following command to delete the resources you created in this article.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## Next steps

Now that you know how to create topics and event subscriptions, learn more about what Event Grid can help you do:

- [About Event Grid](overview.md)
- [Route Blob storage events to a custom web endpoint](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Stream big data into a data warehouse](event-grid-event-hubs-integration.md)
