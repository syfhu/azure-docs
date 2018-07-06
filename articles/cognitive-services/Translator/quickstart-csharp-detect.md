---
title: Translator Text identify language from text with C# | Microsoft Docs
titleSuffix: "Microsoft Cognitive Services"
description: In this quickstart, you identify the language of the source text using the Translator Text API with C# in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar

ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
---
# Quickstart: Identify language from text with C&#35;

In this quickstart, you identify the language of the source text using the Translator Text API.

## Prerequisites

You'll need [Visual Studio 2017](https://www.visualstudio.com/downloads/) to run this code on Windows. (The free Community Edition will work.)

To use the Translator Text API, you also need a subscription key; see [How to sign up for the Translator Text API](translator-text-how-to-signup.md).

## Detect request

The following code identifies the language of the source text using the [Detect](./reference/v3-0-detect.md) method.

1. Create a new C# project in your favorite IDE.
2. Add the code provided below.
3. Replace the `key` value with an access key valid for your subscription.
4. Run the program.

```csharp
using System;
using System.Net.Http;
using System.Text;
// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace TranslatorTextQuickStart
{
    class Program
    {
        static string host = "https://api.cognitive.microsofttranslator.com";
        static string path = "/detect?api-version=3.0";

        static string uri = host + path;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "Salve mondo!";

        async static void Detect()
        {
            System.Object[] body = new System.Object[] { new { Text = text } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                var result = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(responseBody), Formatting.Indented);

                Console.OutputEncoding = UnicodeEncoding.UTF8;
                Console.WriteLine(result);
            }
        }

        static void Main(string[] args)
        {
            Detect();
            Console.ReadLine();
        }
    }
}
```

## Detect response

A successful response is returned in JSON as shown in the following example:

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## Next steps

Explore the sample code for this quickstart and others, including translation and transliteration, as well as other sample Translator Text projects on GitHub.

> [!div class="nextstepaction"]
> [Explore C# examples on GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
