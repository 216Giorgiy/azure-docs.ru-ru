---
title: Справочник по предварительно созданным сущностям number в LUIS Azure | Документация Майкрософт
titleSuffix: Azure
description: В этой статье содержатся сведения о предварительно созданных сущностях number в Интеллектуальной службе распознавания речи (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: c1a263f21ae249ea80c0798ac81818c9e9cf1319
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236811"
---
# <a name="number-entity"></a>Сущность number
Существует множество способов использования числовых значений для подсчета, выражения и описания фрагментов информации. В этой статье рассматриваются только некоторые возможные примеры. Служба LUIS интерпретирует вариации в фразах пользователя и возвращает согласованные числовые значения. Так как эта сущность уже обучена, добавлять в назначение приложения примеры фраз, содержащие сущность number, не нужно. 

## <a name="types-of-number"></a>Типы сущности number
Управление сущностью number выполняется из репозитория GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml).

## <a name="examples-of-number-resolution"></a>Примеры разрешения сущности number

| Фраза        | Сущность   | Способы устранения: |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


Служба LUIS включает распознанное значение сущности **`builtin.number`** в поле `resolution` возвращаемого ответа в формате JSON.

## <a name="resolution-for-prebuilt-number"></a>Разрешение для предварительно созданной сущности number
В следующем примере показан ответ LUIS в формате JSON, который содержит расшифровку высказывания "two dozen" (две дюжины) в значение 24.

```JSON
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Дополнительная информация

Узнайте больше о сущностях [currency](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md) и [percentage](luis-reference-prebuilt-percentage.md). 