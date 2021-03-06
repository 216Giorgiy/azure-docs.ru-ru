---
title: Миграция из API перевода речи в службу "Речь"
titleSuffix: Azure Cognitive Services
description: Узнайте, как переносить ваши приложения из API перевода речи в службы распознавания речи.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: 35f970e81d27511bd35610bc2988a5ea4832906b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "57898641"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Миграция из API перевода речи в службу "Речь"

В этой статьей показано, как выполнить миграцию приложений из API перевода речи Microsoft в [службу "Речь"](index.yml). Здесь описаны различия между API перевода речи и службой "Речь", а также предлагаются стратегии по миграции приложений.

> [!NOTE]
> Ключ подписки API перевода речи не принимается службой "Речь". Необходимо создать подписку службы распознавания речи.

## <a name="comparison-of-features"></a>Сравнение функций

| Функция                                           | API перевода речи                                  | Службы речи | Сведения                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Преобразование в текст                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Преобразование в речь                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Глобальная конечная точка                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Службы речи не предлагают глобальной конечной точки. Глобальная конечная точка может автоматически направлять трафик в ближайшую региональную конечную точку, сокращая задержку в приложении.                                                    |
| Региональные конечные точки                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Ограничение времени ожидания подключения                             | 90 минут                                               | Без ограничений с пакетом SDK. 10 минут с подключением по протоколу WebSocket.                                                                                                                                                                                                                                                                                   |
| Ключ проверки подлинности в заголовке                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Перевод на несколько языков в одном запросе | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Доступные пакеты SDK                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | См. в разделе [документации служб речи](index.yml) для доступных пакетов SDK.                                                                                                                                                    |
| Подключение по протоколу WebSocket                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| API языков                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Службы речи поддерживает тот же самый набор языков, описанные в [Справочник по API перевода языков](../translator-speech/languages-reference.md) статьи. |
| Фильтр и пометка ненормативной лексики                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Входные данные в формате PCM / WAV                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Входные данные в виде файлов других типов                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Частичные результаты                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Сведения о времени                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| Идентификатор корреляции                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Пользовательские модели речи                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Службы речи предлагают настраиваемые модели речи, которые позволяют настраивать распознавания речи для уникальных словаря вашей организации.                                                                                                                                           |
| Пользовательские модели перевода                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Если вы подпишитесь на API перевода текстов Microsoft, вам станет доступен [Custom Translator](https://www.microsoft.com/translator/business/customization/), позволяющий использовать собственные данные для более точных переводов.                                                 |

## <a name="migration-strategies"></a>Стратегии миграции

Если у вас или у вашей организации в среде разработки или в рабочей среде есть приложения, которые используют API перевода речи, их необходимо перевести на использование службы "Речь". Дополнительные сведения о доступных пакетах SDK, примерах кода и руководствах см. в [документации по службе "Речь"](index.yml). При выполнении миграции учитывайте следующие факторы.

* Службы речи не предлагают глобальной конечной точки. Необходимо определить, будет ли ваше приложение эффективно функционировать, используя одну региональную конечную точку для всего трафика. В противном случае используйте геолокацию, чтобы определить наиболее эффективную конечную точку.

* Если приложение использует длительные подключения и не поддерживает доступный пакет SDK, можно воспользоваться подключением WebSocket. Управляйте 10-минутным временем ожидания путем повторного подключения в нужное время.

* Если ваше приложение использует API перевода текстов и API перевода речи, чтобы включить пользовательские модели перевода, вы можете добавить идентификаторы категории с помощью службы "Речь".

* В отличие от Translator Speech API службы распознавания речи можно завершить переводы на несколько языков в одном запросе.

## <a name="next-steps"></a>Дальнейшие действия

* [Бесплатная пробная версия служб речи](get-started.md)
* [Краткое руководство Распознавание речи в приложении UWP с помощью пакета SDK для службы "Речь"](quickstart-csharp-uwp.md)

## <a name="see-also"></a>См. также

* [Общие сведения о службе "Речь"](overview.md)
* [Документация служб речи и Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
