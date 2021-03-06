---
title: Приложение для управления исправлениями Azure Service Fabric для Linux | Документация Майкрософт
description: Приложение для автоматизации установки исправлений операционной системы в кластере Service Fabric под управлением Linux.
services: service-fabric
documentationcenter: .net
author: novino
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/22/2018
ms.author: nachandr
ms.openlocfilehash: 537450dbc386a94fa5c2e0d9334435dce041a32f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59266143"
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Установка исправлений операционной системы Linux в кластере Service Fabric

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

Приложение для управления исправлениями — это приложение Azure Service Fabric, которое позволяет автоматизировать установку исправлений операционной системы в кластере Service Fabric и избежать простоев.

Приложение для оркестрации исправлений предоставляет следующие возможности:

- **Автоматическая установка обновлений операционной системы**. Обновления операционной системы автоматически загружаются и устанавливаются. При необходимости узлы кластера перезагружаются без остановки его работы.

- **Исправление с учетом состояния кластера и интеграция функций работоспособности**. При применении обновлений приложение для управления исправлениями отслеживает работоспособность узлов кластера. Узлы кластера обновляются поочередно или по одному домену обновления за раз. Если из-за установки исправлений работоспособность кластера понижается, процесс останавливается, чтобы проблема не усугублялась.

## <a name="internal-details-of-the-app"></a>Внутренние сведения приложения

Приложение для управления исправлениями состоит из описанных ниже компонентов.

- **Служба координатора** — это служба с отслеживанием состояния, отвечающая за следующие задачи:
    - координация задания обновления ОС во всем кластере;
    - хранение результатов завершенных операций обновления ОС.
- **Служба агента узла** — это служба без отслеживания состояния, которая выполняется на всех узлах кластера Service Fabric. Она отвечает за следующие задачи:
    - начальная загрузка управляющей программы NTService в Linux;
    - мониторинг службы управляющей программы.
- **Управляющая программа агента узла**: Эта служба управляющей программы Linux выполняется в привилегий (корень). Для сравнения служба агента узла и служба координатора выполняются с разрешением более низкого уровня. Служба отвечает за выполнение следующих заданий обновления на всех узлах кластера:
    - выключение автоматического обновления ОС на узле;
    - загрузка и установка обновления ОС в соответствии с политикой, назначенной пользователем;
    - перезапуск машины после установки обновлений ОС, если это необходимо;
    - передача результатов обновления ОС в службу координатора;
    - сообщение сведений о работоспособности, если операцию не удалось выполнить, исчерпав все повторные попытки.

> [!NOTE]
> Приложение для управления исправлениями использует для включения и выключения узла, а также проверки работоспособности системную службу Service Fabric, которая называется Repair Manager. Задание восстановления, созданное приложением для управления исправлениями, отслеживает ход обновления для каждого узла.

## <a name="prerequisites"></a>Технические условия

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Убедитесь, что виртуальные машины Azure запущены под управлением Ubuntu 16.04
На момент написания этого документа Ubuntu 16.04 (`Xenial Xerus`) является единственной поддерживаемой версией.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-62x-and-above"></a>Убедитесь, что кластер Service Fabric для Linux имеет версию 6.2.x или более позднюю.

Приложение для управления исправлениями Linux использует определенные функции среды выполнения, которые доступны только в среде выполнения Service Fabric версии 6.2.x или более поздней.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Включение службы Repair Manager (если она еще не запущена)

Приложению для управления исправлениями необходимо, чтобы системная служба Repair Manager была включена в кластере.

#### <a name="azure-clusters"></a>Кластеры Azure

В кластерах Azure под управлением Linux на уровне надежности Silver и Gold служба Repair Manager включена по умолчанию. В кластерах Azure на уровне надежности Bronze служба Repair Manager по умолчанию не включена. Если служба уже включена, сведения о ее выполнении отображаются в разделе системных служб в Service Fabric Explorer.

