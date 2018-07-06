---
title: Create event-based triggers in Azure Data Factory | Microsoft Docs
description: Learn how to create a trigger in Azure Data Factory that runs a pipeline in response to an event.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: douglasl
---
# Create a trigger that runs a pipeline in response to an event

This article describes the event-based triggers that you can create in your Data Factory pipelines.

Event-driven architecture (EDA) is a common data integration pattern that involves production, detection, consumption, and reaction to events. Data integration scenarios often require Data Factory customers to trigger pipelines based on events. Data Factory is now integrated with [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), which lets you trigger pipelines on an event.

## Data Factory UI

### Create a new event trigger

A typical event is the arrival of a file, or the deletion of a file, in your Azure Storage account. You can  create a trigger that responds to this event in your Data Factory pipeline.

> [!NOTE]
> This integration supports only version 2 Storage accounts (General purpose).

![Create new event trigger](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### Select the event trigger type

As soon as the file arrives in your storage location and the corresponding blob is created, this event triggers and runs your Data Factory pipeline. You can create a trigger that responds to a blob creation event, a blob deletion event, or both events, in your Data Factory pipelines.

![Select trigger type as event](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### Configure the event trigger

With the **Blob path begins with** and **Blob path ends with** properties, you can specify the containers, folders, and blob names for which you want to receive events. You can use variety of patterns for both **Blob path begins with** and **Blob path ends with** properties, as shown in the examples later in this article. At least one of these properties is required.

![Configure the event trigger](media/how-to-create-event-trigger/event-based-trigger-image3.png)

## JSON schema

The following table provides an overview of the schema elements that are related to event-based triggers:

| **JSON Element** | **Description** | **Type** | **Allowed Values** | **Required** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | The Azure Resource Manager resource ID of the Storage Account. | String | Azure Resource Manager ID | Yes |
| **events** | The type of events that cause this trigger to fire. | Array    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Yes, any combination. |
| **blobPathBeginsWith** | The blob path must begin with the pattern provided for trigger to fire. For example, '/records/blobs/december/' will only fire the trigger for blobs in the december folder under the records container. | String   | | At least one of these properties must be provided: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | The blob path must end with the pattern provided for trigger to fire. For example, 'december/boxes.csv' will only fire the trigger for blobs named boxes in a december folder. | String   | | At least one of these properties must be provided: blobPathBeginsWith, blobPathEndsWith. |

## Examples of event-based triggers

This section provides examples of event-based trigger settings.

-   **Blob path begins with**('/containername/') – Receives events for any blob in the container.
-   **Blob path begins with**('/containername/blobs/foldername') – Receives events for any blobs in the containername container and foldername folder.
-   **Blob path begins with**('/containername/blobs/foldername/file.txt') – Receives events for a blob named file.txt in the foldername folder under the containername container.
-   **Blob path ends with**('file.txt') – Receives events for a blob named file.txt at any path.
-   **Blob path ends with**('/containername/blobs/file.txt') – Receives events for a blob named file.txt under container containername.
-   **Blob path ends with**('foldername/file.txt') – Receives events for a blob named file.txt in foldername folder under any container.

> [!NOTE]
> You have to include the `/blobs/` segment of the path whenever you specify container and folder, container and file, or container, folder, and file.

## Using Blob Events Trigger Properties

When a blob events trigger fires, it makes two variables available to your pipeline: *folderPath* and *fileName*. To access these variables, use the `@triggerBody().fileName` or `@triggerBody().folderPath` expressions.

For example, consider a trigger configured to fire when a blob is created with `.csv` as the value of `blobPathEndsWith`. When a .csv file is dropped into the storage account, the *folderPath* and *fileName* describe the location of the .csv file. For example, *folderPath* has the value `/containername/foldername/nestedfoldername` and *fileName* has the value `filename.csv`.

## Next steps
For detailed information about triggers, see [Pipeline execution and triggers](concepts-pipeline-execution-triggers.md#triggers).
