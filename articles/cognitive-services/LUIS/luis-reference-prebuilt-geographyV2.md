---
title: Предварительно созданная сущность GeographyV2
titleSuffix: Azure Cognitive Services
description: В этой статье описана предварительно созданная сущность GeographyV2 в службе "Распознавание речи" (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: a81c97cf8883d2a3629d1eb1b9f6137609c6edda
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135106"
---
# <a name="geographyv2-prebuilt-entity-for-a-luis-app"></a>Предварительно созданная сущность GeographyV2 для приложения LUIS
Предварительно созданная сущность GeographyV2 определяет расположения. Так как эта сущность уже обучена, добавлять примеры высказываний, содержащие сущность GeographyV2, в намерения приложения не нужно. Сущность GeographyV2 поддерживает английский [язык и региональные параметры](luis-reference-prebuilt-entities.md).

## <a name="subtypes"></a>Подтипы
Географические расположения имеют подтипы.

|Subtype|Назначение|
|--|--|
|`poi`|достопримечательность|
|`city`|название города|
|`countryRegion`|название страны или региона|
|`continent`|название континента|
|`state`|название области, республики, края, округа|


## <a name="resolution-for-geographyv2-entity"></a>Разрешение для сущности GeographyV2
В следующем примере показано разрешение для сущности **builtin.geographyV2**.

```json
{
    "query": "Carol is visiting the sphinx in gizah egypt in africa before heading to texas",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.8008023
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.8008023
        }
    ],
    "entities": [
        {
            "entity": "the sphinx",
            "type": "builtin.geographyV2.poi",
            "startIndex": 18,
            "endIndex": 27
        },
        {
            "entity": "gizah",
            "type": "builtin.geographyV2.city",
            "startIndex": 32,
            "endIndex": 36
        },
        {
            "entity": "egypt",
            "type": "builtin.geographyV2.countryRegion",
            "startIndex": 38,
            "endIndex": 42
        },
        {
            "entity": "africa",
            "type": "builtin.geographyV2.continent",
            "startIndex": 47,
            "endIndex": 52
        },
        {
            "entity": "texas",
            "type": "builtin.geographyV2.state",
            "startIndex": 72,
            "endIndex": 76
        },
        {
            "entity": "carol",
            "type": "builtin.personName",
            "startIndex": 0,
            "endIndex": 4
        }
    ]
} 
```

## <a name="next-steps"></a>Дополнительная информация

См. дополнительные сведения о сущностях [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) и [ordinal](luis-reference-prebuilt-ordinal.md). 