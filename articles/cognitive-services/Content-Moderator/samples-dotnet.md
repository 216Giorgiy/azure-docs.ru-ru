---
title: Примеры кода Azure Content Moderator | Документация Майкрософт
description: Использование Content Moderator в приложениях
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2018
ms.author: sajagtap
ms.openlocfilehash: a7f5b0a7433631e303de47667871cc1354053c08
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380804"
---
# <a name="net-sdk-samples"></a>Примеры пакета SDK для .NET

Ниже приведен список ссылок на примеры кода, созданные с помощью пакета SDK Azure Content Moderator для .NET.

- **Вспомогательная библиотека**: [создание клиента Content Moderator для использования в других примерах](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). См. [краткое руководство](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Модерация

- **Модерация изображений**: [оценка изображения на наличие содержимого для взрослых, неприличного содержимого, текста и лиц](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). См. [краткое руководство](image-moderation-quickstart-dotnet.md).
- **Пользовательские изображения**: [модерация по спискам пользовательских изображений](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). См. [краткое руководство](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Существует максимальное ограничение в **5 списков изображений**, каждый из которых может содержать **не более 10 000 изображений**.
>

- **Модерация текста**: [проверка текста на наличие ненормативной лексики и персональных данных (PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). См. [краткое руководство](text-moderation-quickstart-dotnet.md).
- **Настраиваемые термины**: [модерация по спискам настраиваемых терминов](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). См. [краткое руководство](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Существует максимальное ограничение в **5 списков терминов**, каждый из которых может содержать **не более 10 000 терминов**.
>

- **Модерация видео**: [сканирование видео на наличие содержимого для взрослых и неприличного содержимого и получение результатов](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). См. [краткое руководство](video-moderation-api.md).

## <a name="review"></a>Проверка

- **Задания по изображениям**: [запуск задания модерации, выполняющего сканирование и создающего проверки](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). См. [краткое руководство](moderation-jobs-quickstart-dotnet.md).
- **Проверки изображений**: [создание проверок с участием человека](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). См. [краткое руководство](moderation-reviews-quickstart-dotnet.md).
- **Проверки видео**: [создание проверок с участием человека для видео](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). См. [краткое руководство](video-reviews-quickstart-dotnet.md).
- **Проверки расшифровок видео**: [создание проверок с участием человека для расшифровок видео](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs). См. [краткое руководство](video-reviews-quickstart-dotnet.md).

Все примеры для .NET можно просмотреть на странице [примеров Content Moderator для .NET на GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).
