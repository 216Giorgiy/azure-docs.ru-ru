---
title: Краткое руководство. Создание клиента модератора для .NET — Content Moderator
titlesuffix: Azure Cognitive Services
description: Описывается, как вернуть клиент Content Moderator с помощью пакета SDK Azure Content Moderator для .NET.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 08902df231fedd2c1c5653052540cb1cdabba9cf
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224949"
---
# <a name="quickstart-helper-code-to-return-a-content-moderator-client"></a>Краткое руководство. Вспомогательный код для получения клиента Content Moderator

В этой статье содержатся сведения и примеры кода, которые помогут приступить к работе с пакетом SDK Content Moderator для .NET, чтобы создать клиент Content Moderator для своей подписки.

Приведенная библиотека используется в других кратких руководствах этого раздела.

В этой статье предполагается, что вы уже работали с Visual Studio и C#.

> [!IMPORTANT]
> Эта библиотека классов содержит код, предназначенный исключительно для демонстрационных целей.
> В случае адаптации этого кода для рабочей среды используйте безопасный способ хранения и использования ключа подписки Content Moderator.

## <a name="sign-up-for-content-moderator-services"></a>Регистрация в службах Content Moderator

Прежде чем использовать службы Content Moderator через REST API или пакет SDK, необходимо получить ключ подписки.
Сведения о способах получения ключа см. в статье [Краткое руководство. Знакомство с Content Moderator](quick-start.md).

## <a name="create-your-visual-studio-project"></a>Создание проекта Visual Studio

1. Создайте проект на основе **библиотеки классов (.NET Framework)**.

   Проект в примере кода называется **ModeratorHelper**.

1. Добавьте ссылку на сборку платформы **System.Configuration**.

### <a name="install-required-packages"></a>Установка необходимых пакетов

Установите следующие пакеты NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator;
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json.

### <a name="create-the-content-moderator-client"></a>Создание клиента Content Moderator

Замените содержимое файла ModeratorHelper.cs приведенным ниже кодом.

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Укажите в полях **AzureRegion** и **CMSubscriptionKey** значения идентификатора региона и ключа подписки соответственно.

Теперь вы можете быстро создать клиент Content Moderator для своей подписки.

## <a name="next-steps"></a>Дополнительная информация

[Скачайте решение Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) для этого и других кратких руководств по Content Moderator для .NET и приступите к интеграции.
