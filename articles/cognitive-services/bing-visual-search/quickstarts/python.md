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
ms.openlocfilehash: 96bd94e37c75d10726245fbcea7044d4ae2ed07e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070381"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Ваш первый запрос в службу наглядного поиска Bing на Python

API Bing для наглядного поиска возвращает сведения об изображении, которое вы предоставляете. Изображение можно предоставить с помощью URL-адреса изображения, токена аналитики или через отправку изображения. Сведения об этих параметрах см. в статье [What is Bing Visual Search API?](../overview.md) (Что такое API Bing для наглядного поиска?) В этой статье показано, как отправлять изображения. Отправка изображения может быть полезна в сценариях с использованием мобильных устройств, когда вы фотографируете какую-то достопримечательность и получаете сведения о ней. Например, аналитические сведения могут содержать любопытные факты об этой достопримечательности. 

При отправке локального изображения отображаются данные формы, которые необходимо включить в текст запроса POST. Эти данные формы должны содержать заголовок Content-Disposition. Его параметру `name` необходимо присвоить значение "image", а для параметра `filename` можно задать любую строку. Содержимым формы является двоичный файл изображения. Максимально допустимый размер отправляемого изображения — 1 МБ. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

В этой статье приводится простое консольное приложение, которое отправляет запрос к API Bing для наглядного поиска и отображает результаты поиска в формате JSON. Хотя это приложение написано на Python, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать JSON. 

## <a name="prerequisites"></a>Предварительные требования

Для запуска этого кода требуется [Python 3](https://www.python.org/).

В рамках этого краткого руководства можно использовать ключ [бесплатной пробной](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) подписки или ключ платной подписки.

## <a name="running-the-walkthrough"></a>Выполнение пошагового руководства

Чтобы запустить это приложение, сделайте следующее:

1. Создайте проект Python в любой интегрированной среде разработки или в текстовом редакторе.
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

[Получение полезных сведений об изображении с помощью токена аналитики](../use-insights-token.md)  
[Руководство по передаче изображения в Bing Visual Search](../tutorial-visual-search-image-upload.md)
[Руководство по одностраничным веб-приложениям для Bing Visual Search](../tutorial-bing-visual-search-single-page-app.md)  
[Общие сведения об API Bing для наглядного поиска](../overview.md)  
[Попробовать](https://aka.ms/bingvisualsearchtryforfree)  
[Получить ключ доступа бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Справочник по API Bing для наглядного поиска](https://aka.ms/bingvisualsearchreferencedoc)
