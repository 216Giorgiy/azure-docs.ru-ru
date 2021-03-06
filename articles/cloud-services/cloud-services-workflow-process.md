---
title: Рабочий процесс архитектуры Azure виртуальной Машины Windows | Документация Майкрософт
description: В этой статье представлен обзор рабочих процессов, при развертывании службы.
services: cloud-services
documentationcenter: ''
author: genlin
manager: Willchen
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/08/2019
ms.author: kwill
ms.openlocfilehash: 7c8459a6694663a49203b6ec21a760d3e6bd60c3
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "60150526"
---
#    <a name="workflow-of-windows-azure-classic-vm-architecture"></a>Рабочий процесс из классической виртуальной Машины архитектуры Windows Azure 
В этой статье Обзор рабочих процессов, возникающих при развертывании или обновлении ресурс Azure, например, виртуальная машина. 

> [!NOTE]
>В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: Resource Manager и классическая модель. В этой статье рассматривается использование классической модели развертывания.

На следующей схеме представлен архитектура ресурсов Azure.

![Рабочий процесс Azure](./media/cloud-services-workflow-process/workflow.jpg)

## <a name="workflow-basics"></a>Основы рабочих процессов
   
**Ответ**. RDFE / FFE — это путь взаимодействия пользователя в структуру. RDFE (RedDog переднего плана) — это открытый API, который является внешний интерфейс на портал управления и API управления службами, например Visual Studio, MMC в службе Azure и т. д.  Все запросы от пользователя пройти RDFE. FFE (Fabric Интерфейсная часть) — это слой, который преобразует запросы из RDFE в fabric команды. Все запросы из RDFE проходят FFE, чтобы получить доступ к структуре контроллерам.

**B**. Контроллер структуры отвечает за обслуживание и мониторинг всех ресурсов в центре обработки данных. Он взаимодействует с агентами узла fabric на структуре ОС отправка данных, таких как версия гостевой ОС, пакет службы, конфигурации службы и состояние службы.

**C**. Агент узла, содержащего OSsystem узла и отвечает за настройку гостевых ОС и взаимодействия с гостевой агент (WindowsAzureGuestAgent), чтобы обновить роль к состоянии целью и сделать пульса проверяет с гостевым агентом. Если агент узла не получит ответ пульса за 10 минут, агент узла перезапускает гостевой ОС.

**C2**. WaAppAgent несет ответственность за установку, настройку и обновление WindowsAzureGuestAgent.exe.

**D**.  WindowsAzureGuestAgent отвечает за следующее:

1. Настройка гостевой ОС, включая брандмауэр, списки управления доступом, ресурсы LocalStorage, пакет службы и конфигурации и сертификаты. Настройка идентификатор безопасности для учетной записи пользователя, которой будет запускаться роли.
2. Связи на состояние роли в структуру.
3. Запуск WaHostBootstrapper и мониторинга, чтобы убедиться, что роль находится в состоянии цели.

**E**. WaHostBootstrapper отвечает за:

1. Чтение конфигурации роли, и запуск всех необходимых задач и процессов для настройки и запуска роли.
2. Мониторинг всех дочерних процессов.
3. Вызов события StatusCheck процесс размещения роли.

**F**. IISConfigurator выполняется, если роль была настроена веб-роли полноценного сервера IIS (он не запустится для ролей SDK 1.2 HWC). Он отвечает за:

1. Запуск стандартных служб IIS
2. Настройка модуль переопределения в веб-конфигурация
3. Настройка AppPool настроенные роли в модели службы
4. Настройка ведения журнала IIS для указания на папку DiagnosticStore LocalStorage
5. Настройка разрешений и списки управления доступом
6. Веб-сайт находится в % roleroot%:\sitesroot\0, а также указывает пул приложений в это расположение для запуска служб IIS. 

