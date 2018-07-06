---
title: Export and deletion of customer data - LUIS - Azure Cognitive Services | | Microsoft Docs
description: Reference for export and deletion of customer data from Language Understanding service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb 

ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
---

# Export and delete your customer data in Language Understanding (LUIS) in Cognitive Services

## Summary of customer data request features​
Language Understanding Intelligent Service (LUIS) preserves customer content to operate the service, but the LUIS user has full control over viewing, exporting, and deleting their data. This can be done through the LUIS web [portal](luis-reference-regions.md) or the [LUIS Programmatic APIs](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Customer content is stored encrypted in Microsoft regional Azure storage and includes:

- User account content collected at registration
- Training data required to build the models (i.e. intent & entities)
- User queries logged at runtime to help improve the user models
  - Users can turn off query logging by appending `&log=false` to the request, details [here](luis-resources-faq.md#how-can-i-disable-the-logging-of-utterances)

## Deleting customer data
LUIS users have full control to delete any user content, either through the LUIS web portal or the LUIS Programmatic APIs. The following table displays links assisting with both:

| | **User Account** | **Application** | **Utterance(s)** | **End-user queries** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](luis-how-to-account-settings.md) | [Link](create-new-app.md#delete-app) | [Link](create-new-app.md#delete-app) | [Link](create-new-app.md#delete-app) |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## Exporting customer data
LUIS users have full control to view the data on the portal, however it must be exported through the LUIS Programmatic APIs. The following table displays links assisting with data exports via the LUIS Programmatic APIs:

| | **User Account** | **Application** | **Utterance(s)** | **End-user queries** |
| --- | --- | --- | --- | --- |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## Next steps

> [!div class="nextstepaction"]
> [LUIS regions reference](./luis-reference-regions.md)
