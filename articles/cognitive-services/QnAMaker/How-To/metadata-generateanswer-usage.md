---
title: Метаданные с использованием API GenerateAnswer — QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker позволяет добавлять метаданные в виде пар "ключ-значение" в наборы вопросов и ответов. Эти сведения можно использовать, чтобы отфильтровать результаты запросов пользователей и сохранить дополнительную информацию, которая может использоваться в дальнейших диалогах.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 12/18/2018
ms.author: tulasim88
ms.openlocfilehash: 004f09eb77d1bc32e44e1940186e8a631c45846d
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608483"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Использование метаданных и API GenerateAnswer.

QnA Maker позволяет добавлять метаданные в виде пар "ключ — значение" в наборы вопросов и ответов. Эти сведения можно использовать, чтобы отфильтровать результаты запросов пользователей и сохранить дополнительную информацию, которая может использоваться в дальнейших диалогах. Дополнительные сведения см. в разделе [Knowledge base](../Concepts/knowledge-base.md) (База знаний).

## <a name="qna-entity"></a>Сущность QnA

Сначала важно понять, как QnA Maker хранит данные вопросов и ответов. На рисунке ниже показана сущность QnA.

![Сущность QnA](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

У каждой сущности QnA имеется уникальный постоянный идентификатор. Этот идентификатор можно использовать для внесения изменений в определенную сущность QnA.

## <a name="generateanswer-api"></a>API GenerateAnswer

API GenerateAnswer можно использовать в боте или приложении для отправки к базе знаний запросов с вопросом пользователя для получения наиболее точного ответа из наборов вопросов и ответов.

### <a name="generateanswer-endpoint"></a>Конечная точка GenerateAnswer

После публикации базы знаний с помощью [портала QnA Maker](https://www.qnamaker.ai) или [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) можно получить сведения о конечной точке GenerateAnswer.

Вот как это можно сделать.
1. Войдите на сайт [https://www.qnamaker.ai](https://www.qnamaker.ai).
2. В разделе **My knowledge bases** (Мои базы знаний) щелкните **View Code** (Просмотреть код) для своей базы знаний.
Раздел ![My knowledge bases](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png) (Мои базы знаний)
3. Получите сведения о конечной точке GenerateAnswer.

![Сведения о конечной точке](../media/qnamaker-how-to-metadata-usage/view-code.png)

Можно также получить сведения о конечной точки на вкладке **Settings** (Параметры) базы знаний.

### <a name="generateanswer-request"></a>Запрос к GenerateAnswer

Для вызова GenerateAnswer используется HTTP-запрос POST. Пример кода, в котором демонстрируется вызов GenerateAnswer, доступен в этих [кратких руководствах](../quickstarts/csharp.md).

- **URL-адрес запроса**: https://{конечная точка QnA Maker}/knowledgebases/{ИД базы знаний}/generateAnswer

- **Параметры запроса**: 
    - **Knowledge base ID** (строка). Идентификатор GUID для базы знаний.
    - **QnAMaker endpoint** (строка). Имя узла конечной точки, развернутой в вашей подписке Azure.
- **Заголовки запроса**
    - **Content-Type** (строка). Тип носителя текста, отправляемого в API.
    - **Authorization** (строка). Ключ конечной точки (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **Текст запроса**
    - **question** (строка). Вопрос пользователя, ответ на который запрашивается в базе знаний.
    - **top** (необязательный параметр, целое число). Количество ранжированных результатов в выходных данных. Значение по умолчанию — 1.
    - **userId** (необязательный параметр, строка). Уникальный идентификатор для определения пользователя. Этот идентификатор будет записываться в журналы чата.
    - **strictFilters** (необязательный параметр, строка). Если указан, QnA Maker будет возвращать только ответы с указанными метаданными. Дополнительные сведения приведены ниже.
    ```json
    {
        "question": "qna maker and luis",
        "top": 6,
        "strictFilters": [
        {
            "name": "category",
            "value": "api"
        }],
        "userId": "sd53lsY="
    }
    ```

### <a name="generateanswer-response"></a>Ответ GenerateAnswer

- **Response 200** (Ответ 200): успешный вызов возвращает результат для вопроса. Результат содержит следующие поля:
    - **answers** (Ответы): список ответов на запрос пользователя, отсортированных по убыванию оценки;
        - **score**. Оценка от 0 до 100.
        - **questions**. Вопросы пользователя.
        - **answer**. Ответ на вопрос.
        - **source**. Имя источника ответа, который был извлечен или сохранен в базе знаний.
        - **metadata**. Метаданные, связанные с ответом.
            - name. Имя метаданных (строка, максимальная длина 100, обязательный параметр.
            - value. Значение метаданных. (строка, максимальная длина 100, обязательный параметр.
        - **Id**. Уникальный идентификатор, назначенный ответу.
    ```json
    {
        "answers": [
            {
                "score": 28.54820341616869,
                "Id": 20,
                "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
                "source": "Custom Editorial",
                "questions": [
                    "How can I integrate LUIS with QnA Maker?"
                ],
                "metadata": [
                    {
                        "name": "category",
                        "value": "api"
                    }
                ]
            }
        ]
    }
    ```

## <a name="metadata-example"></a>Пример метаданных

Рассмотрим приведенные ниже данные часто задаваемых вопросов. Добавьте метаданные в базу знаний, щелкнув значок метаданных.

![Добавление метаданных](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Фильтрация результатов с помощью strictFilters по тегам метаданных

Рассмотрим вопрос пользователя "Когда закрывается этот отель?", когда подразумевается ресторан "Paradise".

Так как требуются результаты только для ресторана "Paradise", в вызове GenerateAnswer можно задать фильтр по метаданным "Restaurant Name" (Название ресторана), как показано ниже.

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

### <a name="keep-context"></a>Сохранение контекста
Ответ на запрос к GenerateAnswer содержит необходимые сведения о метаданных соответствующего набора вопросов и ответов, как показано ниже.

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

Эти сведения можно использовать для записи контекста предыдущего общения, который можно будет использовать при дальнейшем общении. 

## <a name="next-steps"></a>Дополнительная информация

На странице публикации также предоставляются сведения для создания ответа с помощью [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) и [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Создание базы знаний](./create-knowledge-base.md)
