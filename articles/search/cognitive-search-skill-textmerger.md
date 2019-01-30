---
title: 'Навык когнитивного поиска: объединение текста (служба "Поиск Azure")'
description: Объедините текст из коллекции полей в одно консолидированное поле. Используйте этот когнитивный навык в конвейере обогащения службы "Поиск Azure".
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: a3c64a26ed5ee7454fcfd22f47c1a161f65c6de7
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445365"
---
#    <a name="text-merge-cognitive-skill"></a>Когнитивный навык объединения текста

Когнитивный навык **объединения текста** объединяет текст из коллекции полей в одном поле. 

> [!NOTE]
> Этот навык не привязан к API Cognitive Services, и за его использование плата не взимается. Но вам следует по-прежнему [подключать ресурс Cognitive Services](cognitive-search-attach-cognitive-services.md), чтобы переопределить **бесплатную** категорию ресурса, ограниченную малым количеством обогащений в день.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.MergeSkill

## <a name="skill-parameters"></a>Параметры навыков

Параметры зависят от регистра.

| Имя параметра     | ОПИСАНИЕ |
|--------------------|-------------|
| insertPreTag  | Строка, которая должна быть включена перед каждой вставкой. По умолчанию используется значение `" "`. Чтобы исключить пробел, задайте значение `""`.  |
| insertPostTag | Строка, которая должна быть включена после каждой вставки. По умолчанию используется значение `" "`. Чтобы исключить пробел, задайте значение `""`.  |


##  <a name="sample-input"></a>Пример ввода
Документ JSON, предоставляющий полезные входные данные для этого навыка, может иметь следующий вид:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Пример выходных данных
В этом примере показаны выходные данные предыдущего ввода, при условии, что *insertPreTag* имеет значение `" "`, а *insertPostTag* — `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>Расширенный пример определения набора навыков

Распространенным вариантом использования объединения текста является возможность объединить текстовое представление изображений (текст из навыка OCR или заголовок изображения) с полем содержимого документа. 

В следующем примере набора навыков используется умение OCR для извлечения текста из изображений, встроенных в документ. Затем создается поле *merged_text*, содержащее как исходный, так и обработанный OCR текст из каждого изображения. 

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
В приведенном выше примере предполагается наличие поля нормализованных изображений. Чтобы создать поле normalized-images, настройте конфигурацию *imageAction* в определении индексатора *generateNormalizedImages*, как показано ниже.

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>См. также

+ [Предопределенные навыки](cognitive-search-predefined-skills.md)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Создание индексатора (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
