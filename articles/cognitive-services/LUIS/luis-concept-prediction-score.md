---
title: Understand the prediction score returned by LUIS - Azure | Microsoft Docs
description: Learn what the prediction score means in LUIS
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
---

# Prediction score
A prediction score indicates the degree of confidence LUIS has for prediction results. 

A prediction score is usually between zero (0) and one (1). An example of a highly confident LUIS score is 0.99. An example of a score of low confidence is 0.01. 

|Score value|Confidence|
|--|--|
|1|definite match|
|0.99|high confidence|
|0.01|low confidence|
|0|definite failure to match|

When an utterance results in a low-confidence score, LUIS highlights that in the [LUIS][LUIS] website **Intent** page, with the identified **labeled-intent** outlined with red. 

![Score discrepancy](./media/luis-concept-score/score-discrepancy.png)

## Top-scoring intent
Every utterance prediction returns a top-scoring intent. This is a numerical comparison of prediction scores. The top two scores can have a very small difference between them. LUIS doesn't indicate this proximity other than returning scores.  

If you are concerned about proximity of top scores, you should return the score for all intents. You can either add utterances to the two intents that indicate their differences with word choice and arrangement or you can have the LUIS-calling application, such as a chatbot, make programmatic choices about how to handle the two top intents. 

## Return prediction score for all intents
A test or endpoint result can include all intents. This configuration is set on the [endpoint](https://aka.ms/v1-endpoint-api-docs) with the `verbose=true` query string name/value pair. 

## Review intents with similar scores
Reviewing the score for all intents is a good way to verify that not only is the correct intent identified, but that the next identified intent's score is significantly lower consistently for utterances. 

If multiple intents have close prediction scores, based on the context of an utterance, LUIS may switch between the intents. To fix this, continue to add utterances to each intent with a wider variety of contextual differences.   

## E (exponent) notation

Prediction scores can use exponent notation, *appearing* above the 0-1 range, such as `9.910309E-07`. This score is an indication of a very **small** number.

|E notation score |Actual score|
|--|--|
|9.910309E-07|.0000009910309|

## Differences with predictions
When you train the same model in a different app, and the scores are not this same, this is because there is an element of randomness in the training. Secondly, any overlap of an utterance to more than one intent means the top intent for the same utterance can change based on training.

If your chatbot requires a specific LUIS score to indicate confidence in an intent, you should instead use the score difference between the top two intents. This provides flexibility for variations in training. 

## Next steps

See [Add entities](luis-how-to-add-entities.md) to learn more about how to add entities to your LUIS app.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions