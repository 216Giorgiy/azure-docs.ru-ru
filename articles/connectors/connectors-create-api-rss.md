---
title: Соединитель RSS в Azure Logic Apps | Документация Майкрософт
description: Создание приложений логики с помощью службы приложений Azure. Соединитель RSS позволяет пользователям публиковать и извлекать элементы веб-канала. Он также позволяет пользователям активировать операции при публикации нового элемента в веб-канале.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: a10a6277-ed29-4e68-a881-ccdad6fd0ad8
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 493ee8a961e5d5a57c6276868a55b524dec28018
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-rss-connector"></a>Начало работы с соединителем RSS
RSS — это популярный формат веб-синдикации, используемый для публикации часто обновляемого содержимого (записей блогов и заголовков новостей).  Многие издатели содержимого предоставляют RSS-канал, чтобы пользователи могли на него подписаться.  Соединитель RSS можно использовать для извлечения сведений о канале и активации потоков при публикации новых элементов в RSS-канале.

Для начала можно создать приложение логики, как описано [здесь](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-rss"></a>Создание подключения к RSS
> [!INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]
> 

## <a name="connector-specific-details"></a>Сведения о соединителях

Информацию о существующих ограничениях, а также о триггерах и действиях, определенных в Swagger, см. в статье со [сведениями о соединителях](/connectors/rss/).

## <a name="more-connectors"></a>Дополнительные сведения о соединителях
Вы можете вернуться к [списку интерфейсов API](apis-list.md).