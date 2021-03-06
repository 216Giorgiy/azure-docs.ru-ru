---
title: Моментальный снимок на определенный момент времени в службе "Конфигурация приложений Azure" | Документация Майкрософт
description: Общие сведения о функционировании моментального снимка на определенный момент времени в службе "Конфигурация приложений Azure"
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 6238f96c9e8df0431e42caa5e5846af3fc60e681
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484021"
---
# <a name="point-in-time-snapshot"></a>Моментальный снимок на определенный момент времени

В службе "Конфигурация приложений Azure" сохраняются записи с точным временем создания новой пары "ключ — значение" и ее изменения в дальнейшем. Эти записи образуют полную временную шкалу изменений пары "ключ — значение". С помощью хранилища конфигураций приложений можно восстановить журнал пары "ключ — значение" и воспроизвести ее последнее значение в любой момент времени до текущего. Эта функция позволяет "переместиться назад во времени" и получить старую пару "ключ — значение". Например, вы можете получить вчерашние параметры конфигурации, которые были активны непосредственно перед последним развертыванием, чтобы восстановить предыдущую конфигурацию и откатить приложение.

## <a name="key-value-retrieval"></a>Получение пары "ключ — значение"

Для получения прошлой пары "ключ — значение" укажите время, когда был создан моментальный снимок этой пары, в заголовке HTTP вызова REST API. Например: 

        GET /kv HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Сейчас в службе "Конфигурация приложений" сохраняется журнал изменений за 7 дней.

## <a name="next-steps"></a>Дополнительная информация

* [Краткое руководство Создание веб-приложения ASP.NET](quickstart-aspnet-core-app.md)  
