---
title: Прогнозирование намерений
titleSuffix: Azure Cognitive Services
description: Создание пользовательского приложения, которое прогнозирует пользовательские намерения. Это приложение является простейшим типом приложения LUIS, так как оно не извлекает различные элементы данных из текста высказывания, такие как адреса электронной почты или даты.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: b1a9718fdf7222dae06f7fe9b3a0f14b50293c08
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097800"
---
# <a name="tutorial-1-build-custom-app-to-determine-user-intentions"></a>Руководство 1. Создание пользовательского приложения для определения намерений пользователя

В этом руководстве вы создадите пользовательское приложение "Управление персоналом", которое прогнозирует пользовательские намерения на основе высказываний (текста). В итоге вы получите конечную точку LUIS, работающую в облаке.

Приложение предназначено для определения намерения беседы на естественном языке. Эти **Намерения** подразделяются на категории. Это приложение имеет несколько намерений. Первое намерение, **`GetJobInformation`**, определяет, когда пользователь хочет получить сведения о вакансиях в компании. Второе намерение, **`None`**, используется для всех высказываний пользователя, которые находятся за пределами _домена_ (области) этого приложения. Позже добавляется третье намерение, **`ApplyForJob`**, для любых высказываний о подаче заявки на работу. Третье намерение отличается от `GetJobInformation` отсутствием сведений о должности при подаче заявки. Однако, в зависимости от выбора слов для запроса, различить эти намерения может быть сложно, потому что оба относятся к подаче заявки на работу.

Выполнив запрос, приложение LUIS возвращает ответ JSON. LUIS не предоставляет ответы на пользовательские высказывания, а только определяет, какой тип информации запрашивается на естественном языке. 

**В этом руководстве рассмотрено, как выполнять следующие задачи.**

> [!div class="checklist"]
> * Создание нового приложения 
> * Создание намерений
> * Добавление примеров высказываний
> * Обучать приложения
> * Публикация приложения
> * Получение намерения из конечной точки

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Создание нового приложения

