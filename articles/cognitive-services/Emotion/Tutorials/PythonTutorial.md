﻿---
title: Emotion API Python tutorial | Microsoft Docs
description: Use a Jupyter notebook to learn how to use the Cognitive Services Emotion API with Python. Visualize your results by using popular libraries.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
---

# Emotion API using Python Tutorial

> [!IMPORTANT]
> Video API Preview will end on October 30th, 2017. Try the new [Video Indexer API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) to easily extract insights from 
videos and to enhance content discovery experiences, such as search results, by detecting spoken words, faces, characters, and emotions. [Learn more](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

To make it easy to get started with Emotion API, the Jupyter notebook linked below shows you how to use the API in Python and how to visualize your results using some popular libraries. 

[Link to notebook in GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### Using the Jupyter Notebook

To use the notebook interactively, you will need to clone it and run it in Jupyter. To learn how to get started with interactive Jupyter notebooks, follow the instructions at http://jupyter.readthedocs.org/en/latest/install.html. 

To use this notebook, you will need a subscription key for the Emotion API. Visit the [Subscription page](https://azure.microsoft.com/try/cognitive-services/) to sign up. On the “Sign in” page, use your Microsoft account to sign in and you will be able to subscribe and get free keys. After completing the sign-up process, paste your key into the variables section shown below. Either the primary or the secondary key works.

```
Python Example 

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
