---
title: Краткое руководство. API Bing для визуального поиска для JavaScript | Документация Майкрософт
titleSuffix: Bing Web Search APIs - Cognitive Services
description: В этой статье показано, как передать изображение в API Bing для визуального поиска и получить о нем аналитические сведения.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: dd28c829d8d24980a746244dc6aca880d2d69224
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382485"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>Ваш первый запрос в службу визуального поиска Bing на JavaScript

API Bing для визуального поиска возвращает сведения об изображении, которое вы предоставляете. Изображение можно предоставить с помощью URL-адреса изображения, токена аналитики или с помощью отправки. Сведения об этих параметрах см. в статье [What is Bing Visual Search API?](../overview.md) (Что такое API Bing для визуального поиска?). В этой статье показана отправка изображения. Отправка изображения может быть полезна в сценариях с использованием мобильных устройств, когда вы фотографируете какую-то достопримечательность и получаете сведения о ней. Например, аналитические сведения могут содержать любопытные факты об этой достопримечательности. 

При отправке локального изображения отображаются данные формы, которые необходимо включить в текст запроса POST. Эти данные формы должны содержать заголовок Content-Disposition. Его параметру `name` нужно присвоить значение image, а параметром `filename` может быть любая строка. Содержимым формы является двоичный файл изображения. Максимально допустимый размер отправляемого изображения — 1 МБ. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

В этой статье описано простое консольное приложение, которое отправляет запрос к API Bing для визуального поиска и отображает результаты поиска в формате JSON. Хотя это приложение написано на JavaScript, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать JSON. 

## <a name="prerequisites"></a>предварительным требованиям

Для запуска этого кода требуется [Node.js 6](https://nodejs.org/en/download/).

В рамках этого краткого руководства можно использовать ключ [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) подписки или ключ платной подписки.

## <a name="running-the-application"></a>Запуск приложения

Ниже показано, как отправить сообщение с использованием FormData в Node.js.

Чтобы запустить это приложение, сделайте следующее:

1. Создайте папку для проекта (либо воспользуйтесь выбранной интегрированной средой разработки или редактором).
2. Из командной строки или терминала перейдите в эту папку.
3. Установите модули request:  
  ```  
  npm install request  
  ```  
3. Установите модули form-data:  
  ```  
  npm install form-data  
  ```  
4. Создайте файл с именем GetVisualInsights.js и добавьте в него следующий код.
5. Замените значение `subscriptionKey` своим ключом подписки.
6. Замените значение `imagePath` путем к отправляемому изображению.
7. Запустите программу.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Дополнительная информация

[Получение аналитических сведений об изображении с помощью токена аналитики](../use-insights-token.md)  
[Руководство по одностраничным приложениям для визуального поиска Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Visual Search overview](../overview.md) (Общие сведения об API для визуального поиска Bing)  
[Тестирование](https://aka.ms/bingvisualsearchtryforfree)  
[Получение ключа доступа к бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API reference](https://aka.ms/bingvisualsearchreferencedoc) (Справочник по API Bing для визуального поиска)
