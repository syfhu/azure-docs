---
title: Computer Vision API Go quickstart create thumbnail | Microsoft Docs
titleSuffix: "Microsoft Cognitive Services"
description: In this quickstart, you generate a thumbnail from an image using Computer Vision with Go in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
---
# Quickstart: Generate a thumbnail with Go

In this quickstart, you generate a thumbnail from an image using Computer Vision.

## Prerequisites

To use Computer Vision, you need a subscription key; see [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md).

## Get Thumbnail request

With the [Get Thumbnail method](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), you can generate a thumbnail of an image. You specify the height and width, which can differ from the aspect ratio of the input image. Computer Vision uses smart cropping to intelligently identify the region of interest and generate cropping coordinates based on that region.

To run the sample, do the following steps:

1. Copy the following code into an editor.
1. Replace `<Subscription Key>` with your valid subscription key.
1. Change the `uriBase` value to the location where you got your subscription keys, if necessary.
1. Optionally, change the `imageUrl` value to the image you want to analyze.
1. Save the file with a `.go` extension.
1. Open a command prompt on a computer with Go installed.
1. Build the file, for example: `go build get-thumbnail.go`.
1. Run the file, for example: `get-thumbnail`.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    const subscriptionKey = "<Subscription Key>"

    // You must use the same location in your REST call as you used to get your
    // subscription keys. For example, if you got your subscription keys from
    // westus, replace "westcentralus" in the URL below with "westus".
    const uriBase =
      "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail"
    const imageUrl =
        "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

    const params = "?width=100&height=100&smartCropping=true"
    const uri = uriBase + params
    const imageUrlEnc = "{\"url\":\"" + imageUrl + "\"}"

    r := strings.NewReader(imageUrlEnc)

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, _ := http.NewRequest("POST", uri, r)

    req.Header.Add("Content-Type", "application/json")
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

    jsonFormatted, _ := json.MarshalIndent(f, "", "  ")
    fmt.Println(string(jsonFormatted))
}
```

## Get Thumbnail response

A successful response contains the thumbnail image binary. If the request fails, the response contains an error code and a message to help determine what went wrong.

## Next steps

Explore the Computer Vision APIs used to analyze an image, detect celebrities and landmarks, create a thumbnail, and extract printed and handwritten text.

> [!div class="nextstepaction"]
> [Explore Computer Vision APIs](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
