---
title: Translator Text identify language from text with Go | Microsoft Docs
titleSuffix: "Microsoft Cognitive Services"
description: In this quickstart, you identify the language of the source text using the Translator Text API with Go in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar

ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: nolachar
---
# Quickstart: Identify language from text with Go

In this quickstart, you identify the language of the source text using the Translator Text API.

## Prerequisites

You'll need to install the [Go distribution](https://golang.org/doc/install) in order to run this code. The sample code uses only **core** libraries, so there are no external dependencies.

To use the Translator Text API, you also need a subscription key; see [How to sign up for the Translator Text API](translator-text-how-to-signup.md).

## Detect request

The following code identifies the language of the source text using the [Detect](./reference/v3-0-detect.md) method.

1. Create a new Go project in your favorite code editor.
2. Add the code provided below.
3. Replace the `subscriptionKey` value with an access key valid for your subscription.
4. Save the file with a '.go' extension.
5. Open a command prompt on a computer with Go installed.
6. Build the file, for example: 'go build quickstart-detect.go'.
7. Run the file, for example: 'quickstart-detect'.

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/detect?api-version=3.0"

    const uri = uriBase + uriPath

    const text = "Salve, mondo!"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## Detect response

A successful response is returned in JSON as shown in the following example:

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## Next steps

Explore Go packages for Cognitive Services APIs from the [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) on GitHub.

> [!div class="nextstepaction"]
> [Explore Go packages on GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
