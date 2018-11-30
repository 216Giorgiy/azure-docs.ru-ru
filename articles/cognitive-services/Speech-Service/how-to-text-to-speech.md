---
title: Использование преобразования текста в речь в службе "Речь"
titleSuffix: Azure Cognitive Services
description: Ознакомьтесь с тем, как использовать преобразование текста в речь службе "Речь".
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: erhopf
ms.openlocfilehash: 16b7de909ce57a7f8d3cea54a59996dc6f0a6ba0
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712013"
---
# <a name="use-text-to-speech-in-speech-service"></a>Использование преобразования текста в речь в службе "Речь"

Голосовая служба предоставляет функцию "Преобразование текста в речь" с помощью простого HTTP-запроса. Текст запроса `POST` будет передан в соответствующую конечную точку, откуда будет возвращен службой в виде звукового файла (`.wav`), в котором будет содержаться синтезированная речь. Затем ваше приложение может использовать этот звук как угодно.

Тело запроса POST для преобразовывания текста в речь может быть обычным текстом (в формате ASCII или UTF8) или документом [SSML](speech-synthesis-markup.md). Для преобразования простых текстовых запросов будет использоваться голос по умолчанию. В большинстве случаев используется текст в формате SSML. HTTP-запрос должен содержать маркер [авторизации](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#authentication).

Ниже приведены региональные конечные точки преобразования текста в речь. Используйте ту из них, которая соответствует вашей подписке.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Указание голоса

Чтобы указать голос, используйте тег `<voice>` [SSML](speech-synthesis-markup.md). Например: 

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Чтобы узнать список допустимых голосов и их имена см. раздел [Text to Speech voices](language-support.md#text-to-speech) (Голоса преобразования текста в речь).

## <a name="make-a-request"></a>Выполнение запроса

HTTP-запрос по преобразованию текста в речь выполняется в режиме POST с текстом, который в теле запроса будет заменен на звук. Максимальная длина тела HTTP-запроса — 1024 символа. Для запроса обязательными являются следующие заголовки.

Заголовок|Значения|Комментарии
-|-|-
|`Content-Type` | `application/ssml+xml` | Формат входного текста.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`riff-16khz-16bit-mono-pcm`<br>`raw-8khz-8bit-mono-mulaw`<br>`riff-8khz-8bit-mono-mulaw`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | Формат выходного звука.
|`User-Agent`   |имя приложения; | Обязательным также является имя приложения, длина которого не должна превышать 255 символов.
| `Authorization`   | Маркер проверки подлинности можно получить путем предоставления ключа подписки службе токенов. Каждый из маркеров действителен 10 мин. См. раздел [REST APIs: Authentication](rest-apis.md#authentication) (Речевые службы REST API, подраздел "Аутентификация").

> [!NOTE]
> Если у выбранного голоса и формата вывода будет разная скорость передачи звука, то при необходимости звук будет обработан повторно.

Ниже приведен пример запроса.

```xml
POST /cognitiveservices/v1
HTTP/1.1
Host: westus.tts.speech.microsoft.com
X-Microsoft-OutputFormat: riff-24khz-16bit-mono-pcm
Content-Type: application/ssml+xml
User-Agent: Test TTS application
Authorization: (authorization token)

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
</voice> </speak>
```

В теле ответа со статусом 200 содержится звук в указанном формате вывода.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Если возникнет ошибка, то будут использованы коды состояния, приведенные ниже. В теле ответа ошибки также содержится описание проблемы.

|Код|ОПИСАНИЕ|Проблема|
|-|-|-|
400 |Ошибка запроса |Обязательный параметр отсутствует, пустой или его значение равно нулю. Или переданное либо обязательному, либо необязательному параметру значение является недопустимым. Распространенной проблемой является слишком длинный заголовок.
401|Не авторизовано |Запрос не авторизован. Убедитесь, что ваш ключ подписки или маркер являются допустимыми.
413|Размер запрашиваемой сущности слишком большой|Входной SSML слишком большой или содержит более 3 элементов `<voice>`.
429|Слишком много запросов|Вы превысили квоту или частоту запросов, разрешенных для вашей подписки.
|502|Недопустимый шлюз    | Проблема с сетью или сервером. Может также указывать на недопустимые заголовки.

Дополнительные сведения об API-интерфейсах REST функции "Преобразование текста в речь" см. в разделе [REST APIs](rest-apis.md#text-to-speech-api) (Речевые службы REST API).

## <a name="next-steps"></a>Дополнительная информация

- [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)
- [Краткое руководство. Распознавание речи в классическом приложении C++ для Windows с помощью пакета SDK службы "Речь"](quickstart-cpp-windows.md)
- [Краткое руководство. Распознавание речи с помощью пакета SDK службы распознавания речи Cognitive Services C#](quickstart-csharp-dotnet-windows.md)
- [Краткое руководство. Распознавание речи в приложении Java для Android с помощью пакета SDK для службы "Речь"](quickstart-java-android.md)
