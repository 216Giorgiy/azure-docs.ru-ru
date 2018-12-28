---
title: Модель данных телеметрии Azure Application Insights — телеметрия трассировки | Документы Майкрософт
description: Модель данных Application Insights для телеметрии трассировки
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: f87b2d1bd105a9a9b8eabb3f2f6686c36f71e6df
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721737"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Телеметрия трассировки. Модель данных Application Insights

Телеметрия трассировки (в [Application Insights](app-insights-overview.md)) представляет инструкции трассировки стиля `printf`, доступные для текстового поиска. `Log4Net`, `NLog` и другие записи файла журнала на основе текста преобразуются в экземпляры этого типа. Трассировка не имеет измерений в качестве расширения.

## <a name="message"></a>Сообщение

Сообщение трассировки.

Максимальная длина 32 768 знаков

## <a name="severity-level"></a>Уровень серьезности

Уровень серьезности трассировки. Возможные значения: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Пользовательские свойства

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Дополнительная информация

- [Просмотр журналов трассировки .NET в Application Insights](app-insights-asp-net-trace-logs.md).
- [Просмотр журналов трассировки Java в Application Insights](app-insights-java-trace-logs.md).
- В [этой статье](application-insights-data-model.md) представлены типы данных и модель данных для Application Insights.
- [Написание пользовательской телеметрии трассировки](app-insights-api-custom-events-metrics.md#tracktrace)
- Ознакомление с [платформами](app-insights-platforms.md), поддерживаемыми Application Insights.
