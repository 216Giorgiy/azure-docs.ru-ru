---
title: Распознавание речи с помощью REST API
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать API преобразования речи в текст в службе речи Azure Cognitive Services.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 1d8ae196884d800f441943609ac0189260a0ffcf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865742"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Распознавание речи с помощью REST API

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

Распознать короткие фрагменты речи можно с помощью запроса HTTP POST в REST API.

REST API является самым простым способом распознавания речи, если вы не используете язык, поддерживаемый пакетом [SDK](speech-sdk.md). Вы отправляете запрос HTTP POST к конечной точке службы и передаете весь фрагмент речи в тексте запроса. Вы получаете ответ с распознанным текстом.

> [!NOTE]
> При использовании интерфейса REST API ограничение длительности фрагмента речи составляет 15 секунд.
> Для распознавания более длительных фрагментов речи рекомендуем использовать [пакет SDK службы "Речь"](how-to-recognize-speech-csharp.md).

Дополнительные сведения о REST API **преобразования текста в речь** см. в статье о [REST API](rest-apis.md#speech-to-text-api). Чтобы увидеть API в действии, скачайте [примеры REST API](https://github.com/Azure-Samples/SpeechToText-REST) с GitHub.

## <a name="next-steps"></a>Дополнительная информация

- Ознакомьтесь со статьей [REST API службы распознавания речи](rest-apis.md).
