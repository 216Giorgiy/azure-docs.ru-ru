---
title: Обзор журналов диагностики Azure
description: Узнайте, что такое журналы диагностики Azure и как их использовать для просмотра событий, происходящих в ресурсе Azure.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: 890f2224a4053ec8cad65b44b85eab0e31be3b64
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519397"
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Сбор и использование данных журнала из ресурсов Azure

## <a name="what-are-azure-monitor-diagnostic-logs"></a>Что такое журналы диагностики Azure Monitor

**Журналы диагностики Azure Monitor** генерируются службой Azure. Они содержат подробные и своевременные данные о работе этой службы. Azure Monitor предоставляет два типа диагностических журналов:
* **Журналы клиентов** — эти журналы поступают из служб уровня клиента, которые существуют за пределами подписки Azure, такие как журналы Azure Active Directory.
* **Журналы ресурсов** — эти журналы поступают из служб Azure, которые развертывают ресурсы в подписке Azure, такие как группы безопасности сети или учетные записи хранения.

    ![Журналы диагностики ресурсов и другие типы журналов](./media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

Содержимое этих журналов зависит от типа ресурса и службы Azure. Например, счетчики правила группы безопасности сети и аудит Key Vault представляют два типа журналов диагностики.

Журналы диагностики отличаются от [журналов действий](activity-logs-overview.md). Журнал действий содержит информацию об операциях, которые были выполнены с ресурсами в вашей подписке с помощью Resource Manager, например о создании виртуальной машины или удалении приложения логики. Журнал действий — это журнал уровня подписки. Журналы диагностики уровня ресурса дают представление об операциях, которые были выполнены в рамках этого ресурса, например о получении секрета из Key Vault.

Эти журналы также отличаются от журналов диагностики уровня гостевой ОС. Журналы диагностики гостевой ОС собирает агент, работающий на виртуальной машине или поддерживаемом ресурсе другого типа. Для сбора журналов диагностики уровня ресурса не требуется агент. В них записываются данные ресурсов из платформы Azure, тогда как в журналы диагностики уровня гостевой ОС записываются данные из операционной системы и приложений, выполняющихся на виртуальной машине.

Не все ресурсы поддерживают описанные здесь типы журналов диагностики. [Эта статья содержит раздел со списком служб, поддерживающих журналы диагностики](./../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>Что можно делать с журналами диагностики
Ниже описано несколько доступных операций с журналами диагностики.

![Логическое расположение журналов диагностики](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* Сохранение журналов в [**учетную запись хранения**](../../azure-monitor/platform/archive-diagnostic-logs.md) для аудита или проверки вручную. В **параметрах диагностики ресурсов** можно задать время хранения (в днях).
* [Потоковая передача журналов в **Центры событий**](diagnostic-logs-stream-event-hubs.md) для обработки в сторонней службе или пользовательском аналитическом решении, например в PowerBI.
* Анализируйте журналы с помощью [Azure Monitor](../../azure-monitor/platform/collect-azure-metrics-logs.md), так как данные сразу записываются в Azure Monitor, поэтому их не нужно сначала записывать в хранилище.  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Вы можете использовать учетную запись хранения или пространство имен Центров событий, не входящее в подписку, в которой создаются журналы. Пользователю, который настраивает этот параметр, должен быть предоставлен соответствующий уровень доступа RBAC к обеим подпискам.

> [!NOTE]
>  Сейчас невозможно архивировать журналы потоков в учетную запись хранения, которая находится в защищенной виртуальной сети.

## <a name="diagnostic-settings"></a>Параметры диагностики

Журналы диагностики для всех ресурсов настраиваются с помощью параметров диагностики ресурсов. Журналы диагностики клиентов настраиваются с помощью параметров диагностики клиента. **Параметры диагностики** для службы определяют следующие настройки.

* Куда отправляются журналы диагностики и метрики (учетная запись хранения, Центры событий и (или) Azure Monitor).
* Какие категории журнала отправляются и следует ли отправлять данные метрики.
* Как долго должны храниться журналы каждой категории в учетной записи хранения.
    - Срок хранения 0 дней означает, что журналы хранятся неограниченно долго. В противном случае — значение может быть любое число от 1 до 365 дней.
    - Если политики хранения заданы, но хранение журналов в учетной записи хранения отключено (например, выбраны только Центры событий или Log Analytics), политики хранения не будут применены.
    - Политики хранения применяются по дням, поэтому в конце дня (по времени в формате UTC) журналы, срок которых теперь превышает период хранения, будут удалены. Например, если настроена политика хранения в течение одного дня, то в начале текущего дня журналы за вчерашний день будет удалены. Удаление начинается в полночь по времени UTC. Обратите внимание, что удаление журналов из учетной записи хранения может занять до 24 часов.

Эти параметры легко настраиваются в параметрах диагностики на портале, а также с помощью Azure PowerShell, команд интерфейса командной строки или [REST API Azure Monitor](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Отправка многомерных метрик с помощью параметров диагностики сейчас не поддерживается. Метрики с измерениями экспортируются как преобразованные в плоскую структуру одномерные метрики, агрегированные по значениям измерений.
>
> *Пример*. Метрику "Входящие сообщения" в концентраторе событий можно изучить и вывести в виде диаграммы на уровне очереди. Но при экспорте с помощью параметров диагностики метрика будет представлена в виде всех входящих сообщений для всех очередей концентратора событий.
>
>

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Как включить сбор журналов диагностики

Сбор журналов диагностики можно включить [во время создания ресурса с помощью шаблона Resource Manager](./../../azure-monitor/platform/diagnostic-logs-stream-template.md) или после его создания на странице ресурса на портале. Сбор этих журналов можно также включить в любой момент с помощью Azure PowerShell, команд интерфейса командной строки или REST API Azure Monitor.

> [!TIP]
> При работе с некоторыми ресурсами эти инструкции могут требовать корректировки. Изучите связи на схеме, представленной в конце этой страницы, чтобы разобраться в дополнительных действиях, которые могут потребоваться при использовании некоторых типов ресурсов.

### <a name="enable-collection-of-diagnostic-logs-in-the-portal"></a>Включение сбора журналов диагностики на портале

Сбор журналов диагностики ресурсов на портале Azure после создания ресурса можно включить, перейдя к конкретному ресурсу или к Azure Monitor. Чтобы выполнить это с помощью Azure Monitor, сделайте следующее:

1. На [портале Azure](https://portal.azure.com) перейдите к Azure Monitor и щелкните **Параметры диагностики**

    ![Раздел мониторинга Azure Monitor](media/diagnostic-logs-overview/diagnostic-settings-blade.png)

2. При необходимости отфильтруйте список по группе или типу ресурса, а затем щелкните ресурс, для которого необходимо задать параметр диагностики.

3. Если параметров для выбранного ресурса не существует, вам будет предложено создать параметр. Щелкните Turn on diagnostics (Включить диагностику).

   ![Добавление параметра диагностики — имеющиеся параметры отсутствуют](media/diagnostic-logs-overview/diagnostic-settings-none.png)

   Если в ресурсе имеются параметры, для него отобразится список настроенных параметров. Нажмите Add diagnostic setting (Добавить параметр диагностики).

   ![Добавление параметра диагностики — имеющиеся параметры](media/diagnostic-logs-overview/diagnostic-settings-multiple.png)

3. Задайте имя параметра, установите флажки для каждого целевого расположения, в которое необходимо отправить данные, и укажите ресурс, используемый для каждого назначения. При необходимости с помощью ползунков **Хранение (дни)** укажите продолжительность хранения этих журналов в днях (применимо только к целевому расположению учетной записи хранения). Нулевое значение означает, что журналы будут храниться неограниченно долго.

   ![Добавление параметра диагностики — имеющиеся параметры](media/diagnostic-logs-overview/diagnostic-settings-configure.png)

4. Выберите команду **Сохранить**.

Через несколько секунд появится новый параметр в списке параметров для данного ресурса, и сразу же после создания данных о событии журналы диагностики отправятся по указанным назначениям.

Параметры диагностики клиента могут быть настроены только в колонке службы клиента на портале — эти параметры не отображаются в колонке параметров диагностики Azure Monitor. Например, чтобы настроить журналы аудита Azure Active Directory, щелкните **Экспортировать параметры данных** в колонке "Журналы аудита".

![Параметры диагностики AAD](./media/diagnostic-logs-overview/diagnostic-settings-aad.png)

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Включение сбора журналов диагностики ресурсов с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Чтобы включить сбор журналов диагностики ресурсов с помощью Azure PowerShell, используйте следующие команды.

Выполните приведенную ниже команду, чтобы включить отправку журналов диагностики в учетную запись хранения.

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

StorageAccountId — это идентификатор ресурса учетной записи хранения, в которую будут отправляться журналы.

Чтобы включить потоковую передачу журналов диагностики в концентратор событий, используйте следующую команду.

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

ServiceBusRuleID — это строка в формате `{Service Bus resource ID}/authorizationrules/{key name}`.

Чтобы включить отправку журналов диагностики в рабочую область Log Analytics, используйте следующую команду.

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Идентификатор ресурса рабочей области Log Analytics можно получить с помощью следующей команды.

```powershell
(Get-AzOperationalInsightsWorkspace).ResourceId
```

Можно объединять эти параметры, чтобы получить несколько вариантов вывода.

В настоящее время вы не можете настроить параметры диагностики клиента с помощью Azure PowerShell.

### <a name="enable-collection-of-resource-diagnostic-logs-via-the-azure-cli"></a>Включение сбора журналов диагностики ресурсов с помощью Azure CLI

Чтобы включить сбор журналов диагностики ресурсов с помощью Azure CLI, используйте команду [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

Чтобы включить журналы диагностики в учетную запись хранения, сделайте следующее:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Аргумент `--resource-group` является обязательным, только если `--storage-account` — не идентификатор объекта.

Чтобы включить потоковую передачу журналов диагностики в концентратор событий, сделайте следующее:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Идентификатор правила — это строка в следующем формате: `{Service Bus resource ID}/authorizationrules/{key name}`.

Чтобы включить отправку журналов диагностики в рабочую область Log Analytics, сделайте следующее:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Аргумент `--resource-group` является обязательным, только если `--workspace` — не идентификатор объекта.

Используя любую команду, вы можете расширить набор категорий в журнале диагностики, добавив словари в массив JSON, переданный в качестве параметра `--logs`. Можно объединять параметры `--storage-account`, `--event-hub` и `--workspace`, чтобы получить несколько вариантов вывода.

В настоящее время вы не можете настроить параметры диагностики клиента с помощью Azure CLI.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Включение сбора журналов диагностики ресурсов с помощью REST API

Изменение параметров диагностики с помощью REST API Azure Monitor описано в [этом документе](https://docs.microsoft.com/rest/api/monitor/).

В настоящее время вы не можете настроить параметры диагностики клиента с помощью REST API Azure Monitor.

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Управление параметрами диагностики ресурсов на портале

Убедитесь, что все ресурсы настроены с помощью параметров диагностики. Перейдите к разделу **Монитор** на портале и откройте **Параметры диагностики**.

![Колонка "Журналы диагностики" на портале](./media/diagnostic-logs-overview/diagnostic-settings-nav.png)

Раздел "Монитор" можно найти, щелкнув меню "Все службы".

Здесь можно просматривать и фильтровать все ресурсы, поддерживающие журналы диагностики. Таким образом можно проверить, включена ли для них диагностика. Вы также можете детально просмотреть сведения, чтобы проверить, задано ли для ресурса несколько параметров, в какую учетную запись хранения, пространство имен Центров событий и (или) рабочую область Log Analytics передаются данные.

![Журналы диагностики на портале](./media/diagnostic-logs-overview/diagnostic-settings-blade.png)

При добавлении параметра диагностики открывается колонка "Параметры диагностики", в которой можно включить, отключить или изменить параметры диагностики для выбранного ресурса.

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Поддерживаемые службы, категории и схемы для журналов диагностики

С полным списком поддерживаемых служб, категорий и схем журнала, используемых этими службами, ознакомьтесь в [этой статье](../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Потоковая передача журналов диагностики Azure в **Центры событий**](diagnostic-logs-stream-event-hubs.md)
* [Создание или обновление диагностического параметра](https://docs.microsoft.com/rest/api/monitor/)
* [Анализ журналов из службы хранилища Azure с помощью Azure Monitor](collect-azure-metrics-logs.md)
