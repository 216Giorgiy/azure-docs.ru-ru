---
title: Краткое руководство. API Bing для проверки орфографии, Java
titlesuffix: Azure Cognitive Services
description: Информация и примеры кода, которые помогут вам приступить к работе с API Bing для проверки орфографии.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: d859d2815f47d753c56002a9484f97149e71f1e4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159254"
---
# <a name="quickstart-for-bing-spell-check-api-with-java"></a>Краткое руководство по API Bing для проверки орфографии с использованием Java 

В этой статье показано, как использовать [API Bing для проверки орфографии](https://azure.microsoft.com/services/cognitive-services/spell-check/)  с помощью Java. API проверки орфографии возвращает список нераспознанных слов вмсте с предлагаемыми вариантами для замены. Обычно сначала текст отправляется в API, а затем выполняются предложенные замены в тексте либо список замен отображается для пользователя приложения, который решает, нужно ли делать замены. В этой статье показано, как отправить запрос, который содержит текст "Hollo, wrld!". Для замены будут предложены варианты Hello и world.

## <a name="prerequisites"></a>Предварительные требования

Для компиляции и запуска этого кода вам потребуется [пакет JDK 7 или 8](https://aka.ms/azure-jdks). При желании можно воспользоваться интегрированной средой разработки Java, но и обычного текстового редактора будет достаточно.

Вам потребуется [учетная запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с **API Bing для проверки орфографии версии 7**. Для этого краткого руководства достаточно [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/#lang). Потребуется ключ доступа, предоставляемый при активации бесплатной пробной версии. Можно также использовать ключ платной подписки, указанный на панели мониторинга Azure.  См. также [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="get-spell-check-results"></a>Получение результатов проверки орфографии

1. Создайте новый проект Java в любой удобной интегрированной среде разработки.
2. Добавьте указанный ниже код.
3. Замените значение `subscriptionKey` ключом доступа, допустимым для подписки.
4. Запустите программу.

```java
import java.io.*;
import java.net.*;
import javax.net.ssl.HttpsURLConnection;

public class HelloWorld {

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    // NOTE: Replace this example key with a valid subscription key.
    static String key = "ENTER KEY HERE";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";

    public static void check () throws Exception {
        String params = "?mkt=" + mkt + "&mode=" + mode;
        URL url = new URL(host + path + params);
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        connection.setRequestProperty("Content-Length", "" + text.length() + 5);
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();

        BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
        }
        in.close();
    }

    public static void main(String[] args) {
        try {
            check ();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Ответ**

Успешный ответ возвращается в формате JSON, как показано в примере ниже. 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Руководство по API Bing для проверки орфографии](../tutorials/spellcheck.md)

## <a name="see-also"></a>См. также

- [Общие сведения об API Bing для проверки орфографии](../proof-text.md)
- [Справочник по API проверки орфографии Bing версии 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
