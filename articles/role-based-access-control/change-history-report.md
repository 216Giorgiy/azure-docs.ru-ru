---
title: Просмотр журналов действий на наличие изменений в управлении доступом на основе ролей в Azure | Документы Майкрософт
description: Вы можете просматривать журналы действий на наличие изменений в управлении доступом на основе ролей (RBAC) за последние 90 дней.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f8c3c770cb7e30bda16b4857d5b337923d2417d2
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2018
ms.locfileid: "53541563"
---
# <a name="view-activity-logs-for-rbac-changes"></a>Просмотр журналов действий на предмет изменений в RBAC

Иногда могут потребоваться сведения об изменении доступа на основе ролей (RBAC), например для аудита и устранения неполадок. Каждый раз, когда кто-либо вносит изменения в определения ролей или назначения ролей в ваших подписках, изменения регистрируются в [журнале действий Azure](../azure-monitor/platform/activity-logs-overview.md). Вы можете просматривать журналы действий, чтобы увидеть все изменения в управлении доступом на основе ролей за последние 90 дней.

## <a name="operations-that-are-logged"></a>Операции, которые регистрируются в журнале

Ниже приведены операции, связанные с RBAC, которые регистрируются в журнале действий.

- Создание назначения роли
- Удаление назначения роли
- Создание или изменение определения пользовательской роли
- Удаление определения пользовательской роли

## <a name="azure-portal"></a>Портал Azure

Самый простой способ заключается в просмотре журналов действий на портале Azure. На следующем снимке экрана показан пример журнала действий, отфильтрованный для отображения операций определения и назначений ролей. Он также содержит ссылку для скачивания журналов в виде CSV-файла.

![Снимок экрана: просмотр журналов действий на портале](./media/change-history-report/activity-log-portal.png)

Журнал действий на портале имеет несколько фильтров. Ниже приведены фильтры, связанные с RBAC.

|Фильтр  |Значение  |
|---------|---------|
|Категория событий     | <ul><li>Administrative</li></ul>         |
|Операция     | <ul><li>Создание назначения роли</li> <li>Удаление назначения роли</li> <li>Создание или изменение определения пользовательской роли</li> <li>Удаление определения пользовательской роли</li></ul>      |


Дополнительные сведения о журналах действий см. в статье [Просмотр журналов действий для аудита действий с ресурсами](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

Чтобы просмотреть журналы действий с помощью Azure PowerShell, используйте команду [Get-AzureRmLog](/powershell/module/azurerm.insights/get-azurermlog).

Эта команда выводит список всех изменений назначений ролей в подписке за последние семь дней.

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Эта команда выводит список всех изменений определений ролей в подписке за последние семь дней.

```azurepowershell
Get-AzureRmLog -ResourceGroupName pharma-sales-projectforecast -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Эта команда выводит список всех изменений назначений ролей и определений ролей в подписке за последние семь дней.

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast"}}

```

## <a name="azure-cli"></a>Инфраструктура CLI Azure

Чтобы просмотреть журналы действий с помощью Azure CLI, используйте команду [az monitor activity-log list](/cli/azure/monitor/activity-log#az-monitor-activity-log-list).

Эта команда выводит список журналов действий в группе ресурсов со времени начала.

```azurecli
az monitor activity-log list --resource-group pharma-sales-projectforecast --start-time 2018-04-20T00:00:00Z
```

Эта команда выводит список журналов действий для поставщика ресурсов авторизации со времени начала.

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-log-analytics"></a>Azure Log Analytics

Средство [Azure Log Analytics](../log-analytics/log-analytics-overview.md) можно использовать для сбора и анализа изменений управления доступом на основе ролей для всех ресурсов Azure. Log Analytics имеет следующие преимущества:

- создание сложных запросов и логики;
- интеграция с оповещениями, Power BI и другими средствами;
- хранение данных в течение более длительного срока;
- поддержка перекрестных ссылок с другими журналами, такими как журналы безопасности, журналы виртуальных машин и настраиваемые журналы.

Ниже приведены основные шаги, позволяющие приступить к работе.

1. [Создание рабочей области Log Analytics](../azure-monitor/learn/quick-create-workspace.md).

1. [Настройка решения аналитики журнала действий](../azure-monitor/platform/collect-activity-logs.md#configuration) для рабочей области.

1. [Просмотр журналов действий](../azure-monitor/platform/collect-activity-logs.md#using-the-solution). Чтобы быстро перейти к странице "Обзор Activity Log Analytics", нажмите кнопку **Log Analytics**.

   ![Кнопка "Log Analytics" на портале](./media/change-history-report/azure-log-analytics-option.png)

1. При необходимости используйте страницу [Поиска по журналам](../log-analytics/log-analytics-log-search.md) или [портал расширенной аналитики](../azure-monitor/log-query/get-started-portal.md) для формирования запросов и просмотра журналов. Дополнительные сведения об этих двух вариантах см. в разделах о [странице "Поиск по журналам" или портале расширенной аналитики](../azure-monitor/log-query/portals.md).

Ниже приведен запрос, возвращающий новые назначения ролей, упорядоченные по целевому поставщику ресурсов.

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Ниже приведен запрос, возвращающий изменения назначения ролей, отображаемые на схеме.

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Снимок экрана: журналы действий на портале расширенной аналитики](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Дополнительная информация
* [Просмотр событий в журнале действий](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Monitor Subscription Activity with the Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (Мониторинг действий подписки с помощью журнала действий Azure)
