---
title: Understand LUIS app collaboration - Azure | Microsoft Docs
description: LUIS apps require a single owner and optional collaborators.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
---
# Collaborating

LUIS provides collaboration to allow a group of people to author an app.

## LUIS account
A LUIS account is associated with a single [Microsoft Live](https://login.live.com/) account. Each LUIS account is given a free [authoring key](luis-concept-keys.md#authoring-key) to use for authoring all the LUIS apps the account has access to. 

A LUIS account may have many LUIS apps.

See [Azure Active Directory tenant user](luis-how-to-account-settings.md#azure-active-directory-tenant-user) to learn more about Active Directory user accounts. 

## LUIS app owner
The account that creates an app is the owner. Each app has a single owner. The owner is listed on app **[Settings](luis-how-to-collaborate.md)**. This is the account that can delete the app. This is also the account that receives email when the endpoint quota reaches 75% of the monthly limit. 

## Transfer ownership
LUIS doesn't provide transfer of ownership, however any collaborator can export the app, and then create an app by importing it. Be aware the new app has a different App ID. The new app needs to be trained, published, and the new endpoint used.

## LUIS app collaborators
An app owner can add collaborators to an app. The owner needs to add the collaborator's email address on app **[Settings](luis-how-to-collaborate.md)**. The collaborator has full access to the app. If the collaborator deletes the app, the app is removed from the collaborator's account but remains in the owner's account. 

If you want to share multiple apps with collaborators, each app needs the collaborator's email added. 

## Managing multiple authors
The [LUIS][LUIS] website doesn't currently offer transaction-level authoring. You can allow authors to work on independent versions from a base version. Two different methods are described in the following sections.

### Manage multiple versions inside the same app
Begin by [cloning](luis-how-to-manage-versions.md#clone-a-version), from a base version, for each author. 

Each author makes changes to his own version of the app. Once each author is satisfied with the model, export the new versions to JSON files.  

Exported apps are JSON-formatted files, which can be compared for changes. Combine the files to create a single JSON file of the new version. Change the **versionId** property in the JSON to signify the new merged version. Import that version into the original app. 

This method allows you to have one active version, one stage version, and one published version. You can compare the results in the interactive testing pane across the three versions.

### Manage multiple versions as apps
[Export](luis-how-to-manage-versions.md#export-version) the base version. Each author imports the version. The person that imports the app is the owner of the version. When they are done modifying the app, export the version. 

Exported apps are JSON-formatted files, which can be compared with the base export for changes. Combine the files to create a single JSON file of the new version. Change the **versionId** property in the JSON to signify the new merged version. Import that version into the original app.

## Next steps

Understand [versioning](luis-concept-version.md) concepts. 

See [App Settings](luis-how-to-collaborate.md) to learn how to manage collaborators in your LUIS app.

See [Add email to access list](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) with the Authoring APIs.

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website