##### <a name="azure-portal"></a>Портал Azure
Диспетчер восстановления можно включить на портале Azure при настройке кластера. Выберите параметр **Включить диспетчер восстановления** в разделе **Дополнительные функции** во время настройки кластера.
![Снимок экрана с подключением диспетчера восстановления на портале Azure](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Модель развертывания диспетчера ресурсов Azure
Службу диспетчера восстановления также можно включить для новых и существующих кластеров Service Fabric с помощью [модели развертывания Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm). Получите шаблон для кластера, который требуется развернуть. Можно использовать примеры шаблонов или создать пользовательский шаблон модели развертывания Azure Resource Manager. 

Чтобы включить службу диспетчера восстановления с помощью [шаблона модели развертывания Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm), сделайте следующее:

1. Сначала убедитесь, что для `apiversion` задано значение `2017-07-01-preview` для ресурса `Microsoft.ServiceFabric/clusters`. Если оно другое, необходимо задать для `apiVersion` значение `2017-07-01-preview` или выше:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Включите службу Repair Manager, добавив следующий раздел `addonFeatures` после раздела `fabricSettings`:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. После обновления шаблона кластера примените изменения и дождитесь завершения обновления. Теперь отображаются сведения о выполнении службы Repair Manager в кластере. Он указан в разделе системных служб в Service Fabric Explorer под названием `fabric:/System/RepairManagerService`. 

### <a name="standalone-on-premises-clusters"></a>Автономные локальные кластеры

Изолированные кластеры Service Fabric для Linux не поддерживались на момент написания этого документа.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Отключение автоматического обновления ОС на всех узлах

Автоматическое обновление операционной системы может привести к потере доступности или изменению в поведении выполняющихся приложений. Приложение для управления исправлениями по умолчанию попытается выключить автоматическое обновление ОС на каждом узле кластера, чтобы избежать таких сценариев.
[Автоматические обновления](https://help.ubuntu.com/community/AutomaticSecurityUpdates) Ubuntu отключены приложением для управления исправлениями.

## <a name="download-the-app-package"></a>Загрузка пакета установки

Приложение и сценарии установки можно скачать по [ссылке на архив](https://go.microsoft.com/fwlink/?linkid=867984).

Приложение в формате sfpkg можно скачать по [ссылке на SFPKG-файл](https://aka.ms/POA/POA_v2.0.3.sfpkg). Это удобно для [развертывания приложения на основе Azure Resource Manager](service-fabric-application-arm-resource.md).

## <a name="configure-the-app"></a>Настройка приложения

Поведение приложения для управления исправлениями можно настроить в соответствии с вашими требованиями. Значения по умолчанию можно переопределить, передав параметр приложения во время создания или обновления приложения. Параметры приложения можно задать, указав параметр `ApplicationParameter` командлету `Start-ServiceFabricApplicationUpgrade` или `New-ServiceFabricApplication`.

|**Параметр**        |**Тип**                          | **Дополнительные сведения**|
|:-|-|-|
|MaxResultsToCache    |длинное целое                              | Максимальное количество результатов обновления, которые необходимо кэшировать. <br>Значение по умолчанию — 3000 при условии, что: <br> - количество узлов —20; <br> - количество обновлений на узле в месяц — 5; <br> - количество результатов на операцию может быть 10; <br> - сохраняются результаты за последние три месяца. |
|TaskApprovalPolicy   |Перечисление. <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy указывает политику, которая будет использоваться службой координатора для установки обновлений на узлы кластера Service Fabric.<br>                         Допустимые значения: <br>                                                           <b>NodeWise</b>. Обновления устанавливаются на один узел за раз. <br>                                                           <b>UpgradeDomainWise</b>. Обновления устанавливаются на один домен обновления за раз. (Максимум на всех узлах, принадлежащих домену обновления, можно выполнить обновление).
| UpdateOperationTimeOutInMinutes | Int <br>(Значение по умолчанию: 180)                   | Указывает время ожидания для любой операции обновления (скачивание или установка). Если операция не завершена по истечении заданного времени ожидания, она будет прервана.       |
| RescheduleCount      | Int <br> (Значение по умолчанию: 5)                  | Максимальное количество операций изменения службой расписания обновления ОС в случае постоянных сбоев операции.          |
| RescheduleTimeInMinutes  | Int <br>(Значение по умолчанию: 30) | Интервал, согласно которому служба будет изменять расписание обновления ОС, если сбой продолжает происходить. |
| UpdateFrequency           | Строка с разделителями-запятыми (по умолчанию: Weekly, Wednesday, 7:00:00 [еженедельно, среда, 07:00:00])     | Интервал для установки обновлений ОС в кластере. Формат и возможные значения: <br>— Ежемесячно, ДД,ЧЧ:ММ:СС (например: Monthly, 5, 12:22:32); <br> — Еженедельно, ДЕНЬ, ЧЧ:ММ:СС (например: Weekly, Tuesday, 12:22:32);  <br> - Ежедневно, ЧЧ:ММ:СС (например: Daily, 12:22:32);  <br> — Никогда — указывает, что обновление не должно выполняться.  <br><br> Все значения времени указаны в формате UTC.|
| UpdateClassification | Строка с разделителями-запятыми (по умолчанию: "securityupdates") | Типы обновления, которые должны быть установлены на узлах кластера. Допустимые значения: securityupdates, all. <br> — securityupdates — устанавливаются только обновления для системы безопасности. <br> — all — устанавливаются все доступные обновления из apt.|
| ApprovedPatches | Строка с разделителями-запятыми (по умолчанию: "") | Это список утвержденных обновлений, которые должны быть установлены на узлах кластера. Разделенный запятыми список содержит утвержденные пакеты и при необходимости требуемую целевую версию.<br> Например, "apt-utils = 1.2.10ubuntu1, python3-jwt, apt-transport-https < 1.2.194, libsystemd0 >= 229-4ubuntu16". <br> Результат для указанного выше установит <br> — apt-utils версии 1.2.10ubuntu1, если она доступна в apt-cache. Если конкретно эта версия недоступна, команда является холостой. <br> — python3-jwt выполняет обновление до последней доступной версии. Если пакет не указан, команда является холостой. <br> — apt-transport-https обновляется до последней версии, которая меньше чем 1.2.194. Если версия не указана, команда является холостой. <br> — libsystemd0 обновляется до последней версии, большей или равной 229-4ubuntu16. Если такой версии не существует, команда является холостой.|
| RejectedPatches | Строка с разделителями-запятыми (по умолчанию: "") | Это список обновлений, которые не должны быть устанавливаться на узлах кластера. <br> Например: "bash, sudo". <br> Предыдущее отфильтровывает bash, sudo и они не получают каких-либо обновлений. |


> [!TIP]
> Чтобы обновление ОС началось немедленно, установите `UpdateFrequency` в соответствии с временем развертывания приложения. Например, у вас тестовый кластер с пятью узлами и вы планируете развернуть приложение около 17:00 UTC. Если предположить, что обновление или развертывание приложения занимает не более 30 минут, задайте для WUFrequency значение Daily, 17:30:00.

## <a name="deploy-the-app"></a>Развертывание приложения

1. Подготовьте кластер, закончив все обязательные шаги.
2. Разверните приложение для управления исправлениями, как любое другое приложение Service Fabric. Приложение можно развернуть с помощью PowerShell или Azure Service Fabric CLI. Выполните инструкции из статьи [Развертывание и удаление приложений с помощью PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) или [Развертывание приложения в кластере Service Fabric](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application).
3. Чтобы настроить приложение во время развертывания, передайте `ApplicationParameter` в командлет `New-ServiceFabricApplication` или предоставленный сценарий. Для удобства сценарии PowerShell (Deploy.ps1) и bash (Deploy.sh) предоставляются вместе с приложением. Использование сценария

    - Подключение к кластеру Service Fabric
    - Выполните сценарий развертывания. При необходимости передайте сценарию параметр приложения. Например: .\Deploy.ps1 -ApplicationParameter @{ UpdateFrequency = "Ежедневно, 11:00:00"} OR ./Deploy.sh "{\"UpdateFrequency\":\"Daily, 11:00:00\"}" 

> [!NOTE]
> Сохраните сценарий и папку приложения PatchOrchestrationApplication в одном каталоге.

## <a name="upgrade-the-app"></a>Обновление приложения

Чтобы обновить установленное приложение для управления исправлениями, выполните инструкции из статьи [Обновление приложения Service Fabric с помощью PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) или раздела [sfctl application upgrade](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade).

## <a name="remove-the-app"></a>Удаление приложения

Чтобы удалить приложение, выполните шаги из статьи [Развертывание и удаление приложений с помощью PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) или раздела [sfctl application delete](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete).

Для удобства сценарии PowerShell (Undeploy.ps1) и bash (Undeploy.sh) предоставляются вместе с приложением. Использование сценария

  - Подключение к кластеру Service Fabric
  - Выполнение сценария Undeploy.ps1 or Undeploy.sh

> [!NOTE]
> Сохраните сценарий и папку приложения PatchOrchestrationApplication в одном каталоге.

## <a name="view-the-update-results"></a>Просмотр результатов обновления

Приложение для управления исправлениями предоставляет REST API для отображения исторических результатов. Ниже приведен пример результата: ```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```.
```json
[ 
  { 
    "NodeName": "_bronze_0", 
    "UpdateOperationResults": [ 
      { 
        "OperationResult": "succeeded", 
        "NodeName": "_bronze_0", 
        "OperationTime": "2017-11-21T12:39:29.0435917Z", 
        "UpdateDetails": [ 
          { 
            "UpdateId": "linux-cloud-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-headers-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-image-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "python3-apport:amd64=2.20.1-0ubuntu2.13", 
            "ResultCode": "succeeded" 
          }, 
        ], 
        "OperationType": "installation", 
        "UpdateClassification": "securityupdates", 
        "UpdateFrequency": "Daily, 7:00:00", 
        "RebootRequired": true, 
        "ApprovedList": "", 
        "RejectedList": "" 
      } 
    ] 
  } 
] 
```

Ниже приводится описание полей JSON:

Поле | Значения | Сведения
-- | -- | --
OperationResult | 0 — успешно<br> 1 — выполнено с ошибками<br> 2 — сбой<br> 3 — прервано<br> 4 — прервано с временем ожидания | Указывает результат общей операции (обычно с установкой одного или нескольких обновлений).
ResultCode | Как и для OperationResult | Это поле указывает результат операции установки для отдельного обновления.
OperationType | 1 — установка<br> 0 — поиск и скачивание| По умолчанию в результатах отображается только тип операции "Установка".
UpdateClassification | Значение по умолчанию: "securityupdates" | Тип обновления, установленный во время операции обновления
UpdateFrequency | Значение по умолчанию: "Еженедельно, среда, 7:00:00" | Обновите частоту, настроенную для этого обновления.
RebootRequired | true — требовалась перезагрузка<br> false — перезагрузка не требовалась | Указывает, требовалась ли перезагрузка для завершения установки обновлений.
ApprovedList | Значение по умолчанию: "" | Список утвержденных исправлений для этого обновления
RejectedList | Значение по умолчанию: "" | Список отклоненных исправлений для этого обновления

Если обновление еще не запланировано, поле результата JSON будет пустым.

Войдите в кластер для запроса результатов обновления. Найдите адрес реплики для первичной службы координатора и перейдите по этому URL-адресу в браузере: http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetResults.

Конечная точка REST для службы координатора имеет динамический порт. Точный URL-адрес можно узнать в Service Fabric Explorer. Например, результаты доступны в `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults`.

![Представление конечной точки REST](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>События диагностики и работоспособности

### <a name="diagnostic-logs"></a>Журналы диагностики

Журналы приложения для оркестрации исправлений собираются как часть журналов среды выполнения Service Fabric.

При необходимости можно собирать журналы, используя любой конвейер или средство диагностики. Приложение для оркестрации исправлений использует следующие фиксированные идентификаторы поставщиков для записи событий в журнал с использованием класса [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0).

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Отчеты о работоспособности

Приложение для управления исправлениями также публикует отчеты о работоспособности для службы координатора или службы агента узла в указанных ниже случаях.

#### <a name="an-update-operation-failed"></a>Сбой операции обновления

Если на узле происходит сбой операции обновления, создается отчет о работоспособности для службы агента узла. Отчет о работоспособности содержит имя проблемного узла.

После установки исправления на проблемном узле отчет автоматически очищается.

#### <a name="the-node-agent-daemon-service-is-down"></a>Служба управляющей программы агента узла не работает

Если на узле не работает служба управляющей программы агента узла, генерируется отчет о работоспособности уровня предупреждения для службы агента узла.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Служба Repair Manager не включена

Если служба Repair Manager не найдена в кластере, для службы координатора создается отчет о работоспособности уровня предупреждения.

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

В. **Почему мой кластер находится в состоянии ошибки, когда работает приложение для управления исправлениями?**

О. Во время установки приложение для управления исправлениями отключает или перезагружает узлы. Эта операция может вызвать временную неисправность кластера.

Основываясь на политике приложения, во время установки исправления любой узел может выйти из строя *или же* сбой может произойти во всем домене обновления.

К завершению установки узлы будут повторно включены после перезагрузки.

В примере ниже кластер временно перешел в состояние ошибки, поскольку два узла вышли из строя и политика MaxPercentageUnhealthNodes была нарушена. Это временная ошибка. Она исчезнет, как только установка исправлений будет завершена.

![Представление неработоспособного кластера](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Если проблема не будет устранена, см. сведения в разделе об устранении неполадок.

В. **Приложение для управления исправлениями находится в состоянии предупреждения**

О. Проверьте, является ли отчет о работоспособности, опубликованный для приложения, основной причиной. Обычно в описании предупреждения содержатся сведения о проблеме. Если проблема временная, приложение автоматически восстановится из этого состояния.

В. **Что делать, если нужно срочно обновить операционную систему, а кластер неработоспособен?**

О. Приложение для управления исправлениями не устанавливает обновления, пока кластер находится в неработоспособном состоянии. Попробуйте восстановить работоспособность кластера, чтобы разблокировать рабочий процесс приложения для управления исправлениями.

В. **Почему операция исправления в кластере так долго выполняется?**

О. Время, необходимое приложению для управления исправлениями, обычно зависит от факторов ниже.

- Политика службы координатора. 
  - Политика по умолчанию (`NodeWise`) ограничивает исправление только одним узлом за раз. Чтобы повысить скорость исправления для всех кластеров, рекомендуем использовать политику `UpgradeDomainWise` (особенно если речь идет о больших кластерах).
- Количество обновлений, доступных для загрузки и установки. 
- Среднее время загрузки и установки обновления (не более нескольких часов).
- Производительность виртуальных машин и пропуская способность сети.

В. **Как приложение для управления исправлениями определяет, какие обновления являются обновлениями безопасности?**

О. Приложение для управления исправлениями использует логику, относящуюся к дистрибутивам, чтобы определить, какие обновления среди доступных являются обновлениями системы безопасности. Например:  В ubuntu приложение выполняет поиск обновлений из архивов $RELEASE-безопасность, $RELEASE-обновления ($RELEASE = xenial или linux Стандартная версия выпуска). 

 
В. **Как можно запросить определенную версию пакета?**

О. Используйте параметры ApprovedPatches для запроса пакетов определенной версии. 


В. **Что происходит с автоматическими обновлениями, доступными в Ubuntu?**

О. Как только вы установите приложение для управления исправлениями в кластер, автоматические обновления в узле кластера будут отключены. Все периодические рабочие процессы обновления будут управляться приложением для управления исправлениями.
Чтобы обеспечить согласованность среды в кластере, мы рекомендуем устанавливать обновления только через приложение для управления исправлениями. 
 
В. **Происходит ли в приложении для управления исправлениями очистка неиспользуемых пакетов после обработки?**

О. Да, очистка выполняется в рамках действий после установки. 

В. **Можно ли с помощью приложения для управления исправлениями решать проблемы в кластере для разработки (кластер с одним узлом)?**

О. Нет. С помощью приложения для управления исправлениями проблемы в кластере с одним узлом решить нельзя. Это ограничение сделано намеренно, так как [системные службы Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) или приложения клиента будут простаивать, поэтому диспетчер никогда не утвердит любое задание восстановления и последующее внесение исправлений.

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="a-node-is-not-coming-back-to-up-state"></a>Узел не возвращается в рабочее состояние

**Возможная причина, по которой узел остался в отключенном состоянии**

Проверка безопасности находится в состоянии ожидания. В этом случае необходимо убедиться, что достаточное количество узлов находятся в работоспособном состоянии.

**Возможная причина, по которой узел остался в отключенном состоянии**

- Узел был отключен вручную.
- Узел был отключен из-за текущего задания инфраструктуры Azure.
- Узел был временно отключен приложением для управления исправлениями для исправления узла.

**Возможная причина, по которой узел остался в отключенном состоянии**

- Узел был переведен в неработоспособное состояние вручную.
- Узел находится в режиме перезапуска (который может быть инициирован приложением для управления исправлениями).
- Узел не работает из-за неисправности виртуальной машины, компьютера или проблем с подключением.

### <a name="updates-were-skipped-on-some-nodes"></a>Обновления на некоторых узлах пропущены

Приложение управления исправлениями пытается установить обновление согласно политике перепланирования. Служба пытается восстановить узел и пропустить обновление в соответствии с политикой приложения.

В этом случае для службы агента узла будет создан отчет о работоспособности уровня предупреждения. Результат обновления также содержит возможные причины сбоя.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>При установке обновления кластер перешел в неработоспособное состояние

Некорректное обновление может привести к потере работоспособности приложения или кластера в определенном узле или домене обновления. Приложение для управления исправлениями останавливает любые последующие операции обновления, пока работоспособность кластера не восстановится.

Администратор должен определить, почему обновление приводит к ухудшению работоспособности приложения или кластера из-за ранее установленного обновления.

## <a name="disclaimer"></a>Отказ от ответственности

Приложение для управления исправлениями собирает данные телеметрии для отслеживания использования и производительности. Телеметрия приложения выполняется в соответствии с настройкой телеметрии в среде выполнения Service Fabric (которая включена по умолчанию).

## <a name="release-notes"></a>Заметки о выпуске

### <a name="version-010"></a>Версия 0.1.0
- Закрытые отчеты предварительной версии

### <a name="version-200"></a>Версия 2.0.0
- Общедоступный выпуск

### <a name="version-201"></a>Версия 2.0.1
- Повторная компиляция приложения с использованием последней версии пакета SDK для Service Fabric

### <a name="version-202"></a>Версия 2.0.2 
- Устранена проблема, когда предупреждение о работоспособности исчезало при перезагрузке.

### <a name="version-203-latest"></a>Версии 2.0.3 (последняя версия)
- Исправления проблемы, где ЦП службы управляющей программы агента узла достигнуто не более 99% на виртуальных машинах Standard_D1_v2.
- Исправления проблемы, что негативно влияет на установку исправлений-жизненный цикл на узле в случае узлов с именем, который представляет подмножество имя текущего узла. Для таких узлов, возможно, пропущено исправление или ожидается перезагрузка.
- Исправлена ошибка, из-за чего управляющей программы агента узла постоянно аварийно завершается при поврежден параметры передаются в службу.