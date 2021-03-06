---
title: Stream кодек сжатия аудио с Speech SDK - службы распознавания речи
titleSuffix: Azure Cognitive Services
description: Узнайте, как выполнять потоковую передачу сжатые аудио для служб Azure речи с помощью пакета SDK для распознавания речи. Для C++, C#и Java для Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: amishu
ms.openlocfilehash: 41a55eca321cbe1bfa23a889b8e3ce7c701ce769
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2019
ms.locfileid: "65468053"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>С помощью кодека сжатия аудио вход с помощью пакета SDK для распознавания речи

Пакет SDK для распознавания речи **сжатые аудио входных данных Stream** API позволяет выполнять потоковую передачу сжатые аудио, чтобы служба распознавания речи с помощью PullStream или PushStream.

> [!IMPORTANT]
> Потоковая передача сжатые аудио поддерживается только для C++, C#и Java в Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9).

Wav/PCM см. в документации магистраль речи.  За пределами wav и PCM поддерживаются следующие форматы входных данных кодек сжатия:

- MP3
- ТРУДЕ/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Предварительные требования для использования кодек сжатия аудио вход

Установите эти дополнительные зависимости, чтобы использовать сжатые аудио вход с помощью Speech SDK для Linux:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Пример кода, с помощью кодека сжатия аудио вход

Для потоковой передачи в сжатом формате аудио в службы распознавания речи, создания `PullAudioInputStream` или `PushAudioInputStream`. Затем создайте `AudioConfig` из экземпляра класса stream, задав формат сжатия потока.

Предположим, что у вас есть класс входного потока с именем `myPushStream` и при использовании ТРУДЕ/OGG. Код может выглядеть следующим образом:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>Дальнейшие действия

- [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)
- [Распознавание речи в C#](quickstart-csharp-dotnet-windows.md)
