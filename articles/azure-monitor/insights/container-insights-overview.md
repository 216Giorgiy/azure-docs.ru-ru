---
title: Общие сведения об Azure Monitor для контейнеров | Документация Майкрософт
description: В этой статье описывается служба "Azure Monitor для контейнеров", которая контролирует решение аналитики для контейнеров AKS и значение, которое она поставляет, путем мониторинга работоспособности ваших кластеров и экземпляров контейнеров AKS в Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2019
ms.author: magoedte
ms.openlocfilehash: a31380c8581503a340c55c374afc02c6e1fa290b
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577175"
---
# <a name="azure-monitor-for-containers-overview"></a>Обзор службы "Azure Monitor для контейнеров"

Azure Monitor для контейнеров — это компонент для контроля производительности рабочих нагрузок контейнеров, развертываемых в службе "Экземпляры контейнеров Azure" или на управляемых кластерах Kubernetes в Службе Azure Kubernetes (AKS). Мониторинг контейнеров крайне важен, особенно если вы управляете рабочим кластером в нужном масштабе с несколькими приложениями.

С помощью Azure Monitor для контейнеров можно отслеживать производительность, собирая данные метрик памяти и процессора из контроллеров, узлов и контейнеров, доступных в Kubernetes, используя API метрик. Также собираются журналы контейнеров.  После включения мониторинга из кластеров Kubernetes эти метрики и журналы автоматически собираются через контейнерную версию агента Log Analytics для Linux и хранятся в вашем рабочем пространстве [Log Analytics](../log-query/log-query-overview.md). 
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>В чем заключается польза Azure Monitor для контейнеров?

Azure Monitor для контейнеров включает в себя несколько предопределенных представлений, которые показывают размещенные в контейнерах рабочие нагрузки, влияющие на производительность контролируемого кластера Kubernetes. Благодаря этому вы можете сделать перечисленное ниже.  

* Определить, какие контейнеры AKS запущены на узле, а также данные о среднем использовании их ресурсов процессора и памяти. Эта информация может помочь выявить узкие места ресурсов.
* Определите использование ресурсов процессора и памяти группами и их контейнерами, размещенными в службе "Экземпляры контейнеров Azure".  
* Определить, где находится контейнер в контроллере или модуле pod. Эта информация может помочь просмотреть общую производительность контроллера или pod.
* Просмотреть данные об использовании ресурсов рабочих нагрузок на узле, не относящихся к стандартным процессам, поддерживающим pod.
* Понять поведение кластера при средних и высоких нагрузках. Эта информация может помочь определить потребности в емкости, а также максимальную нагрузку, которую может выдержать кластер. 

Можно также настроить оповещения, чтобы заблаговременно уведомить вас либо запишите его, если уровень загрузки ЦП и памяти на узлах или контейнеры превышают пороговым значениям.  

## <a name="how-do-i-access-this-feature"></a>Как получить доступ к этому компоненту?
Вы можете получить доступ к Azure Monitor для контейнеров двумя способами: из Azure Monitor или непосредственно из выбранного кластера AKS. Через Azure Monitor у вас есть глобальная видимость всех развернутых контейнеров, отслеживаемых и не отслеживаемых, что позволяет их искать и фильтровать через подписки и группы ресурсов, а затем детализировать данные в Azure Monitor для контейнеров из выбранного контейнера.  В противном случае можно просто получить доступ к этому компоненту непосредственно из выбранного контейнера AKS со страницы AKS.  

![Общие сведения о методах доступа к Azure Monitor для контейнеров](./media/container-insights-overview/azmon-containers-views-1812.png)

Если вы заинтересованы мониторинге узлов контейнеров Docker и Windows и управлении ими для просмотра конфигурации, аудита и изучения данных об использовании ресурсов, ознакомьтесь с разделом [Решение для мониторинга контейнеров в Log Analytics](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Дальнейшие действия
Чтобы получить представление о требованиях и доступных методах для мониторинга и начать мониторинг кластера AKS, просмотрите [How to onboard the Azure Monitor for containers](container-insights-onboard.md) (Как подключить Azure Monitor для контейнеров).  
