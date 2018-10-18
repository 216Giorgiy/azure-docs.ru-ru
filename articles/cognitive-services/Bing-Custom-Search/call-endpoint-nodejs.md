---
title: Краткое руководство. Вызов конечной точки службы "Пользовательский поиск Bing" с помощью Node.js
titlesuffix: Azure Cognitive Services
description: В этом кратком руководстве показано, как для запрашивать результаты поиска из экземпляра службы пользовательского поиска с помощью Node.js для вызова конечной точки службы пользовательского поиска Bing.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: af77b4c06b61cda4fd18d19ac3578129004c4914
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167211"
---
# <a name="quickstart-call-bing-custom-search-endpoint-nodejs"></a>Краткое руководство. Вызов конечной точки службы "Пользовательский поиск Bing" (Node.js)

В этом кратком руководстве показано, как запрашивать результаты поиска из экземпляра службы пользовательского поиска с помощью Node.js для вызова конечной точки службы пользовательского запроса Bing. 

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

- Готовый экземпляр службы пользовательского поиска. Ознакомьтесь с разделом [Create your first Bing Custom Search instance](quick-start.md) (Создание первого экземпляра службы "Пользовательский поиск Bing").
- Установленный компонент [Node.js](https://www.nodejs.org/).
- ключ подписки; Ключ подписки можно получить при активации [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search). Кроме того, вы можете использовать ключ платной подписки из панели мониторинга Azure (см. раздел об [учетной записи API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    

## <a name="run-the-code"></a>Выполнение кода

Чтобы запустить этот пример, выполните следующее.

1. Создайте папку для своего кода.  
  
2. Из командной строки или терминала перейдите в эту папку.  
  
3. Установите модуль Node **request**.
    <pre>
    npm install request
    </pre>  
    
4. Создайте файл с именем BingCustomSearch.js в созданной папке и скопируйте в него следующий код. Замените **YOUR-SUBSCRIPTION-KEY** и **YOUR-CUSTOM-CONFIG-ID** своими ключом подписки и идентификатором конфигурации.  
  
    ``` javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    
    var options = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
          'q=' + searchTerm + 
          '&customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    
    request(options, function(error, response, body){
        var searchResponse = JSON.parse(body);
        for(var i = 0; i < searchResponse.webPages.value.length; ++i){
            var webPage = searchResponse.webPages.value[i];
            console.log('name: ' + webPage.name);
            console.log('url: ' + webPage.url);
            console.log('displayUrl: ' + webPage.displayUrl);
            console.log('snippet: ' + webPage.snippet);
            console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
            console.log();
        }
    })
    ```  
  
6. Выполните код с помощью приведенной ниже команды:  
  
    ```    
    node BingCustomSearch.js
    ``` 

## <a name="next-steps"></a>Дополнительная информация
- [Настройка размещенного пользовательского интерфейса](./hosted-ui.md)
- [Use decoration markers to highlight text](./hit-highlighting.md) (Использование маркеров оформления для выделения текста)
- [Разбивка веб-страниц на страницы](./page-webpages.md)
