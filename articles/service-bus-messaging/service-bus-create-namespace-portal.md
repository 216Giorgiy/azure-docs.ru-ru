---
title: Как создать пространство имен служебной шины с помощью портала Azure | Документация Майкрософт
description: Создание пространства имен служебной шины с помощью портала Azure.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/29/2018
ms.author: sethm
ms.openlocfilehash: 2763e401454cdb6145067a3ac415c3a252d7c494
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38630087"
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Создание пространства имен служебной шины с помощью портала Azure

Пространство имен — это контейнер, определяющий область действия для всех компонентов обмена сообщениями. В одном пространстве имен могут содержаться несколько очередей и разделов. Часто пространства имен выполняют роль контейнеров приложений. Сейчас создать пространство имен служебной шины можно двумя способами.

1. Портал Azure (в этой статье)
2. [Шаблоны Resource Manager][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Создание пространства имен на портале Azure

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Поздравляем! Вы создали пространство имен служебной шины, предназначенное для обмена сообщениями.

## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь с [примерами GitHub][github-samples], демонстрирующими расширенные возможности обмена сообщениями служебной шины.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
