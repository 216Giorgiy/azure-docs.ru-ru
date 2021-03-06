---
title: Пакеты SDK Служб мультимедиа Azure версии 3 в Azure
description: В этой статье представлен обзор того, как приступить к разработке с помощью API Служб мультимедиа Azure версии 3, используя пакеты SDK.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 04/11/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9fb4d1561a661387f759aada9e776d43a95aa5c7
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564515"
---
# <a name="develop-against-media-services-v3-api-using-sdks"></a>Разработка с использованием API Служб мультимедиа версии 3 с помощью пакетов SDK

Как разработчик вы можете использовать [REST API](https://aka.ms/ams-v3-rest-ref) Служб мультимедиа или клиентские библиотеки, которые позволяют взаимодействовать с REST API для простого создания, контроля и обслуживания настраиваемых рабочих процессов. API [Служб мультимедиа версии 3](https://aka.ms/ams-v3-rest-sdk) основан на спецификации OpenAPI (ранее известной как Swagger).

> [!NOTE]
> Пакеты SDK для Служб мультимедиа Azure версии 3 могут не обеспечивать нужную безопасность для потоков. При разработке приложений с несколькими потоками вам следует добавить собственную логику синхронизации потоков, чтобы защитить клиент, или использовать новый объект AzureMediaServicesClient для каждого потока. Также помните, что предоставляемые кодом клиенту дополнительные объекты (например, экземпляр HttpClient в .NET) могут создавать проблемы при обработке нескольких потоков.

В этом разделе приведены ссылки на пакеты SDK, инструменты и практические руководства.

## <a name="prerequisites"></a>Предварительные требования

Чтобы начать разработку с использованием Служб мультимедиа, вам потребуется:

- Активная подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начинать работу.
- [Learn about fundamental concepts](concepts-overview.md) (Основные понятия служб мультимедиа)
- Ознакомьтесь с [разработкой с помощью интерфейсов API Служб мультимедиа версии 3](media-services-apis-overview.md).
- [Создание учетной записи Служб мультимедиа с помощью CLI](create-account-cli-how-to.md)

## <a name="start-developing-with-sdks"></a>Начало разработки с использованием пакетов SDK

### <a name="net"></a>.NET

Используйте [пакет SDK для .NET](https://aka.ms/ams-v3-dotnet-sdk) для [подключения к Службам мультимедиа](configure-connect-dotnet-howto.md).

Ознакомьтесь со [справочной документации по .NET](https://aka.ms/ams-v3-dotnet-ref) для Служб мультимедиа.

### <a name="java"></a>Java

Используйте [пакет SDK для Java](https://aka.ms/ams-v3-java-sdk) для [подключения к Службам мультимедиа](configure-connect-java-howto.md).

Ознакомьтесь со [справочной документацией по Java](https://aka.ms/ams-v3-java-ref) для Служб мультимедиа.

### <a name="nodejs"></a>Node.js

Используйте [пакет SDK для Node.js](https://aka.ms/ams-v3-nodejs-sdk) для [подключения к Службам мультимедиа](configure-connect-nodejs-howto.md).

Ознакомьтесь со [справочной документацией по Node.js](https://aka.ms/ams-v3-nodejs-ref) для Служб мультимедиа и изучите [примеры](https://github.com/Azure-Samples/media-services-v3-node-tutorials), показывающие, как использовать API Служб мультимедиа с Node.js.

### <a name="python"></a>Python

Воспользуйтесь [пакетом SDK для Python](https://aka.ms/ams-v3-python-sdk).

Ознакомьтесь со [справочной документации по Python](https://aka.ms/ams-v3-python-ref) для Служб мультимедиа.

### <a name="go"></a>Go

Воспользуйтесь [пакетом SDK для Go](https://aka.ms/ams-v3-go-sdk).

Ознакомьтесь со [справочной документации по Go](https://aka.ms/ams-v3-go-ref) для Служб мультимедиа.

### <a name="ruby"></a>Ruby

Воспользуйтесь [пакетом SDK для Ruby](https://aka.ms/ams-v3-ruby-sdk).

## <a name="azure-media-services-explorer"></a>Обозреватель Служб мультимедиа Azure

[Обозреватель Служб мультимедиа Azure](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) — это инструмент, доступный для клиентов Windows, которые хотят узнать о Службах мультимедиа. AMSE — это приложение Winforms или C#, которое отправляет, скачивает, кодирует, транслирует видео по запросу и в режиме реального времени с помощью Служб мультимедиа. Инструмент AMSE предназначен для клиентов, которые хотят протестировать Службы мультимедиа без написания кода. Код AMSE предоставляется как ресурс для клиентов, которым необходима разработка с помощью Служб мультимедиа.

AMSE — это проект с открытым кодом, поддержка которого предоставляется сообществом (о проблемах можно сообщать по ссылке https://github.com/Azure/Azure-Media-Services-Explorer/issues). Microsoft Open Source Code of Conduct [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). Дополнительные сведения см. в [вопросах и ответах по правилам поведения](https://opensource.microsoft.com/codeofconduct/faq/). С любыми другими вопросами или комментариями обращайтесь по адресу opencode@microsoft.com.

## <a name="next-steps"></a>Дополнительная информация

[Обзор](media-services-overview.md)
