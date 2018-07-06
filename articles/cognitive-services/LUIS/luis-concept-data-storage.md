---
title: Understand data storage in LUIS - Azure | Microsoft Docs
description: Learn how data is stored in Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2018
ms.author: v-geberr
---

# Data storage and removal
LUIS stores data encrypted in an Azure data store corresponding to the region specified by the key. This data is stored for 30 days. 

## Export and delete app
Users have full control over [exporting](create-new-app.md#export-app) and [deleting](create-new-app.md#delete-app) the app. 

## Utterances in an intent
Delete example utterances used for training [LUIS][LUIS]. If you delete an example utterance from your LUIS app, it is removed from the LUIS web service and is unavailable for export.

## Utterances in review
You can delete utterances from the list of user utterances that LUIS suggests in the **[Review endpoint utterances page](label-suggested-utterances.md)**. Deleting utterances from this list prevents them from being suggested, but doesn't delete them from logs.

## Accounts
If you delete an account, all apps are deleted, along with their example utterances and logs. The data is retained for 60 days before the account and data are deleted permanently.

Deleting account is available from the **Settings** page. Select your account name in the top right navigation bar to get to the **Settings** page.

## Data inactivity as an expired subscription
For the purposes of data retention and deletion, an inactive LUIS app may at _Microsoft’s discretion_ be treated as an expired subscription. An app is considered inactive if it meets the following criteria for the last 90 days: 

* Has had **no** calls made to it.
* Has not been modified.
* Does not have a current key assigned to it.
* Has not had a user sign in to it.

## Next steps

> [!div class="nextstepaction"]
> [Learn about exporting and deleting an app](create-new-app.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions