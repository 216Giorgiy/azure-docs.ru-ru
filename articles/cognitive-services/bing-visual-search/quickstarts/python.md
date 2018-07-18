---
title: Краткое руководство по API Bing для наглядного поиска для Python | Документация Майкрософт
titleSuffix: Bing Web Search APIs - Cognitive Services
description: В этой статье показано, как передать изображение в API Bing для наглядного поиска и получить о нем аналитические сведения.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: a520466825eb429e45e0500b52bd7af502c0a38c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382473"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Ваш первый запрос в службу наглядного поиска Bing на Python

API Bing для наглядного поиска возвращает сведения об изображении, которое вы предоставляете. Изображение можно предоставить с помощью URL-адреса изображения, токена аналитики или через отправку изображения. Сведения об этих параметрах см. в статье [Что такое API для визуального поиска?](../overview.md) В этой статье показана отправка изображения. Отправка изображения может быть полезна в сценариях с использованием мобильных устройств, когда вы фотографируете какую-то достопримечательность и получаете сведения о ней. Например, аналитические сведения могут содержать любопытные факты об этой достопримечательности. 

При отправке локального изображения отображаются данные формы, которые необходимо включить в текст запроса POST. Эти данные формы должны содержать заголовок Content-Disposition. Его параметру `name` необходимо присвоить значение "image", а параметром `filename` может быть любая строка. Содержимым формы является двоичный файл изображения. Максимально допустимый размер отправляемого изображения — 1 МБ. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

В этой статье приводится простое консольное приложение, которое отправляет запрос к API Bing для наглядного поиска и отображает результаты поиска в формате JSON. Хотя это приложение написано на Python, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать JSON. 

## <a name="prerequisites"></a>предварительным требованиям

Для запуска этого кода необходим [Python 3](https://www.python.org/).

В рамках этого краткого руководства можно использовать ключ подписки [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) или платный ключ подписки.

## <a name="running-the-walkthrough"></a>Выполнение пошагового руководства

Чтобы запустить это приложение, выполните указанные ниже действия.

1. Создайте новый проект Python в любой интегрированной среде разработки или в текстовом редакторе.
2. Создайте файл с именем visualsearch.py и добавьте в него код, приведенный в этом кратком руководстве.
3. Замените значение `SUBSCRIPTION_KEY` своим ключом подписки.
3. Замените значение `imagePath` путем к отправляемому изображению.
4. Запустите программу.



Ниже показано, как отправить сообщение, используя составные данные формы в Python.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Дополнительная информация

[Получение аналитических сведений об изображении с помощью токена аналитики](../use-insights-token.md)  
[Руководство по одностраничным приложениям для наглядного поиска Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Обзор API для наглядного поиска Bing](../overview.md)  
[Попробовать](https://aka.ms/bingvisualsearchtryforfree)  
[Получить ключ доступа бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Справочник по API Bing для наглядного поиска](https://aka.ms/bingvisualsearchreferencedoc)
