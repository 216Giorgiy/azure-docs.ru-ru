---
title: Руководство. Как создать языковую модель с помощью службы "Речь"
titlesuffix: Azure Cognitive Services
description: Узнайте, как создать языковую модель с помощью службы "Речь". Используйте эту пользовательскую языковую модель в сочетании с имеющимися новейшими моделями речи от Майкрософт, чтобы добавить в приложение голосовое взаимодействие.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 8276b86df2dc1bc90fc07da262aa0979f7562619
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57548495"
---
# <a name="tutorial-create-a-custom-language-model"></a>Руководство. Создание пользовательской языковой модели

В этом документе вы создадите пользовательскую языковую модель. Затем вы можете использовать эту пользовательскую языковую модель в сочетании с имеющимися новейшими моделями речи от Майкрософт, чтобы добавить в приложение голосовое взаимодействие.

В этом документе рассматриваются следующие вопросы:
> [!div class="checklist"]
> * Подготовка данных
> * Импорт набора языковых данных
> * Создание пользовательской языковой модели.

Если у вас еще нет учетной записи Cognitive Services, создайте [бесплатную учетную запись](https://azure.microsoft.com/try/cognitive-services/), а затем приступайте к работе.

## <a name="prerequisites"></a>Предварительные требования

Подключите учетную запись Cognitive Services к подписке. Для этого откройте страницу [подписок Cognitive Services](https://customspeech.ai/Subscriptions).

Чтобы подключиться к подписке служб распознавания речи, созданной на портале Azure, нажмите кнопку **Connect existing subscription** (Подключить существующую подписку).

Дополнительные сведения о создании подписок служб распознавания речи на портале Azure см. на странице [Бесплатная пробная подписка на службу "Речь"](get-started.md).

## <a name="prepare-the-data"></a>Подготовка данных

Для создания пользовательской языковой модели для приложения вам необходимо предоставить список примеров фраз для системы, например:

*   "На прошлой неделе у пациента была крапивница".
*   "У пациента был заживший шрам, оставшийся после герниопластической операции".

Предложения необязательно должны быть полными или грамматически правильными, но они должны точно отражать произносимые входные данные, с которыми система ожидаемо должна столкнуться после развертывания. Эти примеры должны отражать стиль и содержимое задачи, которую пользователи будут выполнять с помощью приложения.

Данные языковой модели должны описываться с использованием метки порядка байтов UTF-8. Текстовый файл должен содержать один пример (предложение, фразу или запрос) на строку.

Если некоторые термины должны иметь более высокий вес (значимость), вы можете добавить в данные несколько фраз, содержащих эти термины.

Основные требования к языковым данным приведены в следующей таблице.

| Свойство | Значение |
|----------|-------|
| Кодировка текста | Метка порядка байтов UTF-8|
| Количество фраз в строке | 1 |
| Максимальный размер файла | 1,5 ГБ |
| Примечания | Избегайте повторения знаков более четырех раз, например "aaaaa"|
| Примечания | Не используйте специальные знаки, такие как "\t" или любой другой знак UTF-8, идущий после U+00A1 в [таблице знаков Юникод](https://www.utf8-chartable.de/)|
| Примечания | Также будут отклонены универсальные коды ресурсов (URI), так как нет уникального способа произнести их|

При импорте текст нормализуется, чтобы система могла его обработать. Однако есть некоторые важные действия нормализации, которые пользователь должен выполнять _перед_ отправкой данных. Ознакомьтесь с [рекомендациями по транскрибированию](prepare-transcription.md), чтобы определить соответствующий формат языка, используемый при подготовке языковых данных.

## <a name="language-support"></a>Поддержка языков

Просмотрите весь список [поддерживаемых языков](language-support.md#text-to-speech) для пользовательских языковых моделей **преобразования речи в текст**.



## <a name="import-the-language-data-set"></a>Импорт набора языковых данных

В строке **Language Datasets** (языковые наборы данных) нажмите кнопку **Import** (Импортировать), и на сайте появится страница для отправки нового набора данных.

Когда вы будете готовы импортировать свой набор языковых данных, войдите на [портал службы распознавания речи](https://customspeech.ai). Затем в верхней ленте выберите раскрывающееся меню **Настраиваемое распознавание речи**. Затем выберите **Adaptation Data** (Данные адаптации). При первой попытке передать данные в службу распознавания речи вы увидите пустую таблицу с названием **Наборы данных**.

Чтобы импортировать новый набор данных, в строке **Language Datasets** (Наборы языковых данных) нажмите кнопку **Import** (Импортировать). Сайт отобразит страницу для передачи нового набора данных. Введите **Имя** и **Описание**, которые помогут определять набор данных в будущем, после чего выберите языковой стандарт.

Затем нажмите кнопку **Выбрать файл**, чтобы найти текстовый файл языковых данных. После этого для передачи набора данных нажмите кнопку **Import** (Импортировать). В зависимости от размера набора данных импортирование может занять несколько минут.

![Попытка](media/stt/speech-language-datasets-import.png)

После завершения операции импорта вашему набору языковых данных будет присвоена соответствующая запись в языковых данных. Обратите внимание, что ему был присвоен уникальный идентификатор (GUID). В данных также есть характеристика, отражающая их текущее состояние. Состояние может принимать значения **Ожидание**, если данные находятся в очереди для обработки, **Идет обработка**, если выполняется проверка, и **Завершено**, когда данные можно использовать. При проверке данных выполняется ряд проверок текста в файле. А также некоторая нормализация данных.

В случае состояния **Завершено**, вы можете нажать **View Report** (Просмотр отчета), чтобы просмотреть отчет о проверке языковых данных. Отобразится количество успешных и неудачных проверок, а также сведения о фразах, завершившихся сбоем. В следующем примере представлены два примера сбоя проверки из-за недопустимых символов. (В этом наборе данных в первой строке было два символа табуляции, во второй — несколько символов, которые не являются частью набора символов для печати ASCII, а третья строка была пустой).

![Попытка](media/stt/speech-language-datasets-report.png)

Если набор языковых данных имеет состояние **Завершено**, его можно использовать для создания пользовательской языковой модели.

![Попытка](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Создание пользовательской языковой модели

Когда языковые данные будут готовы, в раскрывающемся меню **Меню** выберите **Language Models** (Языковые модели), чтобы начать процесс создания пользовательской языковой модели. На этой странице содержится таблица с именем **Language Models** (Языковые модели) с текущими пользовательскими языковыми моделями. Если вы еще не создали ни одной пользовательской языковой модели, таблица будет пустой. Рядом с соответствующей записью данных в таблице отображается текущий языковой стандарт.

Прежде чем выполнять какие-либо действия, необходимо выбрать соответствующий языковой стандарт. Текущий языковой стандарт указывается в заголовке таблицы на всех страницах данных, модели и развертывания. Чтобы изменить языковой стандарт, нажмите кнопку **Change Locale** (Изменить языковой стандарт) под заголовком таблицы.  Откроется страница подтверждения языкового стандарта. Чтобы вернуться к таблице, нажмите **ОК**.

На странице создания языковой модели введите **имя** и **описание**, чтобы затем отслеживать соответствующую информацию об этой модели, например используемый набор данных. После этого в раскрывающемся меню выберите **Base Language Model** (Базовая языковая модель). Эта модель — отправная точка настройки.

Вы можете выбрать из двух базовых языковых моделей. Модель поиска и диктовки подходит для речи, направленной в приложение, например команд, поисковых запросов или диктовки. Разговорная модель подходит для распознавания речи при общении. Этот тип речи обычно направлен на другого человека и встречается в колл-центрах или на встречах. Также есть новая общедоступная модель "Универсальная". Эта модель ориентирована на работу со всеми сценариями и в конечном итоге призвана заменить модель поиска и диктовки и разговорную модель.

Как показано в следующем примере, после указания базовой языковой модели используйте раскрывающееся меню **Language Data** (Языковые данные), чтобы выбрать набор языковых данных, который вы хотите использовать для настройки.

![Попытка](media/stt/speech-language-models-create2.png)

Как и при создании акустической модели, после завершения обработки вы можете выбрать автономное тестирование новой модели. Для оценки модели требуется набор акустических данных.

Чтобы выполнить автономное тестирование языковой модели, установите флажок напротив **Автономное тестирование**. Затем в раскрывающемся меню выберите акустическую модель. Если вы не создали ни одной пользовательской акустической модели, то базовая акустическая модель Майкрософт будет единственной моделью в меню. Если вы выбрали базовую разговорную языковую модель, вам нужно использовать разговорную акустическую модель. Если вы используете языковую модель поиска и диктовки, нужно выбрать акустическую модель поиска и диктовки.

Наконец, выберите акустический набор данных, который вы хотите использовать для оценки.

Когда будете готовы начать обработку, нажмите **Создать**. Затем появится таблица языковых моделей. В таблице появится новая запись, соответствующая этой модели. Статус отражает состояние модели и будет изменяться, представляя разные этапы: **Ожидание**, **Идет обработка** и **Завершено**.

Когда модель достигнет состояния **Завершено**, ее можно развернуть в конечную точку. Выбрав **View Result** (Просмотр результатов), вы увидите результаты автономного тестирования (если оно выполнялось).

Если на каком-то этапе вы захотите изменить **имя** или **описание** модели, щелкните ссылку **Изменить** в соответствующей строке таблицы языковых моделей.

## <a name="next-steps"></a>Дополнительная информация

- [Пробная версия службы речи Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)
- [Распознавание речи в C#](quickstart-csharp-dotnet-windows.md)
- [Образец данных Git](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
