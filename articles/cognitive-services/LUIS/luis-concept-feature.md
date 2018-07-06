---
title: Understand features in LUIS apps in Azure | Microsoft Docs
description: Learn about features, which help improve a LUIS app's performance. Features include phrase lists and patterns for recognizing regular expressions.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/18/2018
ms.author: v-geberr
---
# Phrase list features in LUIS

In machine learning, a *feature* is a distinguishing trait or attribute of data that your system observes. 

Add features to a language model to provide hints about how to recognize input that you want to label or classify. Features help LUIS recognize both intents and entities, but features are not intents or entities themselves. Instead, features might provide examples of related terms.  

## What is a phrase list feature?
A phrase list includes a group of values (words or phrases) that belong to the same class and must be treated similarly (for example, names of cities or products). What LUIS learns about one of them is automatically applied to the others as well. This is not a closed [list entity](luis-concept-entity-types.md#types-of-entities) (exact text matches) of matched words.

A phrase list adds to the vocabulary of the app domain as a second signal to LUIS about those words.

## How to use phrase lists
In a travel agent app, create a phrase list named "Cities" that contains the values London, Paris, and Cairo. If you label one of these values as a simple entity in an [example utterance](luis-how-to-add-example-utterances.md#add-simple-entity-label) in an intent, LUIS learns to recognize the others. 

A phrase list may be interchangeable or non-interchangeable. An *interchangeable* phrase list is for values that are synonyms, and a *non-interchangeable* phrase list is intended for values that aren't synonyms but are similar in another way. 

## Phrase lists help identify simple exchangeable entities
Exchangeable phrase lists are a good way to tune the performance of your LUIS app. If your app has trouble predicting utterances to the correct intent, or recognizing entities, think about whether the utterances contain unusual words, or words that might be ambiguous in meaning. These words are good candidates to include in a phrase list.

## Phrase lists help identify intents by better understanding context
Use phrase lists for rare, proprietary, and foreign words. LUIS may be unable to recognize rare and proprietary words, as well as foreign words (outside of the culture of the app), and therefore they should be added to a phrase list. This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.

A phrase list is not an instruction to LUIS to perform strict matching or always label all terms in the phrase list exactly the same. It is simply a hint. For example, you could have a phrase list that indicates that "Patti" and "Selma" are names, but LUIS can still use contextual information to recognize that they mean something different in "Make a reservation for 2 at Patti's Diner for dinner" and "Find me driving directions to Selma, Georgia". 

Adding a phrase list is an alternative to adding more example utterances to an intent. 

## When to use phrase lists versus list entities
While both a phrase list and list entities can impact utterances across all intents, each does this in a different way. Use a phrase list to affect intent prediction score. Use a list entity to affect entity extraction for an exact text match. 

### Use a phrase list
With a phrase list, LUIS can still take context into account and generalize to identify items that are similar to, but not an exact match, as items in a list. If you need your LUIS app to be able to generalize and identify new items in a category, use a phrase list. 

When you want to be able to recognize new instances of an entity, like a meeting scheduler that should recognize the names of new contacts, or an inventory app that should recognize new products, use another type of machine-learned entity such as a simple or hierarchical entity. Then create a phrase list of words and phrases that helps LUIS find other words similar to the entity. This list guides LUIS to recognize examples of the entity by adding additional significance to the value of those words. 

Phrase lists are like domain-specific vocabulary that help with enhancing the quality of understanding of both intents and entities. A common usage of a phrase list is proper nouns such as city names. A city name can be several words including hyphens, or apostrophes.
 
### Don't use a phrase list 
A list entity explicitly defines every value an entity can take, and only identifies values that match exactly. A list entity may be appropriate for an app in which all instances of an entity are known and don't change often, like the food items on a restaurant menu that changes infrequently. If you need an exact text match of an entity, do not use a phrase list. 

## Best practices
Learn [best practices](luis-concept-best-practices.md).

## Next steps

See [Add Features](luis-how-to-add-features.md) to learn more about how to add features to your LUIS app.
