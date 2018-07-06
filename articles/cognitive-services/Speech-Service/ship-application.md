---
title: Azure Cognitive Services, Cognitive Services Speech SDK API Documentation - Tutorials, API Reference | Microsoft Docs
description: Learn how to create and develop apps with the Cognitive Services Speech SDK
titleSuffix: "Microsoft Cognitive Services"
services: cognitive-services
author: wolfma61
manager: onano

ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
---

# Shipping an application

Observe the [Speech SDK license](license.md), as well as the [third party software notices](third-party-notices.md) when distributing the Cognitive Services Speech SDK. Also, review the [Microsoft Privacy Statement](https://aka.ms/csspeech/privacy).

Depending on the platform, different dependencies exist to execute your application.

## Windows

The Cognitive Services Speech SDK is tested on Windows 10 and on Windows Server 2016.

The Cognitive Services Speech SDK requires the [Microsoft Visual C++ Redistributable for Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) on the system. You can download installers for the latest version of the `Microsoft Visual C++ Redistributable for Visual Studio 2017` here:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

If your application is using managed code, the `.Net Framework 4.6.1` or later is required on the target machine.

For microphone input, the Media Foundation Libraries need to be installed. These libraries are part of Windows 10 and Windows Server 2016. It is possible to use the Speech SDK without these libraries, as long as microphone is not used as the audio input device.

The required Speech SDK files can be deployed in the same directory as your application. This way your application can directly access the libraries. Make sure you select the correct version (Win32/x64) matching your application.

| Name | Function
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | core SDK, required for native and managed deployment
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | required for managed deployment
| `Microsoft.CognitiveServices.Speech.csharp.dll` | required for managed deployment

## Linux

For a native application you need to ship the Speech SDK library, `libMicrosoft.CognitiveServices.Speech.core.so`.
Make sure you select the version (x86, x64) matching your application. Depending on the Linux version, you may also need to include the following dependencies:

* The shared libraries of the GNU C Library (including the POSIX Threads Programming library, `libpthreads`)
* The OpenSSL library (`libssl.so.1.0.0`)
* The cURL library (`libcurl.so.4`)
* The shared library for ALSA applications (`libasound.so.2`)

On Ubuntu 16.04, for example, the GNU C libraries should already be installed by default. The last three can be installed using these commands:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## Next steps

* [Get your Speech trial subscription](https://azure.microsoft.com/try/cognitive-services/)
* [See how to recognize speech in C#](quickstart-csharp-windows.md)
