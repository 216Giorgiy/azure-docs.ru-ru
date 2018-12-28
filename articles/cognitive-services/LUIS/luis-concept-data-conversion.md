---
title: Преобразование данных
titleSuffix: Language Understanding - Azure Cognitive Services
description: Узнайте, как изменить высказывания перед прогнозами в службе "Распознавание речи" (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: dc9040661eee4cafc655deb2436130f1abcfcfd5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094803"
---
# <a name="convert-data-format-of-utterances"></a>Преобразование формата данных высказываний
LUIS использует службу Cognitive Services Speech, чтобы преобразовать высказывания из произнесенной речи в текст перед получением прогноза. 

## <a name="speech-to-intent-conversion-concepts"></a>Принципы преобразования речи в намерения
Преобразование речи в текст в LUIS позволяет отправить на конечную точку произнесенные высказывания и получить от LUIS ответ с прогнозом. Этот процесс выполняется за счет интеграции LUIS со службой [Речь](https://docs.microsoft.com/azure/cognitive-services/Speech). 

### <a name="key-requirements"></a>Основные требования
Для этой интеграции нет необходимости создавать ключ **API распознавания речи Bing**. Здесь можно использовать ключ **службы "Распознавание речи"**, создаваемый на портале Azure. Не используйте начальный ключ LUIS, он не будет работать для этой интеграции.

### <a name="new-endpoint"></a>Новая конечная точка 
При такой интеграции создается новая конечная точка и модель прогнозирования [цены](luis-boundaries.md#key-limits). С использованием [пакета SDK для распознавания речи](https://github.com/Azure-Samples/cognitive-services-speech-sdk) конечная точка может получать как текстовые, так и произнесенные высказывания, что позволяет обойтись одной конечной точкой. 

### <a name="quota-usage"></a>Использование квоты
Информацию вы найдете в разделе об [ограничениях для ключей](luis-boundaries.md#key-limits). 

### <a name="data-retention"></a>Хранение данных
Данные, отправленные на конечную точку с использованием пакета SDK для распознавания речи, будь то текстовые или произнесенные высказывания, используются только для расширения возможностей модели речи. Они не используются за пределами модели для расширения возможностей службы "Речь" или LUIS в целом. При удалении приложения LUIS все сохраненные данные также удаляются.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Использование преобразования речи в текст](luis-tutorial-speech-to-intent.md)