**G**. Задачи запуска определяются в модели роли и создано WaHostBootstrapper. Задачи запуска можно настроить для асинхронного выполнения в фоновом режиме и загрузчика узла будет начинаться задачи запуска и затем перейти к изучению других задач запуска. Задачи запуска можно также настроить для работы в режиме простой (по умолчанию), в котором начальный загрузчик узел будет ожидать задачи запуска завершить выполнение и возвращает код выхода успешное завершение (0) перед продолжением к следующей задаче запуска.

**H**. Эти задачи являются частью пакета SDK и определяются как подключаемые модули в определении роли службы (.csdef). При развертывании в задачах запуска **DiagnosticsAgent** и **RemoteAccessAgent** являются уникальными, в том, что каждый из них определить два, один обычный и выполнить задачи запуска, для которой **/blockStartup** параметра. Обычный запуск задачи определяется как фоновая задача запуска, чтобы оно могло выполняться в фоновом режиме во время работы сама роль. **/BlockStartup** задачи запуска определяется как задачу простого запуска таким образом, чтобы WaHostBootstrapper ожидает его закрытию перед продолжением. **/BlockStartup** задача ожидает получения обычная задача для завершения инициализации, а затем завершает работу и разрешить загрузчика узла продолжить. Это делается, чтобы перед началом процессы роли (это осуществляется посредством задач /blockStartup) можно настроить диагностику и доступ по протоколу RDP. Это также позволяет диагностики и доступ по протоколу RDP для продолжения выполнения после завершения начального загрузчика узла задачи запуска (это осуществляется посредством обычного задания).

**Я**. WaWorkerHost — это стандартный хост-процесс для обычных рабочих ролей. Этот хост-процесс размещаются библиотек DLL и код точки входа, например OnStart и выполнения роли.

**J**. WaWebHost — это процесс стандартного узла для веб-ролей, если они настроены для использования пакета SDK 1.2-совместимой Внедряемое Webcore (HWC). Роли можно включить режим HWC, удалив элемент из определения службы (.csdef). В этом режиме служба кода и библиотек DLL, выполняются из процесса WaWebHost. IIS (w3wp) не используется и существуют нет пулов приложений, настроены в диспетчере IIS, так как IIS размещается внутри WaWebHost.exe.

**K**. WaIISHost является хост-процессу код точки входа роли для веб-ролей, использующие полные службы IIS. Этот процесс загружает первый библиотеку DLL, которую можно найти, использующий **RoleEntryPoint** класса и выполняет код из этого класса (OnStart, выполнения, OnStop). Любой **RoleEnvironment** события (например, StatusCheck и Changed), которые создаются в классе RoleEntryPoint вызываются в этот процесс.

**L**. W3wp — это стандартный рабочий процесс IIS, который используется, если роль настроена для использования полноценного сервера IIS. Это выполняется пул приложений, который настроен с IISConfigurator. Любой RoleEnvironment (например, StatusCheck и Changed), которые созданы здесь событий в этом процессе. Обратите внимание, что события RoleEnvironment будет срабатывать в обоих расположениях (WaIISHost и w3wp.exe), если вы подписываетесь на события в оба процесса.

## <a name="workflow-processes"></a>Бизнес-процессов

