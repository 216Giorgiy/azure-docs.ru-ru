---
title: Управление учетными данными в Azure Content Moderator — инструмент проверки Content Moderator
titlesuffix: Azure Cognitive Services
description: Управляйте учетными данными в Content Moderator, которые вам понадобятся для работы с API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0da6b6b0fef0f998e20789253b2a65c54121532c
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260015"
---
# <a name="manage-content-moderator-service-credentials"></a>Управление учетными данными службы Content Moderator

Учетные данные Content Moderator можно создать с помощью следующих средств:

- [Портал Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Инструмент проверки Content Moderator](https://contentmoderator.cognitive.microsoft.com/)

В этой статье объясняется, где их можно найти и как они связаны между собой.

## <a name="the-azure-portal"></a>Портал Azure

На панели мониторинга портала Azure выберите свою учетную запись Content Moderator. В разделе **Управление ресурсами** выберите **Ключи**. Чтобы скопировать ключ, щелкните значок справа от ключа.

![Ключи Content Moderator на портале Azure](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>Использование учетной записи Azure с инструментом проверки и API проверки
Чтобы использовать свой ключ Azure с интерфейсами API проверки, скопируйте идентификатор ресурса, указанный на экране **Свойства** на приведенном ниже снимке экрана. Затем введите его на экране учетных данных инструмента проверки в поля **Whitelisted Resource Id(s)** (Разрешенные идентификаторы ресурсов), как показано в следующем разделе **Идентификатор ресурса**. 

> [!NOTE]
> Регион подписки Content Moderator должен совпадать с регионом команды проверки, чтобы обеспечить распознавание команды и доступ к ее данным. Например, в изображениях на этой странице регион **Западная часть США** **(4)** содержит подписку Azure для Content Moderator и вашу команду проверки.
>
> После ввода в инструмент проверки ключа и идентификатора ресурса из подписки Azure ключ **Ocp-Apim-Subscription-Key** для пробной версии, отображенный на экране "Credentials" (Учетные данные), больше не используется, но всегда будет доступен.
> Ключ пробной версии позволяет выполнить до 5000 транзакций в месяц с частотой в 1 запрос в секунду.

![Идентификатор ресурса Content Moderator на портале Azure](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>Использование учетной записи Azure с рабочими процессами в инструменте проверки

Чтобы использовать свой ключ Azure для рабочих процессов, доступных в Content Moderator, введите его в поле **Ocp-Apim-Subscription-Key** в разделе **Workflow Settings** (Параметры рабочего процесса), как показано в разделе **Рабочие процессы** ниже. Щелкните **+**, чтобы сохранить свой идентификатор ресурса.

![Учетные данные рабочего процесса Content Moderator в инструменте проверки](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>Инструмент проверки

На панели мониторинга инструмента проверки на вкладке **Settings** (Параметры) выберите **Credentials** (Учетные данные).

![Учетные данные Content Moderator в инструменте проверки](images/credentials-trial-resource-workflow.PNG)

В следующем разделе более подробно рассматривается приведенное выше изображение.

### <a name="api"></a>API

В первой части указаны **конечная точка API проверки**, **идентификатор команды** и **Ocp-Apim-Subscription-Key** (ключ пробной версии Content Moderator, созданный вместе с командой проверки). Используйте их для вызова всех интерфейсов API Content Moderator, включая API проверки.

Также запишите свой идентификатор региона для конечной точки API. Например **westus** — регион, в «https:\//westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0»

![Ключ Content Moderator в инструменте проверки](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Идентификатор ресурса

Этот набор полей описан в предыдущем разделе, [Использование учетной записи Azure со средством проверки и API](credentials.md#use-the-azure-account-with-the-review-tool-and-review-api). Это поле обычно пусто, если вы не добавите в него идентификатор ресурса Azure, как описано в предыдущем разделе.

### <a name="workflows"></a>Рабочие процессы

Этот набор полей описан в предыдущем разделе, [Использование учетной записи Azure с рабочими процессами в средстве проверки](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool). По умолчанию инструмент проверки использует для выполнения рабочих процессов автоматически созданный ключ пробной версии, который отображается в самом начале. С помощью других двух полей можно использовать списки терминов и изображений для операций проверки текста и оценки изображений соответственно.

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте, как использовать учетные данные Content Moderator в своих [рабочих процессах](workflows.md).
