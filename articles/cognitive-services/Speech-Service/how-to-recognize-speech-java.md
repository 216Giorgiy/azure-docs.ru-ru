---
title: Распознавание речи с помощью пакета SDK службы "Речь" для Java
titleSuffix: Azure Cognitive Services
description: Сведения о различных способах распознавания речи (из файла, с микрофона, с помощью настроенной модели, а также непрерывное или однократное распознавание) с помощью пакета SDK службы "Речь" для Java.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 5bf1ec96689bc5b1d6e667edc01b83b385daae56
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223856"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-java"></a>Распознавание речи с помощью пакета SDK службы "Речь" для Java

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-java[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionWithMicrophone)]

[!INCLUDE [Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-java[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionCustomized)]

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-java[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionContinuousWithFile)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Найдите код, используемый в этой статье, в папке samples/java/jre/console.

## <a name="next-steps"></a>Дополнительная информация

- [Как распознавать намерения на основе речи](how-to-recognize-intents-from-speech-java.md).
- [Как переводить речь](how-to-translate-speech-java.md)

