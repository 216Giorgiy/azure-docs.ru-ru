---
title: Навык когнитивного поиска OCR (Поиск Azure) | Документация Майкрософт
description: Извлечение текста из файлов изображений в конвейер обогащения поиска Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 478afe81ed739b98487973eb092ee9cad0aa17fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37055683"
---
# <a name="ocr-cognitive-skill"></a>Когнитивный навык распознавания текста

Навык **OCR** извлекает текст из файлов изображений. Поддерживаемые форматы файлов включают:

+ .JPEG
+ .JPG
+ .PNG
+ .BMP
+ .GIF


## <a name="skill-parameters"></a>Параметры навыков

Параметры зависят от регистра.

| Имя параметра     | ОПИСАНИЕ |
|--------------------|-------------|
| detectOrientation | Включает автоопределение ориентации изображения. <br/> Допустимые значения: true или false.|
|defaultLanguageCode | <p>  Код языка вводимого текста. Поддерживаемые языки включают в себя: <br/> zh-Hans (китайский, упрощенное письмо) <br/> zh-Hant (китайский, традиционное письмо) <br/>CS (чешский) <br/>da (датский) <br/>nl (нидерландский) <br/>en (английский) <br/>fi (финский)  <br/>fr (французский) <br/>  de (немецкий) <br/>el (греческий) <br/> hu (венгерский) <br/> it (итальянский) <br/>  ja (японский) <br/> ko (корейский) <br/> nb (норвежский) <br/>   pl (польский) <br/> pt (португальский) <br/>  ru (русский) <br/>  es (испанский) <br/>  sv (шведский) <br/>  tr (турецкий) <br/> ar (арабский) <br/> ro (румынский) <br/> sr-Cyrl (сербский, кириллица) <br/> sr-Latn (сербский, латиница) <br/>  sk (словацкий) <br/>  unk (неизвестно) <br/><br/> Если код языка не указан или для него задано значение null, язык — autodetected. </p> |
| textExtractionAlgorithm | printed или handwritten. Алгоритм распознавания текста handwritten в настоящее время находится в режиме предварительной версии и поддерживается только на английском языке. |

## <a name="skill-inputs"></a>Входные данные навыков

| Ввод имени      | ОПИСАНИЕ                                          |
|---------------|------------------------------------------------------|
| изображение         | Сложный тип. В настоящее время работает только с полем /document/normalized_images, созданным индексатором большого двоичного объекта Azure, если для ```imageAction``` установлено значение ```generateNormalizedImages```. Дополнительные сведения см. в [этом примере](#sample-output).|


## <a name="skill-outputs"></a>Выходные данные навыка
| Имя вывода     | ОПИСАНИЕ                   |
|---------------|-------------------------------|
| текст          | Обычный текст, извлеченный из изображения.   |
| layoutText    | Сложный тип, описывающий извлеченный текст, а также расположение, где найден указанный текст.|


## <a name="sample-definition"></a>Пример определения

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Пример вывода текста и layoutText

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Пример. Слияние текста, извлеченного из внедренных изображений с содержимым документа.

Распространенным вариантом использования текстового слияния является возможность объединить текстовое представление изображений (текст из навыка OCR или заголовок изображения) с полем содержимого документа. 

В следующем примере набор навыков создает поле *merged_text*, содержащее текстовое содержимое вашего документа, а также текст OCRed из каждого изображения, внедренного в этот документ. 

#### <a name="request-body-syntax"></a>Синтаксис текста запроса
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
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
В приведенном выше примере набора навыков предполагается наличие поля нормализованных изображений. Чтобы создать это поле, настройте конфигурацию *imageAction* в определении индексатора *generateNormalizedImages*, как показано ниже.

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
+ [Text Merge cognitive skill](cognitive-search-skill-textmerger.md) (Когнитивный навык слияния текста)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Создание индексатора (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
