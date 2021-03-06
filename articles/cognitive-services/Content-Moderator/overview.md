---
title: Что такое Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Узнайте, как использовать Content Moderator для отслеживания, пометки, оценки и фильтрации неподходящего материала, создаваемого пользователями.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 440471acb6e122bf25ba21b0ab3b5a2f7d9b021d
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758129"
---
# <a name="what-is-azure-content-moderator"></a>Что такое Azure Content Moderator?

API Azure Content Moderator — это когнитивная служба, которая проверяет текст, изображения и видео на наличие материалов, потенциально оскорбительных, представляющих риск или нежелательных по иным причинам. При обнаружении таких материалов служба применяет к содержимому соответствующие метки (флаги). Затем приложение может обрабатывать помеченное содержимое для обеспечения соответствия нормативным требованиям или оставлять его как предполагаемую среду для пользователей. Сведения о том, что означают различные флаги содержимого, см. в разделе [Интерфейсы API](#content-moderator-apis).

## <a name="where-it-is-used"></a>Сценарии использования

Ниже приведены несколько сценариев, в которых разработчик программного обеспечения или команда используют Content Moderator:

- интернет-магазины с модерацией каталогов продукции и создаваемого пользователями содержимого;
- компании-разработчики игр, которые модерируют создаваемые пользователями игровые артефакты и чаты;
- социальная платформа обмена сообщениями с модерацией изображений, текста и видео, добавляемых пользователями;
- корпоративные медиакомпании, внедряющие централизованную модерацию своего содержимого;
- поставщики образовательных решений K-12, фильтрующие неуместное содержимое для учащихся и преподавателей.

## <a name="what-it-includes"></a>Компоненты

Служба Content Moderator состоит из нескольких API веб-службы, доступных через вызовы REST и пакет SDK для .NET. Она также содержит средство пользовательской проверки, с помощью которого человек может помочь службе и улучшить или оптимизировать ее функцию модерации.

![Блок-схема для Content Moderator, отображающая API модерации, API проверки и средство пользовательской проверки](images/content-moderator-block-diagram.png)

### <a name="content-moderator-apis"></a>API Content Moderator

Служба Content Moderator включает API для следующих сценариев.

| Группа API | ОПИСАНИЕ |
| ------ | ----------- |
|[**Модерация текста**](text-moderation-api.md)| Просматривает текст на наличие оскорбительного содержимого, содержимого сексуального или непристойного характера, содержимого с ненормативной лексикой и персональными данными.|
|[**Пользовательские списки терминов**](try-terms-list-api.md)| Проверяет текст с использованием пользовательского списка терминов помимо встроенных терминов. Используйте пользовательские списки для блокировки или разрешения содержимого в соответствии с собственными политиками содержимого.|  
|[**Модерация изображений**](image-moderation-api.md)| Проверяет изображения на наличие непристойного содержимого или содержимого для взрослых, выявляет в изображениях текст с помощью функции распознавания текста, а также распознает лица.|
|[**Пользовательские списки изображений**](try-image-list-api.md)| Проверяет изображения с использованием пользовательского списка изображений. Используйте пользовательский список изображений для фильтрации экземпляров часто встречающегося содержимого, которое вы не хотите классифицировать еще раз.|
|[**Модерация видео**](video-moderation-api.md)| Проверяет видео на наличие непристойного содержимого или содержимого для взрослых и возвращает метки времени для указанного содержимого.|
|[**Просмотр API-интерфейсов**](try-review-api-job.md)| Используйте операции [Задания](try-review-api-job.md), [Проверки](try-review-api-review.md) и [Рабочий процесс](try-review-api-workflow.md) для создания и автоматизации рабочих процессов с участием человека с использованием средства пользовательской проверки. API рабочего процесса еще недоступны в пакете SDK для .NET.|

### <a name="review-tool"></a>Средство проверки

Служба Content Moderator также имеет [средство проверки](Review-Tool-User-Guide/human-in-the-loop.md), в котором находятся результаты проверки для обработки людьми. Пользовательские входные данные не обучают службу, но объединенная работа службы и команд пользовательской проверки позволяет разработчикам найти баланс между эффективностью и точностью. Средство проверки также имеет удобный интерфейс, в котором доступны разнообразные ресурсы Content Moderator.

![Домашняя страница средства пользовательской проверки Content Moderator](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Конфиденциальность и безопасность данных

Как и в случае со всеми другими Cognitive Services, разработчикам, использующим службу Content Moderator, следует учитывать политику корпорации Майкрософт касательно клиентских данных. Дополнительные сведения см. на [странице о Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) Центра управления безопасностью Майкрософт.

## <a name="next-steps"></a>Дополнительная информация

Инструкции по началу работы со службой Content Moderator см. в статье [Краткое руководство. Знакомство с Content Moderator](quick-start.md).