1. Пользователь выполняет запрос, например отправка cspkg-файл и cscfg-файлов, предписывая ресурс, чтобы остановить или внесения изменений конфигурации, и т. д. Это можно сделать через портал Azure или средство, которое использует API управления службами, например функцию публикации Visual Studio. Этот запрос направляется к RDFE для выполнения всех связанных с подпиской работать и обмениваться данными запроса для FFE. Все эти шаги рабочего процесса являются развернуть новый пакет и запустить его.
2. FFE находит правильный пула машин (с учетом отзывов клиентов, таких как территориальная группа или географическое расположение, а также входные данные из структуры, таких как доступность машин) и взаимодействует с контроллером структуры master в этом пуле машин.
3. Fabric controller находит узел, имеющий доступные ядра ЦП (или запускает копию нового узла). Пакет службы и конфигурации копируются на узел, и контроллер структуры взаимодействует с агентом узла на узле ОС для развертывания пакета (настройки частные интерфейсы, порты, гостевой ОС и т. д.).
4. Агент узла запускает гостевой ОС и взаимодействует с гостевым агентом (WindowsAzureGuestAgent). Узел отправляет пакеты пульса для виртуальной машины, чтобы убедиться в том, что роль работает над созданием состояния цели.
5. WindowsAzureGuestAgent настраивает гостевой ОС (брандмауэра, списки управления доступом, LocalStorage и т. д.) и копирует новый XML-файл конфигурации c:\Config затем начинает процесс WaHostBootstrapper.
6. Для веб-ролей Full IIS WaHostBootstrapper запускает IISConfigurator и дает ему удалить любые существующие пулов приложений веб-роли из служб IIS.
7. Считывает WaHostBootstrapper **запуска** задачи из E:\RoleModel.xml и начала выполнения задачи запуска. WaHostBootstrapper ожидает, пока все задачи запуска простого завершения и возвращается сообщение «success».
8. Для веб-ролей Full IIS, WaHostBootstrapper сообщает IISConfigurator для настройки IIS AppPool и указывает на узел для `E:\Sitesroot\<index>`, где `<index>` является основанным на 0 индексом в число <Sites> элементов, определенных для службы.
9. WaHostBootstrapper запустит процесс узла в зависимости от типа роли:
    1. **Рабочая роль**: Запускается WaWorkerHost.exe. WaHostBootstrapper выполняет метод OnStart(). После возврата, WaHostBootstrapper начинает выполнять метод Run() одновременно помечает роли как готовые и помещает его в подсистеме балансировки нагрузки (если InputEndpoints определены). WaHostBootsrapper затем запускает цикл проверки состояния роли.
    1. **Роль веб-пакета SDK 1.2 HWC**: WaWebHost запускается. WaHostBootstrapper выполняет метод OnStart(). После возврата, WaHostBootstrapper начинает выполнять метод Run() и одновременно помечает роли как готовые и помещает его в подсистеме балансировки нагрузки. WaWebHost выдает запрос прогрева (GET /do.rd_runtime_init). Все веб-запросы отправляются WaWebHost.exe. WaHostBootsrapper затем запускает цикл проверки состояния роли.
    1. **Полное веб-роль IIS**: aIISHost запущена. WaHostBootstrapper выполняет метод OnStart(). После возврата, он начинает выполнять метод Run() и одновременно помечает роли как готовые и помещает его в подсистеме балансировки нагрузки. WaHostBootsrapper затем запускает цикл проверки состояния роли.
10. Входящие веб-запросы для полноценного сервера IIS веб-триггеров роль IIS, чтобы начать процесс W3WP и обслуживать запрос, так же, как если бы в локальной среде IIS.

## <a name="log-file-locations"></a>Расположение файлов журнала

**WindowsAzureGuestAgent**

- C:\Logs\AppAgentRuntime.Log.  
Этот журнал содержит изменения в службе, включая запускает, останавливает и новые конфигурации. Если служба не будет изменяться, можно ожидать большого промежутков времени в этот файл журнала.
- C:\Logs\WaAppAgent.Log.  
Этот журнал содержит состояние обновлений и уведомлений пульса и обновляется каждые 2 – 3 секунды.  Этот журнал содержит исторические представления состояния экземпляра и сообщит, если экземпляр не был в состоянии готовности.
 
**WaHostBootstrapper**

C:\Resources\Directory\<deploymentID>.<role>.DiagnosticStore\WaHostBootstrapper.log
 
**WaWebHost**

C:\Resources\Directory\<guid>.<role>\WaWebHost.log
 
**WaIISHost**

C:\Resources\Directory\<deploymentID>.<role>\WaIISHost.log
 
**IISConfigurator**

C:\Resources\Directory\<deploymentID >.<role> \IISConfigurator.log
 
**Журналы IIS**

C:\Resources\Directory\<guid>.<role>.DiagnosticStore\LogFiles\W3SVC1
 
**Журналы событий Windows**

D:\Windows\System32\Winevt\Logs
 



