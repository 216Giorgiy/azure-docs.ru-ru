---
title: Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API Визуального поиска Bing и Node.js
titleSuffix: Azure Cognitive Services
description: Узнайте, как отправить изображение в API визуального поиска Bing и получить аналитические сведения об этом изображении.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 9414bac220d928618b403aa2f7df7748772e0e9a
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047574"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API Визуального поиска Bing и Node.js

В этом кратком руководстве вы узнаете, как сделать первый вызов API визуального поиска Bing и просмотреть результаты поиска. Это простое приложение JavaScript отправляет изображение в API и отображает данные о нем. Хотя это приложение создается на языке JavaScript, API представляет собой веб-службу RESTful, совместимую с большинством языков программирования.

При отправке локального изображения данные формы должны содержать заголовок `Content-Disposition`. Необходимо задать для его параметра `name` значение "image", а для параметра `filename` можно присвоить любую строку. Форма содержит двоичные данные изображения. Максимально допустимый размер отправляемого изображения — 1 МБ.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Предварительные требования

* [Node.js](https://nodejs.org/en/download/)
* Модуль запросов для JavaScript. Вы можете использовать команду `npm install request`, чтобы установить модуль.
* Модуль данных формы. Вы можете использовать команду `npm install form-data`, чтобы установить модуль. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Инициализация приложения

1. Создайте файл JavaScript в избранной интегрированной среде разработки или редакторе и установите следующие требования:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Создайте переменные для конечной точки API, ключ подписки и путь к изображению:

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Создайте функцию с именем `requestCallback()`, чтобы вывести ответ, полученный от API:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Создание и отправка поискового запроса

1. Создайте объект **FormData** с помощью `FormData()` и добавьте в него путь к своему изображению, используя `fs.createReadStream()`:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Используйте библиотеку запросов, чтобы отправить изображение, и вызовите `requestCallback()` для вывода ответа. Не забудьте добавить ключ подписки в заголовок запроса:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Создание одностраничного веб-приложения Визуального поиска](../tutorial-bing-visual-search-single-page-app.md)
