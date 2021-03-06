---
title: Ведение журнала для экземпляров контейнеров с помощью Журналов Azure Monitor
description: Узнайте, как отправить вывод из контейнера (STDOUT и STDERR) в Журналы Azure Monitor.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 07/17/2018
ms.author: danlep
ms.openlocfilehash: 13f1fa92365c284ed10bd7c0a1b2fdefef50b29e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2019
ms.locfileid: "56879719"
---
# <a name="container-instance-logging-with-azure-monitor-logs"></a>Ведение журнала для экземпляров контейнеров с помощью Журналов Azure Monitor

Рабочие области Log Analytics предоставляют централизованное расположение для хранения и данных журналов и обращения к ним не только для ресурсов Azure, но и для ресурсов в локальной среде и других облаках. Экземпляры контейнеров Azure поддерживают отправку данных в Журналы Azure Monitor по умолчанию.

Чтобы отправлять данные из экземпляра контейнера в Журналы Azure Monitor, вам нужно создать группу контейнеров с помощью Azure CLI (или Cloud Shell) и файла YAML. В следующих разделах описано, как создать группу контейнеров с поддержкой ведения журналов и обращения к ним.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Предварительные требования

Чтобы включить ведение журнала в экземпляре контейнера, необходимо следующее:

* [Рабочая область Log Analytics](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (или [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Получение учетных данных Log Analytics

Экземплярам контейнеров Azure нужны разрешения на отправку данных в рабочую область Log Analytics. Чтобы предоставить такие разрешения и включить ведение журнала, необходимо указать идентификатор рабочей области Log Analytics и один из ключей для нее (первичный или вторичный) при создании группы контейнеров.

Чтобы получить идентификатор и первичный ключ рабочей области Log Analytics, сделайте следующее:

1. На портале Azure перейдите к рабочей области Log Analytics.
1. В разделе **Параметры** выберите **Дополнительные параметры**.
1. Последовательно выберите **Подключенные источники** > **Серверы Windows** (или **Серверы Linux** — идентификатор и ключи для них одинаковые).
1. Запишите следующие значения:
   * **идентификатор рабочей области**;
   * **первичный ключ**.

## <a name="create-container-group"></a>Создание группы контейнеров

Теперь у вас есть идентификатор и первичный ключ рабочей области Log Analytics, а значит вы можете создать группу контейнеров с поддержкой ведения журнала.

В следующих примерах показаны два способа создания группы контейнеров с одним контейнером [fluentd][fluentd]: с помощью Azure CLI и Azure CLI с шаблоном YAML. Контейнер Fluentd в стандартной конфигурации создает несколько строк выходных данных. Так как эти выходные данные направляются в рабочую область Log Analytics, такой контейнер отлично подходит для демонстрации процессов просмотра журналов и обращения к ним.

### <a name="deploy-with-azure-cli"></a>Развертывание с помощью интерфейса командной строки Azure

Для развертывания с помощью Azure CLI следует указать параметры `--log-analytics-workspace` и `--log-analytics-workspace-key` при вызове команды [az container create][az-container-create]. Замените два значения рабочей области значениями, полученными на предыдущем шаге (и обновите имя группы ресурсов), прежде чем выполнять следующую команду.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainergroup001 \
    --image fluent/fluentd \
    --log-analytics-workspace <WORKSPACE_ID> \
    --log-analytics-workspace-key <WORKSPACE_KEY>
```

### <a name="deploy-with-yaml"></a>Развертывание с помощью YAML

Используйте этот метод, чтобы развернуть группы контейнеров с YAML. Этот пример YAML определяет группу контейнеров с одним контейнером. Скопируйте код YAML в новый файл, а затем замените `LOG_ANALYTICS_WORKSPACE_ID` и `LOG_ANALYTICS_WORKSPACE_KEY` значениями, полученными на предыдущем шаге. Сохраните файл как **deploy-aci.yaml**.

```yaml
apiVersion: 2018-06-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Затем выполните следующую команду, чтобы развернуть группу контейнеров. Замените в ней `myResourceGroup` именем группы ресурсов в вашей подписке (или создайте группу ресурсов с именем myResourceGroup перед выполнением команды).

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Вскоре после выполнения команды вы получите от Azure ответ с информацией о развертывании.

## <a name="view-logs-in-azure-monitor-logs"></a>Просмотр журналов в Azure Monitor

После развертывания группы контейнеров может потребоваться до 10 минут, чтобы на портале Azure появились первые записи журнала. Чтобы просмотреть журналы группы контейнеров, откройте рабочую область Log Analytics и сделайте следующее:

1. В обзоре **рабочей области OMS** выберите **Поиск по журналам**. Рабочие области OMS теперь называются рабочими областями Log Analytics.  
1. В разделе **Несколько дополнительных запросов** выберите ссылку **Все собранные данные**.

Вы увидите несколько результатов, отображаемых запросом `search *`. Если результаты не выводятся, подождите несколько минут и щелкните **Запустить**, чтобы выполнить запрос еще раз. По умолчанию записи журнала отображаются в представлении "Список". Щелкните **Таблица**, чтобы увидеть более сжатый формат отображения записей журнала. Вы также можете развернуть любую строку, чтобы просмотреть содержимое отдельной записи журнала.

![Параметры поиска по журналам на портале Azure][log-search-01]

## <a name="query-container-logs"></a>Запрос по журналам контейнера

Журналы Azure Monitor поддерживают расширенный [язык запросов][query_lang] для извлечения информации из выходных данных журналов, размер которых может достигать тысяч строк.

Агент ведения журнала в Экземплярах контейнеров Azure отправляет записи в таблицу `ContainerInstanceLog_CL` в рабочей области Log Analytics. Базовая структура запроса включает имя таблицы с исходными данными (`ContainerInstanceLog_CL`) и ряд операторов, разделенных символом вертикальной черты (`|`). Вы можете объединить несколько операторов в цепочку, чтобы получить более точные результаты или применить более сложные функции.

Чтобы просмотреть пример результатов запроса, вставьте следующий текст в поле запроса (в разделе "Показать устаревший языковой преобразователь") и щелкните **Запустить**, чтобы выполнить этот запрос. Этот запрос отображает все записи журнала, у которых поле Message (Сообщение) которого содержит слово warn (Предупреждение):

```query
ContainerInstanceLog_CL
| where Message contains("warn")
```

Также поддерживаются более сложные запросы. Например, такой запрос отображает записи из журнала группы контейнеров mycontainergroup001, которые были созданы за последний час:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Дополнительная информация

### <a name="azure-monitor-logs"></a>Журналы Azure Monitor

Дополнительные сведения о запросах по журналам и настройке предупреждений в журналах Azure Monitor см. в следующих статьях:

* [Анализ данных журнала в Azure Monitor](../log-analytics/log-analytics-log-search.md)
* [Новые возможности оповещений в Azure Monitor](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>Мониторинг использования ЦП и памяти

Дополнительные сведения о мониторинге ресурсов ЦП и памяти для экземпляра контейнера см. в следующей статье:

* [Мониторинг ресурсов контейнеров в службе "Экземпляры контейнеров Azure"](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://aka.ms/LogAnalyticsLanguage

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create