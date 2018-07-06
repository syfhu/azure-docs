---
title: Manage delivery settings for Azure Event Grid subscriptions
description: Describes how to customize event delivery options for Event Grid.
services: event-grid
author: tfitzmac
manager: timlt

ms.service: event-grid
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: tomfitz
---

# Manage Event Grid delivery settings

When creating an event subscription, you can customize the settings for event delivery. You can set how long Event Grid tries to deliver the message. You can set a storage account to use for storing events that can't be delivered to an endpoint.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## Set retry policy

When creating an Event Grid subscription, you can set values for how long Event Grid should try to deliver the event. By default, Event Grid attempts for 24 hours (1440 minutes), and tries a maximum of 30 times. You can set either of these values for your event grid subscription.

To set the event time-to-live to a value other than 1440 minutes, use:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --event-ttl 720
```

To set the maximum retry attempts to a value other than 30, use:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --max-delivery-attempts 18
```

If you set both `event-ttl` and `max-deliver-attempts`, Event Grid uses the first to expire for retry attempts.

## Set dead-letter location

When Event Grid can't deliver an event, it can send the undelivered event to a storage account. This process is known as dead-lettering. By default, Event Grid doesn't turn on dead-lettering. To enable it, you must specify a storage account to hold undelivered events when creating the event subscription. You pull events from this storage account to resolve deliveries.

Event Grid sends an event to the dead-letter location if it has tried all of its retry attempts, or if it receives an error message that indicates delivery will never succeed. For example, if Event Grid receives an improper format error when delivering an event, it immediately sends that event to the dead-letter location.

Before setting the deadletter location, you must have a storage account with a container. You provide the endpoint for this container when creating the event subscription. The endpoint is in the format of:
`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

The following script gets the resource ID of an existing storage account, and creates an event subscription that uses a container in that storage account for the dead-letter endpoint.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

To use Event Grid to respond to undelivered events, [create an event subscription](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) for the dead-letter blob storage. Every time your dead-letter blob storage receives an undelivered event, Event Grid notifies your handler. The handler responds with actions you wish to take for reconciling undelivered events. 

## Next steps

* For information about event delivery and retries, [Event Grid message delivery and retry](delivery-and-retry.md).
* For an introduction to Event Grid, see [About Event Grid](overview.md).
* To quickly get started using Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).
