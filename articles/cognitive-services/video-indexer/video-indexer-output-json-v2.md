---
title: Examine the Azure Video Indexer output produced by v2 API | Microsoft Docs
description: This topic examines the Video Indexer output produced by v2 API.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler

ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
---

# Examine the Video Indexer output produced by v2 API

> [!Note]
> The Video Indexer V1 APIs are now deprecated, and will be removed on August 1st, 2018. You should start using the Video Indexer v2 APIs to avoid disruptions.
>
> To develop with Video Indexer v2 APIs, please refer to the instructions found [here](https://api-portal.videoindexer.ai/). 

When you call the **Get Video Index** API and the response status is OK, you get a detailed JSON output as the response content. The JSON content contains details of the specified video insights. The insights include dimensions like: transcripts, ocrs, faces, topics, blocks, etc. The dimensions have instances of time ranges that show when each dimension appeared in the video.  

You can also visually examine the video's summarized insights by pressing the **Play** button on the video in the Video Indexer portal. For more information, see [View and edit video insights](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

This article examines the JSON content returned by the  **Get Video Index** API. 

> [!NOTE]
> Expiration of all the access tokens in Video Indexer is one hour.


## Root elements

|Name|Description|
|---|---|
|accountId|The playlist's VI account ID.|
|id|The playlist's ID.|
|name|The playlist's name.|
|description|The playlist's description.|
|userName|The name of the user who created the playlist.|
|created|The playlist's creation time.|
|privacyMode|The playlist’s privacy mode (Private/Public).|
|state|The playlist’s (uploaded, processing, processed, failed, quarantined).|
|isOwned|Indicates whether the playlist was created by the current user.|
|isEditable|Indicates whether the current user is authorized to edit the playlist.|
|isBase|Indicates whether the playlist is a base playlist (a video) or a playlist made of other videos (derived).|
|durationInSeconds|The total duration of the playlist.|
|summarizedInsights|Contains one [summarizedInsights](#summarizedinsights).
|videos|A list of [videos](#videos) constructing the playlist.<br/>If this playlist of constructed of time ranges of other videos (derived), the videos in this list will contain only data from the included time ranges.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## summarizedInsights

This section shows the summary of the insights.

|Attribute | Description|
|---|---|
|name|The name of the video. For example, Azure Monitor.|
|shortId|The ID of the video. For example, 63c6d532ff.|
|privacyMode|Your breakdown can have one of the following modes: **Private**, **Public**. **Public** - the video is visible to everyone in your account and anyone that has a link to the video. **Private** - the video is visible to everyone in your account.|
|duration|Contains one duration that describes the time an insight occurred. Duration is in seconds.|
|thumbnailUrl|The video's thumbnail full URL. For example, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Notice that if the video is private, the URL contains a one hour access token. After one hour, the URL will no longer be valid and you will need to either get the breakdown again with a new url in it, or call GetAccessToken to get a new access token and construct the full url manually ('https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]').|
|faces|May contain one or more faces. For more information, see [faces](#faces).|
|topics|May contain one or more topics. For more information, see [topics](#topics).|
|sentiments|May contain one or more sentiments. For more information, see [sentiments](#sentiments).|
|audioEffects| May contain one or more audioEffects. For more information, see [audioEffects](#audioeffects).|
|brands| May contain zero or more brands. For more information, see [brands](#brands).|
|statistics | For more information, see [statistics](#statistics).|

### statistics

|Name|Description|
|---|---|
|CorrespondenceCount|Number of correspondences in the video.|
|WordCount|The number of words per speaker.|
|SpeakerNumberOfFragments|The amount of fragments the speaker has in a video.|
|SpeakerLongestMonolog|The speaker's longest monolog. If the speaker has silences inside the monolog it is included. Silence at the beginning and the end of the monolog is removed.| 
|SpeakerTalkToListenRatio|The calculation is based on the time spent on the speaker's monolog (without the silence in between) divided by the total time of the video. The time is rounded to the third decimal point.|

## videos

|Name|Description|
|---|---|
|accountId|The video’s VI account ID.|
|id|The video's ID.|
|name|The video's name.
|state|The video’s state (uploaded, processing, processed, failed, quarantined).|
|processingProgress|The processing progress during processing (for example, 20%).|
|failureCode|The failure code if failed to process (for example, 'UnsupportedFileType').|
|failureMessage|The failure message if failed to process.|
|externalId|The video's external ID (if specified by the user).|
|externalUrl|The video's external url (if specified by the user).|
|metadata|The video's external metadata (if specified by the user).|
|isAdult|Indicates whether the video was manually reviewed and identified as an adult video.|
|insights|The insights object.|
|thumbnailUrl|The video's thumbnail full URL. For example, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Notice that if the video is private, the URL contains a one hour access token. After one hour, the URL will no longer be valid and you will need to either get the breakdown again with a new url in it, or call GetAccessToken to get a new access token and construct the full url manually ('https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]').|
|publishedUrl|A url to stream the video.|
|publishedUrlProxy|A url to stream the video from (for Apple devices).|
|viewToken|A short lived view token for streaming the video.|
|sourceLanguage|The video's source language.|
|language|The video's actual language (translation).|
|indexingPreset|The preset used to index the video.|
|streamingPreset|The preset used to publish the video.|
|linguisticModelId|The CRIS model used to transcribe the video.|

```json
{
	"videos": [{
		"accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
		"id": "142a356aa6",
		"state": "Processed",
		"privacyMode": "Private",
		"processingProgress": "100%",
		"failureCode": "General",
		"failureMessage": "",
		"externalId": null,
		"externalUrl": null,
		"metadata": null,
		"insights": {. . . },
		"thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
		"publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
		"publishedProxyUrl": null,
		"viewToken": "Bearer=<token>",
		"sourceLanguage": "En-US",
		"language": "En-US",
		"indexingPreset": "Default",
		"linguisticModelId": "00000000-0000-0000-0000-000000000000"
	}],
}
```
### insights

The insights are a set of dimensions (for example, transcript lines, faces, brands, etc.), where each dimension is a list of unique elements (for example, face1, face2, face3), and each element has its own metadata and a list of its instances (which are time ranges with additional optional metadata).

A face might  have an ID, a name, a thumbnail, other metadata, and a list of its temporal instances (for example: 00:00:05 – 00:00:10, 00:01:00 - 00:02:30 and 00:41:21 – 00:41:49.) Each temporal instance can have additional metadata. For example, the face’s rectangle coordinates (20,230,60,60).

|Version|The code version|
|---|---|
|sourceLanguage|The video's source language (assuming one master language). In the form of a [BCP-47](https://tools.ietf.org/html/bcp47) string.|
|language|The insights language (translated from the source language). In the form of a [BCP-47](https://tools.ietf.org/html/bcp47) string.|
|transcript|The [transcript](#transcript) dimension.|
|ocr|The [ocr](#ocr) dimension.|
|keywords|The [keywords](#keywords) dimension.|
| blocks|May contain one or more [blocks](#blocks)|
|faces|The [faces](#faces) dimension.|
|labels|The [labels](#labels) dimension.|
|shots|The [shots](#shots) dimension.|
|brands|The [brands](#brands) dimension.|
|audioEffects|The [audioEffects](#audioEffects) dimension.|
|sentiments|The [sentiments](#sentiments) dimension.|
|visualContentModeration|The [visualContentModeration](#visualcontentmoderation) dimension.|
|textualConentModeration|The [textualConentModeration](#textualconentmoderation) dimension.|

Example:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### blocks

Attribute | Description
---|---
id|ID of the block.
lines|May contain one or more [lines](#lines)
sentimentIds|The **sentimentIds** attribute is reserved for future use.
thumbnailIds|The **thumbnailIds** attribute is reserved for future use.
sentiment|The sentiment in the block (0-1, negative to positive).
faces|May contain one or more [faces](#faces).
ocrs|May contain one or more [ocrs](#ocrs).
audioEffectInstances|May contain one or more [audioEffectInstances](#audioEffectInstances).
scenes|May contain one or more [scenes](#scenes).
annotations|May contain zero or more [annotations](#annotations).

#### transcript

|Name|Description|
|---|---|
|id|The line ID.|
|text|The transcript itself.|
|language|The transcript language. Intended to support transcript where each line can have a different language.|
|instances|A list of time ranges where this line appeared. If the instance is transcript, it will have only 1 instance.|

Example:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### ocr

|Name|Description|
|---|---|
|id|The OCR line ID.|
|text|The OCR text.|
|confidence|The recognition confidence.|
|language|The OCR language.|
|instances|A list of time ranges where this OCR appeared (the same OCR can appear multiple times).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### keywords

|Name|Description|
|---|---|
|id|The keyword ID.|
|text|The keyword text.|
|confidence|The keyword's recognition confidence.|
|language|The keyword language (when translated).|
|instances|A list of time ranges where this keyword appeared (a keyword can appear multiple times).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### faces

|Name|Description|
|---|---|
|id|The face ID.|
|name|The face name. It can be ‘Unknown #0’, an identified celebrity or a customer trained person.|
|confidence|The face identification confidence.|
|description|A description of the celebrity. |
|thumbnalId|The ID of the thumbnail of that face.|
|knownPersonId|If it is a known person, its internal ID.|
|referenceId|If it is a Bing celebrity, its Bing ID.|
|referenceType|Currently just Bing.|
|title|If it is a celebrity, its title (for example "Microsoft's CEO").|
|imageUrl|If it is a celebrity, its image url.|
|instances|These are instances of where the face appeared in the given time range. Each instance also has a thumbnailsId. |

```json
"faces": [{
	"id": 2002,
	"name": "Xam 007",
	"confidence": 0.93844,
	"description": null,
	"thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
	"knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
	"referenceId": null,
	"title": null,
	"imageUrl": null,
	"instances": [{
		"thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
		"cef03f24-b0c7-4145-94d4-a84f81bb588c"],
		"adjustedStart": "00:00:07.2400000",
		"adjustedEnd": "00:00:45.6780000",
		"start": "00:00:07.2400000",
		"end": "00:00:45.6780000"
	},
	{
		"thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
		"adjustedStart": "00:10:23.9570000",
		"adjustedEnd": "00:10:39.2390000",
		"start": "00:10:23.9570000",
		"end": "00:10:39.2390000"
	}]
}]
```

#### labels

|Name|Description|
|---|---|
|id|The label ID.|
|name|The label name (for example, 'Computer', 'TV').|
|language|The label name language (when translated). BCP-47|
|instances|A list of time ranges where this label appeared (a label can appear multiple times). Each instance has a confidence field. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### shots

|Name|Description|
|---|---|
|id|The shot ID.|
|keyFrames|A list of key frames within the shot (each has an ID and a list of instances time ranges). Key frames instances have a thumbnailId field with the keyFrame’s thumbnail ID.|
|instances|A list of time ranges of this shot (shots have only 1 instance).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
	      "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
	   "thumbnailId": "00000000-0000-0000-0000-000000000000",	
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
	      "thumbnailId": "00000000-0000-0000-0000-000000000000",	    
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
	  "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### brands

Business and product brand names detected in the speech to text transcript and/or Video OCR. This does not include visual recognition of brands or logo detection.

|Name|Description|
|---|---|
|id|The brand ID.|
|name|The brands name.|
|referenceId | The suffix of the brand wikipedia url. For example, "Target_Corporation” is the suffix of [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | The brand’s Wikipedia url, if exists. For example, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|The brands description.|
|tags|A list of predefined tags that were associated with this brand.|
|confidence|The confidence value of the Video Indexer brand detector (0-1).|
|instances|A list of time ranges of this brand. Each instance has a brandType, which indicates whether this brand appeared in the transcript or in OCR.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### audioEffects

|Name|Description|
|---|---|
|id|The audio effect ID.|
|type|The audio effect type (for example, Clapping, Speech, Silence).|
|instances|A list of time ranges where this audio effect appeared.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### sentiments

Sentiments are aggregated by their sentimentType field (Positive/Neutral/Negative). For example, 0-0.1, 0.1-0.2.

|Name|Description|
|---|---|
|id|The sentiment ID.|
|averageScore |The average of all scores of all instances of that sentiment type - Positive/Neutral/Negative|
|instances|A list of time ranges where this sentiment appeared.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### visualContentModeration

The visualContentModeration block contains time ranges which Video Indexer found to potentially have adult content. If visualContentModeration is empty, there is no adult content that was identified.

Videos that are found to contain adult or racy content might be available for private view only. Users have the option to submit a request for a human review of the content, in which case the IsAdult attribute will contain the result of the human review.

|Name|Description|
|---|---|
|id|The visual content moderation ID.|
|adultScore|The adult score (from content moderator).|
|racyScore|The racy score (from content moderation).|
|instances|A list of time ranges where this visual content moderation appeared.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### textualConentModeration 

|Name|Description|
|---|---|
|id|The textual content moderation ID.|
|bannedWordsCount |The number of banned words.|
|bannedWordsRatio |The ratio from total number of words.|


## Next steps

[Video Indexer API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

For information about how to embed widgets in your application, see [Embed Video Indexer widgets into your applications](video-indexer-embed-widgets.md). 

