---
title: Потоковая передача журнала действий Azure в Центры событий
description: Узнайте, как настроить потоковую передачу журнала действий Azure в Центры событий.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: b58b7c7ebc3547153f805f762c4caf3511a5a709
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2018
ms.locfileid: "53717507"
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Потоковая передача журнала действий Azure в Центры событий
Можно осуществлять потоковую передачу [журнала действий Azure](../../azure-monitor/platform/activity-logs-overview.md) почти в реальном времени в любое приложение:

* с помощью встроенной возможности **Экспорт** на портале;
* включив идентификатор правила служебной шины Azure в профиле журнала с помощью командлетов Azure PowerShell или Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Что можно делать с журналом действий и Центрами событий
Ниже приведены два способа использования потоковой передачи журнала действий.

* **Потоковая передача в сторонние системы ведения журналов и сбора телеметрии.** Со временем Центры событий Azure будут использоваться для передачи журнала действий в сторонние решения SIEM и Log Analytics.
* **Создание пользовательской платформы для телеметрии и ведения журнала.** Если у вас уже есть пользовательская платформа для телеметрии и ведения журнала или вы только планируете ее создать, высокая масштабируемость публикации и подписки Центров событий позволит вам гибко настраивать прием журнала действий. Чтобы узнать больше, ознакомьтесь с [видео Дэна Росановы (Dan Rosanova) об использовании Центров событий в глобальной платформе телеметрии](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Включение потоковой передачи журнала действий
Потоковую передачу журнала действий можно включить программно или с помощью портала. Можно также выбрать пространство имен Центров событий и политику общего доступа для этого пространства имен. В этом пространстве имен создается концентратор событий insights-logs-operationallogs, когда происходит первое событие журнала действий. 

Если у вас нет пространства имен Центров событий, необходимо сначала создать его. Если потоковая передача событий журнала действий уже выполнялась в это пространство имен Центров событий, то созданный ранее концентратор событий будет использован повторно. 

Политика общего доступа определяет разрешения, предоставленные для механизма потоковой передачи. В настоящее время для потоковой передачи журналов в Центры событий нужны разрешения **Управление**, **Отправка** и **Прослушивание**. Политики общего доступа для пространства имен Центров событий можно создать или изменить на портале Azure, на вкладке **Настройка** соответствующего пространства имен. 

Чтобы изменить профиль журнала действий и разрешить потоковую передачу, пользователь, который вносит изменение, должен иметь разрешение ListKey для соответствующего правила авторизации Центров событий. Пространство имен службы "Центры событий Azure" не должно находиться в той же подписке, что и подписка, генерирующая журналы, если пользователь, который настраивает этот параметр, имеет соответствующий доступ RBAC к обеим подпискам, и обе подписки находятся в том же клиенте AAD.

### <a name="via-the-azure-portal"></a>Использование портала Azure
1. Перейдите в раздел **Журнал действий**, используя раздел поиска **Все службы** в левой части портала.
   
   ![Выбор пункта "Журнал действий" из списка служб на портале](./media/activity-logs-stream-event-hubs/activity-logs-portal-navigate-v2.png)
2. Нажмите кнопку **Экспорт в концентратор событий** в верхней части журнала.
   
   ![Кнопка "Экспорт" на портале](./media/activity-logs-stream-event-hubs/activity-logs-portal-export-v2.png)

   Обратите внимание на то, что параметры фильтра, которые были применены при просмотре журнала действий в предыдущем представлении, не оказывают влияния на параметры экспорта. Они предназначены исключительно для фильтрации данных, отображаемых при просмотре журналов действий на портале.
3. В открывшемся разделе выберите **Все регионы**. Не выбирайте какие-либо определенные регионы.
   
   ![Раздел "Экспорт"](./media/activity-logs-stream-event-hubs/export-audit.png)

   > [!WARNING]  
   > Если выбрать не **Все регионы**, а другие параметры, то вы пропустите ключевые события, которые ожидаете получить. Журнал действий является глобальным (а не региональным), поэтому с большей частью событий не связан какой-либо регион. 
   >

4. Щелкните параметр **Центры событий Azure**, выберите пространство имен Центров событий, в которые должны отправляться журналы, а затем нажмите кнопку **ОК**.
5. Нажмите кнопку **Сохранить**, чтобы сохранить эти параметры. Параметры будут немедленно применены к подписке
6. Если у вас несколько подписок, повторите это действие и отправьте все данные в тот же концентратор событий.

### <a name="via-powershell-cmdlets"></a>Использование командлетов PowerShell
Если профиль журнала уже существует, сначала удалите его, а затем создайте новый.

1. Используйте `Get-AzureRmLogProfile`, чтобы проверить, существует ли профиль журнала.  Если профиль журнала существует, найдите свойство *name*.
2. Удалите журнал профиля с помощью `Remove-AzureRmLogProfile`, используя значение свойства *name*.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzureRmLogProfile -Name "default"
    ```
3. Создайте профиль журнала с помощью `Add-AzureRmLogProfile`:

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```

### <a name="via-azure-cli"></a>С помощью интерфейса командной строки Azure
Если профиль журнала уже существует, сначала удалите его, а затем создайте новый.

1. Используйте `az monitor log-profiles list`, чтобы проверить, существует ли профиль журнала.
2. Удалите журнал профиля с помощью `az monitor log-profiles delete --name "<log profile name>`, используя значение свойства *name*.
3. Создайте профиль журнала с помощью `az monitor log-profiles create`:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

## <a name="consume-the-log-data-from-event-hubs"></a>Использование данных журнала из Центров событий
Схема для журнала действий представлена в разделе [Мониторинг действий подписки с помощью журнала действий Azure](../../azure-monitor/platform/activity-logs-overview.md). Каждое событие сохраняется в массиве больших двоичных объектов JSON, которые называются *записями*.

## <a name="next-steps"></a>Дополнительная информация
* [Архивация журнала действий Azure](../../azure-monitor/platform/archive-activity-log.md)
* [Общие сведения о журнале действий Azure](../../azure-monitor/platform/activity-logs-overview.md)
* [Настройка объекта webhook для оповещений журнала действий Azure](../../azure-monitor/platform/alerts-log-webhook.md)

