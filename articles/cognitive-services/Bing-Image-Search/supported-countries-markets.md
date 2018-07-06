---
title: Supported countries and languages for Bing Image Search API on Azure | Microsoft Docs
description: Find out which countries and languages are supported by the Bing Image Search API.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
---

# Bing Image Search countries and languages

The Bing Image Search API supports more than three dozen countries, many with more than one language. Specifying a country with a query serves primarily to refine search results based on interests in that country. Additionally, the results may contain links to Bing, and these links may localize the Bing user experience according to the specified country or language.

To specify the country and language, set the `mkt` (market) query parameter to a code from the **Markets** table below. The market specifies both a country and language. If the user prefers to see display text in a different language, set `setLang` query parameter to the appropriate language code.

Alternatively, you can specify the country using the `cc` query parameter. If you specify a country, you must also specify one or more language codes using the `Accept-Language` HTTP header. The supported languages vary by country; they are given for each country in the Markets table.

> [!NOTE]
> The Trending Images API currently supports only the following markets:
> - en-US (English, United States) 
> - en-CA (English, Canada) 
> - en-AU (English, Australia) 
> - zh-CN (Chinese, China)

## Countries

|Country|Code|
|-------|----|
|Argentina|AR|
|Australia|AU|
|Austria|AT|
|Belgium|BE|
|Brazil|BR|
|Canada|CA|
|Chile|CL|
|Denmark|DK|
|Finland|FI|
|France|FR|
|Germany|DE|
|Hong Kong|HK|
|India|IN|
|Indonesia|ID|
|Italy|IT|
|Japan|JP|
|Korea|KR|
|Malaysia|MY|
|Mexico|MX|
|Netherlands|NL|
|New Zealand|NZ|
|Norway|NO|
|China|CN|
|Poland|PL|
|Portugal|PT|
|Philippines|PH|
|Russia|RU|
|Saudi Arabia|SA|
|South Africa|ZA|
|Spain|ES|
|Sweden|SE|
|Switzerland|CH|
|Taiwan|TW|
|Turkey|TR|
|United Kingdom|GB|
|United States|US|


## Markets

|Country|Language|Market Code|
|-------|--------|-----------|
|Argentina|Spanish|es-AR|
|Australia|English|en-AU|
|Austria|German|de-AT|
|Belgium|Dutch|nl-BE|
|Belgium|French|fr-BE|
|Brazil|Portuguese|pt-BR|
|Canada|English|en-CA|
|Canada|French|fr-CA|
|Chile|Spanish|es-CL|
|Denmark|Danish|da-DK|
|Finland|Finnish|fi-FI|
|France|French|fr-FR|
|Germany|German|de-DE|
|Hong Kong|Traditional Chinese|zh-HK|
|India|English|en-IN|
|Indonesia|English|en-ID|
|Italy|Italian|it-IT|
|Japan|Japanese|ja-JP|
|Korea|Korean|ko-KR|
|Malaysia|English|en-MY|
|Mexico|Spanish|es-MX|
|Netherlands|Dutch|nl-NL|
|New Zealand|English|en-NZ|
|China|Chinese|zh-CN|
|Poland|Polish|pl-PL|
|Portugal|Portuguese|pt-PT|
|Philippines|English|en-PH|
|Russia|Russian|ru-RU|
|Saudi Arabia|Arabic|ar-SA|
|South Africa|English|en-ZA|
|Spain|Spanish|es-ES|
|Sweden|Swedish|sv-SE|
|Switzerland|French|fr-CH|
|Switzerland|German|de-CH|
|Taiwan|Traditional Chinese|zh-TW|
|Turkey|Turkish|tr-TR|
|United Kingdom|English|en-GB|
|United States|English|en-US|
|United States|Spanish|es-US|

## Next steps
For more information about the Bing News Search endpoints, see [News Image Search API v7 reference](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
