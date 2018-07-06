---
title: Transcription guidelines for Speech training | Microsoft Docs
description: Learn how to prepare text to customize acoustic and language models and voice fonts for the Speech service.
titleSuffix: "Microsoft Cognitive Services"
services: cognitive-services
author: PanosPeriorellis
manager: noellelacharite

ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/01/2018
ms.author: panosper
---

# Transcription guidelines for using Speech service

To customize **Speech to Text** or **Text to Speech**, you must provide text along with speech. Each line in the text corresponds to a single utterance. The text should match the speech as exactly as possible. The text is called a *transcript*, and you must create it in a specific format.

The Speech service normalizes the input to keep text consistent. 

This article describes both types of normalizations. The guidelines vary slightly for various languages.

## US English (en-US)

Text data should be written one utterance per line, in plain text, using only the ASCII character set.

Avoid the use of extended (Latin-1) or Unicode punctuation characters. These characters can be included inadvertently when preparing the data in a word-processing program or scraping data from web pages. Replace these characters with appropriate ASCII substitutions. For example:

| Characters to avoid | Substitution |
|----- | ----- |
| “Hello world” (open and close double quotes) | "Hello world" (double quotes) |
| John’s day (right single quotation mark) | John's day (apostrophe) |
| it was good—no, it was great! (em dash) | it was good--no, it was great! (hyphens) |

### Text normalization rules for English

The Speech service carries out the following normalization rules.

*   Lower-casing all text
*   Removing all punctuation except word-internal apostrophes
*   Expansion of numbers to spoken form, including dollar amounts

Here are some examples

| Original Text | After Normalization |
|----- | ----- |
| "Holy cow!" said Batman. | holy cow said batman |
| "What?" said Batman's sidekick, Robin. | what said batman's sidekick robin |
| Go get -em! | go get em |
| I'm double-jointed | I'm double jointed |
| 104 Elm Street | one oh four Elm street |
| Tune to 102.7 | tune to one oh two seven |
| Pi is about 3.14 | pi is about three point one four |
| It costs $3.14 | it costs three fourteen |

Apply the following normalization to your text transcripts.

*   Abbreviations should be written out in words
*   Non-standard numeric strings (such as some date or accounting forms) should be written out in words
*   Words with non-alphabetic characters or mixed alphanumeric characters should be transcribed as pronounced
*   Leave abbreviations pronounced as words untouched. For example, radar, laser, RAM, NATO.
*   Write abbreviations pronounced as separate letters, with letters separated by spaces. For example, IBM, CPU, FBI, TBD, NaN. 

Here are some examples:

| Original Text | After Normalization |
|----- | ----- |
| 14 NE 3rd Dr. | fourteen northeast third drive |
| Dr. Bruce Banner | Doctor Bruce Banner |
| James Bond, 007 | James Bond, double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| my blood type is O+ | My blood type is O positive |
| water is H20 | water is H 2 O |
| play OU812 by Van Halen | play O U 8 1 2 by Van Halen |
| UTF-8 with BOM | U T F 8 with BOM |

## Chinese (zh-CN)

Text data uploaded to the Custom Speech Service should use UTF-8 encoding with byte-order marker. The file should be written one utterance per line.

Avoid the use of half-width punctuation characters. These characters can be included inadvertently when preparing the data in a word-processing program or scraping data from web pages. Replace them with appropriate full-width substitutions. For example:

| Characters to avoid | Substitution |
|----- | ----- |
| "你好" (open and close double quotes) | "你好" (double quotes) |
| 需要什么帮助? (question mark) | 需要什么帮助？ |

### Text normalization rules for Chinese

The Speech service carries out the following normalization rules.

*   Removing all punctuation
*   Expanding numbers to spoken form
*   Converting full-width letters to half-width letters
*   Upper-casing all English words

Here are some examples.

| Original Text | After Normalization |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

Apply the following normalization to your text before importing it.

*   Abbreviations should be written out in words (as in spoken form)
*   Write out numeric strings in spoken form.

Here are some examples.

| Original Text | After Normalization |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## Other languages

Text data uploaded to the **Speech to Text** service must use UTF-8 encoding with byte-order marker. The file should be written one utterance per line.

> [!NOTE]
> These examples use German. However, these guidelines apply to all languages that are not US English or Chinese.

### Text normalization rules for German

The Speech service carries out the following normalization rules.

*   Lower-casing all text
*   Removing all punctuation including various types of quotes ("test", 'test', "test„ or «test» are ok)
*   Discarding rows with any special character from the set  ¢ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬
*   Expansion of numbers to word form, including dollar or euro amounts
*   Umlauts are accepted only for a, o, u; others will be replaced by "th" or be discarded

Here are some examples

| Original Text | After Normalization |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |

Apply the following normalization to your text before importing it.

*   Decimal point should be "," and not "."
*   Time separator between hours and minutes should be ":" and not ".": 12:00 Uhr
*   Abbreviations such as 'ca.' aren't replaced. We recommend you use the full form.
*   The five main mathematical operators are removed: +, -, \*, /. We recommend replacing them with their literal form: plus, minus, mal, geteilt.
*   Same applies for comparison operators (=, <, >) - gleich, kleiner als, grösser als
*   Use fractions, such as 3/4, in word form (such as 'drei viertel' instead of ¾)
*   Replace the € symbol with the word form "Euro"

Here are some examples.

| Original Text | After user's normalization | After system normalization
|--------  | ----- | -------- |
| Es ist 12.23Uhr | Es ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier|

## Next steps

- [Get your Speech trial subscription](https://azure.microsoft.com/try/cognitive-services/)
- [Recognize speech in C#](quickstart-csharp-windows.md)
