---
title: Управление оповещениями из других служб мониторинга в Azure Monitor
description: Управление оповещениями Nagios, Zabbix и SCOM в Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: bc597d42fe89c0e03c4af1db3a935031b9043a98
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345481"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Управление оповещениями из других служб мониторинга

Теперь вы можете просматривать оповещения от Nagios, Zabbix и System Center Operations Manager в [едином интерфейсе оповещений](https://aka.ms/azure-alerts-overview). Эти оповещения поступают в Log Analytics из интегрированных серверов Nagios и Zabbix либо System Center Operations Manager. 

## <a name="prerequisites"></a>Предварительные требования
Любые записи в репозитории Log Analytics с типом Alert (Оповещение), будут импортироваться в единый интерфейс оповещений, поэтому вам нужно выполнить настройку, которая требуется для сбора этих записей.
1. Для оповещений **Nagios** и **Zabbix** настройте [соответствующие серверы](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) на отправку оповещений в Log Analytics.
1. Для оповещений **System Center Operations Manager** [подключите группу управления Operations Manager к рабочей области Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Все оповещения, созданные в System Center Operations Manager, импортируются в Log Analytics.

## <a name="view-your-alert-instances"></a>Просмотр экземпляров оповещений
Настроив импорт в Log Analytics, можно просматривать экземпляры оповещений от этих служб мониторинга в [едином интерфейсе оповещений](https://aka.ms/azure-alerts-overview). Когда эти оповещения появятся в едином интерфейсе, вы сможете [управлять их экземплярами](https://aka.ms/managing-alert-instances), [управлять смарт-группами, созданными на основе этих оповещений,](https://aka.ms/managing-smart-groups) и [изменять состояние оповещений и смарт-групп](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Оповещения Nagios в едином интерфейсе не поддерживают отслеживание состояния. Например, [состояние монитора](https://aka.ms/azure-alerts-overview) оповещения не изменится с "Сработало" на "Разрешено". Вместо этого оповещения с такими состояниями отображаются как отдельные экземпляры оповещений. 