1. Войдите на портал LUIS с помощью URL-адреса [https://www.luis.ai](https://www.luis.ai). 

2. Выберите **Создать приложение**.  

    [![Снимок экрана со страницей My Apps (Мои приложения) Интеллектуальной службы распознавания речи (LUIS)](media/luis-quickstart-intents-only/app-list.png "Screenshot of Language Understanding (LUIS) My Apps page")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Во всплывающем диалоговом окне введите имя `HumanResources` и сохраните язык и региональные параметры по умолчанию (**на английском языке**). Описание оставьте пустым.

    ![Создание приложения LUIS под названием HumanResources](./media/luis-quickstart-intents-only/create-app.png)

    Далее появится страница **намерений** с намерением **None** (Отсутствует).

## <a name="getjobinformation-intent"></a>Намерение GetJobInformation

1. Выберите **Create new intent**. (Создать намерение). Введите имя нового намерения `GetJobInformation`. Это намерение предсказывается каждый раз, когда пользователь хочет получить сведения об открытых вакансиях в компании.

    ![Снимок экрана с диалоговым окном New intent (Новое намерение) Интеллектуальной службы распознавания речи (LUIS)](media/luis-quickstart-intents-only/create-intent.png "Screenshot of Language Understanding (LUIS) New intent dialog")

2. Предоставляя _примеры высказываний_, вы обучаете службу LUIS прогнозированию определенного типа высказываний для этого намерения. Добавьте в намерение несколько примеров высказываний, которые ожидаются от пользователя.

    | Примеры высказываний|
    |--|
    |Сегодня опубликованы новые вакансии?|
    |Какие должности доступны для старших инженеров?|
    |Есть ли должность для работы с базами данных?|
    |Ищу новую работу с обязанностями в бухгалтерском учете|
    |Где находится список вакансий|
    |Новые вакансии?|
    |Есть вакансии в офисе в Сиэтле?|

    [![Снимок экрана: ввод новых высказываний для намерения MyStore](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Screenshot of entering new utterances for MyStore intent")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    


## <a name="none-intent"></a>Намерение None 
Клиентскому приложению необходимо определить, находится ли высказывание за пределами домена субъекта приложения. Если LUIS возвращает намерение **None** (Отсутствует) для высказывания, клиентское приложение может спросить, хочет ли пользователь завершить разговор. Клиентское приложение может также дать больше указаний для продолжения разговора, если пользователь хочет продолжить. 

Эти примеры высказываний, находящиеся за пределами домена, группируются в намерение **None** (Отсутствует). Не оставляйте это намерение пустым. 

1. Выберите **намерения** на панели слева.

2. Выберите намерение **None**. Добавьте три высказывания, которые пользователь может ввести, но которые не имеют отношения к приложению "Управление персоналом". Если приложение связано с объявлениями о вакансиях, ниже приведены несколько примеров высказываний **None**.

    | Примеры высказываний|
    |--|
    |Громкий лай раздражает|
    |Закажи пиццу|
    |Пингвины в океане|


## <a name="train"></a>Train 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Опубликовать

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent"></a>Получение намерения

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Перейдите в конец URL-адреса в адресной строке и введите `I'm looking for a job with Natural Language Processing`. Последний параметр строки запроса — `q`. Это **запрос** фразы. Это высказывание не похоже ни на один из примеров высказываний. Это необходимая проверка, которая должна вернуть намерение `GetJobInformation` в качестве намерения с наибольшим уровнем совпадения. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

    Результаты включают в себя **все намерения** в приложении (в настоящее время — 2). Массив сущностей пуст, потому что в настоящее время у этого приложения нет никаких сущностей. 

    Результат JSON определяет намерение с наивысшим показателем как свойство **`topScoringIntent`**. Все оценки находятся в диапазоне 0–1, наилучшие оценки — те, которые ближе к 1. 

## <a name="applyforjob-intent"></a>Намерение ApplyForJob
Вернитесь на веб-сайт LUIS и создайте новое намерение, чтобы определить, является ли пользовательское высказывание заявкой на работу.

1. Выберите **Создать** в меню в правом верхнем углу, чтобы вернуться к созданию приложения.

2. Выберите **намерения** в меню слева.

3. Выберите **Создать намерение** и введите имя `ApplyForJob`. 

    ![Диалоговое окно LUIS для создания намерения](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Добавьте в намерение несколько фраз, которые вы ожидаете от пользователя, например:

    | Примеры фраз|
    |--|
    |Я хочу подать заявку на вакансию в бухгалтерии|
    |Заполнить заявку для вакансии 123456|
    |Отправить резюме на должность инженера|
    |Это мое резюме должность 654234|
    |Должность 567890 и мои документы|

    [![Снимок экрана: ввод новых высказываний для намерения ApplyForJob](media/luis-quickstart-intents-only/utterance-applyforjob.png "Screenshot of entering new utterances for MyStore intent")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    Помеченное намерение выделено красным цветом, так как в настоящее время приложение LUIS не уверено в правильности намерения. При обучении приложение сообщает LUIS, что эти фразы выражают верное намерение. 

## <a name="train-again"></a>Повторное обучение

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Повторная публикация

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-again"></a>Повторное получение намерения

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. В новом окне браузера введите `Can I submit my resume for job 235986` в конце URL-адреса. 

    ```json
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

    Результаты включают в себя новое намерение **ApplyForJob**, а также существующие намерения. 

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве создано приложение "Управление персоналом" и 2 намерения, добавлены примеры высказываний для каждого намерения, в том числе, для намерения "None", которые обучены, опубликованы и протестированы на конечной точке. Ниже приведены основные этапы создания модели LUIS. 

> [!div class="nextstepaction"]
> [Добавление предварительно созданных намерений и сущностей к этому приложению](luis-tutorial-prebuilt-intents-entities.md)
