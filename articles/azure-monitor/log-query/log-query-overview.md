---
title: Анализ данных Log Analytics в Azure Monitor | Документация Майкрософт
description: Чтобы получить данные из Log Analytics, требуется поиск по журналам.  В этой статье описывается использование новых поисков по журналам в Log Analytics и приведены основные сведения, которые необходимо знать перед их созданием.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.openlocfilehash: bc37b3b60a5ad7f4e2b4794e4fcb74c1a5004b75
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336889"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Анализ данных Log Analytics в Azure Monitor

Данные журнала, собранные Azure Monitor, хранятся в рабочей области Log Analytics, которая основана на [Azure Data Explorer](/azure/data-explorer). Azure Monitor собирает телеметрию из разных источников и использует [язык запросов из Data Explorer](/azure/kusto/query) для извлечения и анализа данных.

> [!NOTE]
> Log Analytics ранее рассматривалась как собственная служба в Azure. Она считается частью Azure Monitor и сфокусирована на хранении и анализе данных журнала с помощью языка запросов. Компоненты, которые были частью Log Analytics, такие как агенты Windows и Linux для сбора данных, просмотры для визуализации существующих данных и оповещения для предварительного уведомления о проблемах, не изменились, но теперь они считаются частью Azure Monitor.



## <a name="log-queries"></a>Запросы журнала

Чтобы получить данные из Log Analytics, требуется запрос к журналам.  Чтобы указать нужные данные, вы будете использовать запрос независимо от того, что вы делаете: [анализируете данные на портале](../../azure-monitor/log-query/portals.md), [настраиваете правило генерации оповещений](../../azure-monitor/platform/alerts-metric.md) для уведомления о конкретном состоянии или получаете данные с помощью [API Log Analytics](https://dev.loganalytics.io/).  В этой статье описываются запросы к журналам в Log Analytics и предоставляются основные сведения, которые необходимо знать перед их созданием.



## <a name="where-log-queries-are-used"></a>Использование запросов к журналу

Ниже приведены различные способы использования запросов к журналам в Log Analytics.

- **Порталы.** На [портале Azure](../../azure-monitor/log-query/portals.md) можно выполнять интерактивный анализ данных журнала.  Так вы можете изменить запрос и анализировать результаты в различных форматах и визуализациях.  
- **Правила генерации оповещений.** [Правила генерации оповещений](../../azure-monitor/platform/alerts-overview.md) заранее выявляют проблемы с данными в рабочей области.  Каждое правило генерации оповещений основано на поиске по журналам, который автоматически выполняется через определенные интервалы.  Результаты проверяются, чтобы определить, следует ли создавать оповещение.
- **Панели мониторинга.** Результаты любого запроса можно закрепить на [панели мониторинга Azure](../../azure-monitor/platform/dashboards.md), что даст вам возможность визуализировать данные журналов и метрик вместе и при необходимости использовать совместно с другими пользователями Azure. 
- **Представления.**  Вы можете создавать визуализации данных, которые добавлены на панели мониторинга пользователя, с помощью [конструктора представлений](../../azure-monitor/platform/view-designer.md).  Запросы к журналам предоставляют данные, используемые [плитками](../../azure-monitor/platform/view-designer-tiles.md) и [элементами визуализации](../../azure-monitor/platform/view-designer-parts.md) в каждом просмотре.  
- **Экспорт.**  Когда вы экспортируете данные из рабочей области Log Analytics в Excel или [Power BI](../../azure-monitor/platform/powerbi.md), вы создаете запрос к журналам для определения экспортируемых данных.
- **PowerShell.** Вы можете запустить скрипт PowerShell из командной строки или модуль Runbook службы автоматизации Azure, использующий командлет [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0), для получения данных из Log Analytics.  Для этого командлета требуется запрос, чтобы определить извлекаемые данные.
- **API Log Analytics.**  [API поиска по журналам службы Log Analytics](../../azure-monitor/platform/alerts-overview.md) позволяет любому клиенту REST API извлекать данные из рабочей области.  Запрос API включает запрос, который выполняется в Log Analytics, чтобы определить извлекаемые данные.

![Поиск по журналам](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Напишите запрос
Log Analytics использует [версию языка запросов Data Explorer](../../azure-monitor/log-query/get-started-queries.md) для извлечения и анализа данных журнала различными способами.  Обычно начинают с основных запросов, а затем переходят к использованию более усовершенствованных функций, так как требования становятся более сложными.

Базовая структура запроса — это исходная таблица и ряд операторов, разделенных символом вертикальной черты `|`.  Вы можете объединить в цепочку несколько операторов для уточнения данных и выполнения расширенных функций.

Например, предположим, необходимо найти первые десять компьютеров с наибольшим количеством событий ошибок за последний день.

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

Или нужно найти компьютеры, которые не были активными в течение последнего дня.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

Как насчет графика, отражающего использование процессора на каждом компьютере за прошлую неделю?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

Из этих примеров можно увидеть, что независимо от типа данных, с которыми вы работаете, структура запроса является аналогичной.  Ее можно разбить на отдельные действия, где полученные данные из одной команды отправляются через конвейер в следующую команду.

Кроме того, вы можете запрашивать данные из рабочих областей Log Analytics в рамках своей подписки.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-log-analytics-data-is-organized"></a>Упорядочивание данных в Log Analytics
При создании запроса сначала нужно определить, какие таблицы содержат нужные данные. Различные типы данных разделяются на выделенные таблицы в каждой [рабочей области Log Analytics](../../azure-monitor/learn/quick-create-workspace.md).  Документация для каждого источника данных включает имя создаваемого типа данных и описание каждого из его свойств.  Многие запросы потребуют только данные из одной таблицы, но другие могут использовать множество параметров для включения данных из нескольких таблиц.

Хотя служба [Application Insights](../../application-insights/app-insights-overview.md) хранит данные приложения, например запросы, исключения, трассировки и данные использования в Log Analytics, эти данные хранятся в отдельном разделе, в отличие от других данных журнала. Используется тот же язык запросов для доступа к этим данным, но для этого должны быть [консоль Application Insights](../../application-insights/app-insights-analytics.md) или [REST API Application Insights](https://dev.applicationinsights.io/). Вы можете использовать [кросс-ресурсные запросы](../../azure-monitor/log-query/cross-workspace-query.md), чтобы объединить данные Application Insights с другими данными в Log Analytics.


![Таблицы](media/log-query-overview/queries-tables.png)







## <a name="next-steps"></a>Дополнительная информация

- Узнайте о [порталах, которые можно использовать для создания и изменения поисков по журналам](../../azure-monitor/log-query/portals.md).
- Изучите [руководство по написанию запросов](../../azure-monitor/log-query/get-started-queries.md) на новом языке.
