---
title: Общие сведения о расширении (копировании) оповещений Log Analytics в оповещения Azure.
description: Общие сведения о процессе копирования оповещений из Log Analytics на портале OMS в оповещения Azure, а также дополнительные сведения о распространенных проблемах клиентов.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6484142eafa8388117c1e96ab31eefeab188e488
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750278"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Расширение оповещений Log Analytics в оповещения Azure
До недавнего времени в Azure Log Analytics были функции, которые позволяли заблаговременно уведомлять о некоторых условиях на основе данных Log Analytics. Ранее правила генерации оповещений задавались на портале Microsoft Operations Management Suite. Новые возможности оповещений теперь интегрированы в различные службы Microsoft Azure. Они доступны в разделе **Оповещения** в Azure Monitor на портале Azure и позволяют реализовать оповещения на основе журнала действий, метрик и журналов Log Analytics и Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Преимущества расширения оповещений
Есть несколько преимуществ создания оповещений и управления ими на портале Azure, например:

- В отличие от портала Operations Management Suite, где можно было создавать и просматривать только 250 оповещений, в оповещениях Azure нет такого ограничения.
- В оповещениях Azure можно контролировать, перечислять и просматривать все их типы. Раньше это можно было сделать только для оповещений Log Analytics.
- Можно ограничить доступ пользователей только к мониторингу и оповещению, используя [роль Azure Monitor](monitoring-roles-permissions-security.md).
- В оповещениях Azure можно использовать [группы действий](monitoring-action-groups.md). Эта функция позволяет поддерживать несколько действий для каждого оповещения. Вы можете отправить текстовое сообщение, совершить голосовой вызов, запустить runbook службы автоматизации Azure, вызывать веб-перехватчик и настроить соединитель управления ИТ-услугами. 

## <a name="process-of-extending-your-alerts"></a>Процесс расширения оповещений
Процесс перемещения оповещений из Log Analytics в оповещения Azure не затрагивает определение, запрос или конфигурацию оповещений каким-либо образом. В Azure требуется только одно изменение — выполнять все действия с помощью группы действий. Если группы действий связываются с оповещением, они включаются при расширении в Azure.

> [!NOTE]
> Microsoft автоматически расширит предупреждения, созданные в Log Analytics для оповещений Azure начиная с 14 мая 2018, в повторяющейся серии до завершения. Если у вас возникли проблемы с созданием [групп действий](monitoring-action-groups.md), используйте [эти шаги по устранению неполадок](monitoring-alerts-extend-tool.md#troubleshooting), чтобы автоматически создать группы действий. Эти шаги можно предпринять до 5 июля 2018 г. 
> 

После расширения оповещений из рабочей области Log Analytics в Azure они продолжат работу и никак не нарушат вашу конфигурацию. Во время планирования оповещения могут быть временно недоступны для изменения, однако в это время можно создать новые оповещения Azure. При создании или редактировании оповещений на портале Operations Management Suite у вас есть возможность продолжить это действие в рабочем пространстве Log Analytics. Можно также создать их в оповещениях Azure на том же портале.

 ![Снимок экрана с возможностью создания оповещений в Log Analytics или в разделе "Оповещения Azure".](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Плата за расширение оповещений из Log Analytics в Azure в учетной записи не взимается. Плата за использование оповещений Azure для запросов на основе журнала Log Analytics не взимается, если это осуществляется в рамках и по условиям, указанным в статье [Цены на Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Произвольное расширение оповещений
Чтобы расширить оповещения в оповещения Azure, можно использовать мастер, доступный на портале Operations Management Suite, или сделать это программно с помощью API. Дополнительные сведения см. в статье [Расширение (копирование) оповещений из портала Operations Management Suite и API в Azure](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Возможности, доступные после расширения оповещений
После расширения оповещений в оповещения Azure они остаются доступными на портале Operations Management Suite без изменений в управлении.

![Снимок экрана Operations Management Suite со списком оповещений](./media/monitor-alerts-extend/PostExtendList.png)

При попытке создать новое оповещение или изменить существующее на портале Operations Management Suite вы будете автоматически перенаправлены в раздел "Оповещения Azure".  

> [!NOTE]
> Убедитесь, что индивидуальным пользователям, которым необходимо добавлять или редактировать оповещения, должным образом присвоены разрешения в Azure. Чтобы понять, какие разрешения необходимо предоставить, см. раздел [Разрешения для использования оповещений и Azure Monitor](monitoring-roles-permissions-security.md).  
> 

Можно продолжить создание оповещений в [Log Analytics API](../log-analytics/log-analytics-api-alerts.md) и в [Шаблоне ресурсов Log Analytics](../monitoring/monitoring-solutions-resources-searches-alerts.md). При этом необходимо включить группы действий.

## <a name="next-steps"></a>Дополнительная информация

* Дополнительные сведения о средствах [инициирования расширения оповещений из Log Analytics в Azure](monitoring-alerts-extend-tool.md).
* Дополнительные сведения об [оповещениях Azure](monitoring-overview-unified-alerts.md).
* Узнайте о том, как создать [журнал оповещений в оповещениях Azure](monitor-alerts-unified-log.md).
