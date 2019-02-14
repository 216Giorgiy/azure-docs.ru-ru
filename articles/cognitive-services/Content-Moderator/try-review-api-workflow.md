---
title: Рабочие процессы для модерации контента из консоли API — Content Moderator
titlesuffix: Azure Cognitive Services
description: Используйте операции рабочего процесса в Azure Content Moderator, чтобы создать, обновить рабочий процесс или получить сведения о рабочем процессе с помощью API проверки.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 1c18544a0fd135eb546660c442b865bf1249dfe5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883090"
---
# <a name="workflows-from-the-api-console"></a>Использование рабочих процессов из консоли API

Используйте [операции рабочего процесса](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) в Azure Content Moderator, чтобы создать, обновить рабочий процесс или получить сведения о рабочем процессе с помощью API проверки. С помощью этого API можно определить простые, сложные и даже вложенные выражения для рабочих процессов. Рабочие процессы отображаются в инструменте проверки и могут использоваться вашей командой. Рабочие процессы также используются в операциях заданий API проверки.

## <a name="prerequisites"></a>Предварительные требования

1. Перейдите к [инструменту проверки](https://contentmoderator.cognitive.microsoft.com/). Зарегистрируйтесь, если вы этого еще не сделали. 
2. В инструменте проверки в разделе **Settings** (Параметры) выберите вкладку **Workflows** (Рабочие процессы), как показано в [руководстве по рабочим процессам](Review-Tool-User-Guide/Workflows.md) инструмента проверки.

### <a name="browse-to-the-workflows-screen"></a>Переход к разделу "Workflows" (Рабочие процессы)

На панели мониторинга Content Moderator выберите **Review** (Проверка)  >  **Settings** (Параметры)  >  **Workflows** (Рабочие процессы). Вы увидите рабочий процесс по умолчанию.

  ![Рабочий процесс по умолчанию](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Получение определения JSON рабочего процесса по умолчанию

Выберите параметр **Edit** (Изменить) для рабочего процесса, а затем выберите вкладку **JSON**. Отобразится приведенное ниже выражение JSON.

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

## <a name="get-workflow-details"></a>Получение сведений о рабочем процессе

С помощью операции **Workflow - Get** можно получить сведения о существующем рабочем процессе по умолчанию.

В инструменте проверки перейдите к разделу [Credentials](Review-Tool-User-Guide/credentials.md#the-review-tool) (Учетные данные).

### <a name="browse-to-the-api-reference"></a>Переход к справочнику по API

1. В представлении **Credentials** (Учетные данные) выберите [API reference](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) (Справочник по API). 
2. Когда откроется страница **Workflow - Create Or Update** (Рабочий процесс — создание или обновление), перейдите к справочнику [Workflow - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) (Рабочий процесс — получение).

### <a name="select-your-region"></a>Выбор региона

Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения.

  ![Выбор региона на странице "Workflow - Get" (Рабочий процесс — получение)](images/test-drive-region.png)

  Откроется консоль API **Workflow - Get** (Рабочий процесс — получение).

### <a name="enter-parameters"></a>Ввод параметров

Введите значения параметров **team**, **workflowname** и **Ocp-Apim-Subscription-Key** (ваш ключ подписки).

- **team**: идентификатор группы, созданной при настройке [учетной записи средства проверки](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: имя рабочего процесса. C помощью `default`.
- **Ocp-Apim-Subscription-Key**: отображается на вкладке **Settings** (Параметры). Дополнительные сведения см. в этом [обзоре](overview.md).

  ![Получение параметров запроса и заголовков](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Отправка запроса
  
Нажмите кнопку **Отправить**. Если операция выполнена успешно, **Response status** (Состояние ответа) имеет значение `200 OK`, а в поле **Response content** (Содержимое ответа) отображается следующий рабочий процессе в формате JSON.

    {
        "Name": "default",
        "Description": "Default",
        "Type": "Image",
        "Expression": {
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isadult",
            "Operator": "eq",
            "Value": "true",
            "AlternateInput": null,
            "Type": "Condition"
            },
        "Then": {
            "Perform": [{
                "Name": "createreview",
                "Subteam": null,
                "CallbackEndpoint": null,
                "Tags": []
            }],
            "Type": "Actions"
            },
            "Else": null,
            "Type": "Logic"
            }
    }


## <a name="create-a-workflow"></a>Создание рабочего процесса

В инструменте проверки перейдите к разделу [Credentials](Review-Tool-User-Guide/credentials.md#the-review-tool) (Учетные данные).

### <a name="browse-to-the-api-reference"></a>Переход к справочнику по API

В представлении **Credentials** (Учетные данные) выберите [API reference](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) (Справочник по API). Откроется страница **Workflow - Create Or Update** (Рабочий процесс — создание или обновление).

### <a name="select-your-region"></a>Выбор региона

Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения.

  ![Выбор региона на странице "Workflow - Create Or Update" (Рабочий процесс — создание или обновление)](images/test-drive-region.png)

  Откроется консоль API **Workflow - Create Or Update** (Рабочий процесс — создание или обновление).

### <a name="enter-parameters"></a>Ввод параметров

Введите значения параметров **team**, **workflowname** и **Ocp-Apim-Subscription-Key** (ваш ключ подписки).

- **team**: идентификатор группы, созданной при настройке [учетной записи средства проверки](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: Имя нового рабочего процесса.
- **Ocp-Apim-Subscription-Key**: отображается на вкладке **Settings** (Параметры). Дополнительные сведения см. в этом [обзоре](overview.md).

  ![Параметры запроса и заголовки в консоли "Workflow - Create Or Update" (Рабочий процесс — создание или обновление)](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Ввод определения рабочего процесса

1. Измените содержимое поля **Request body** (Текст запроса), чтобы ввести запрос в формате JSON с параметрами **Description** (Описание) и **Type** (Тип) (изображение или текст). 
2. В поле **Expression** (Выражение) скопируйте выражение рабочего процесса по умолчанию из предыдущего раздела, как показано ниже.

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    Текст запроса будет выглядеть как приведенный ниже запрос в формате JSON.

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": {
                "Type": "Logic",
                "If": {
                    "ConnectorName": "moderator",
                    "OutputName": "isAdult",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                    },
                "Then": {
                "Perform": [
                {
                    "Name": "createreview",
                    "CallbackEndpoint": null,
                    "Tags": [ ]
                }
                ],
                "Type": "Actions"
                }
            }
        }
 
### <a name="submit-your-request"></a>Отправка запроса
  
Нажмите кнопку **Отправить**. Если операция выполнена успешно, **Response status** (Состояние ответа) имеет значение `200 OK`, а в поле **Response content** (Содержимое ответа) отображается `true`.

### <a name="check-out-the-new-workflow"></a>Проверка нового рабочего процесса

В инструменте проверки выберите **Review** (Проверка)  >  **Settings** (Параметры)  >  **Workflows** (Рабочие процессы). Новый рабочий процесс будет отображен на странице и готов к использованию.

  ![Список рабочих процессов в инструменте проверки](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Просмотр сведений о новом рабочем процессе

1. Выберите параметр **Edit** (Изменить) для рабочего процесса, а затем выберите вкладки **Designer** (Конструктор) и **JSON**.

   ![Вкладка "Designer" (Конструктор) для выбранного рабочего процесса](images/workflow-console-new-workflow-designer.PNG)

2. Чтобы просмотреть представление JSON рабочего процесса, выберите вкладку **JSON**.

## <a name="next-steps"></a>Дополнительная информация

* Более сложные примеры рабочих процессов приведены в разделе [Рабочие процессы модерации](workflow-api.md).
* Узнайте, как использовать рабочие процессы с [заданиями модерации контента](try-review-api-job.md).
