---
title: Сбор данных из среды с помощью Azure Log Analytics | Документация Майкрософт
description: В этой статье содержатся сведения о том, как собирать данные и наблюдать за компьютерами, размещенными в локальной или другой облачной среде, с помощью Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 2a21c7867bf0dd2d6ca6ee0bd9025739315c8d0a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003324"
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Сбор данных с компьютеров в среде с помощью Log Analytics

Azure Log Analytics позволяет собирать и обрабатывать данные компьютеров с ОС Windows или Linux, размещенные:

* на [виртуальных машинах Azure](log-analytics-quick-collect-azurevm.md) с помощью расширения виртуальной машины Log Analytics; 
* в центре обработки данных в качестве физических серверов или виртуальных машин;
* на виртуальных машинах в облачной службе (например, Amazon Web Services (AWS)).

Компьютеры, размещенные в среде, можно подключать непосредственно к службе Log Analytics. Если вы уже проводите мониторинг этих компьютеров с помощью System Center Operations Manager 2012 R2, 2016 или версии 1801, можно интегрировать группы управления Operations Manager с Log Analytics и продолжить обслуживание рабочих процессов ИТ-службы.  

## <a name="overview"></a>Обзор

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Перед анализом и обработкой собранных данных необходимо установить и подключить агенты для всех компьютеров, данные которых нужно отправить в службу Log Analytics. Вы можете установить агенты на локальных компьютерах с помощью программы установки, командной строки или DSC (настройки требуемого состояния) в службе автоматизации Azure. 

Этот агент для Linux и Windows обменивается исходящими данными со службой Log Analytics и службой автоматизации Azure через TCP-порт 443. Если компьютер подключен к брандмауэру или прокси-серверу для обмена данными через Интернет, ознакомьтесь с разделом [предварительных требований](#prerequisites), чтобы должным образом настроить сеть.  Если политики ИТ-безопасности запрещают подключение компьютеров в сети к Интернету, вы можете настроить [шлюз OMS](log-analytics-oms-gateway.md), а затем настроить агент и подключиться к Log Analytics через шлюз. После этого агент сможет получать сведения о конфигурации и отправлять данные, которые зависят от правил сбора и включенных решений. 

Компьютер, отслеживаемый решением System Center 2016 Operations Manager или Operations Manager 2012 R2, может использоваться как многосетевой. С помощью службы Log Analytics будет выполняться сбор данных и их пересылка в службу, а компьютер по-прежнему будет отслеживаться решением [Operations Manager](log-analytics-om-agents.md). Компьютеры Linux, отслеживаемые группой управления Operations Manager, интегрированной с Log Analytics, не получают конфигурацию для источников данных. Они пересылают собранные данные через группы управления. Агент для Windows может отправлять отчет в четыре рабочих области, а агент для Linux поддерживает отчетность только для одной рабочей области.  

Агент для Linux и Windows предназначен не только для подключения к Log Analytics, в нем также поддерживается подключение к службе автоматизации Azure для размещения гибридной рабочей роли runbook и решений управления (например, "Отслеживание изменений" и "Управление обновлениями").  Дополнительные сведения о гибридной рабочей роли Runbook см. в разделе [Общие сведения об архитектуре автоматизации](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Поддерживаемые операционные системы Windows
Для агента Windows официально поддерживаются следующие версии операционной системы Windows:

* Windows Server 2008 с пакетом обновления 1 (SP1) или более поздней версии;
* Windows 7 с пакетом обновления 1 и более поздней версии.

## <a name="supported-linux-operating-systems"></a>Поддерживаемые операционные системы Linux
Официально поддерживаются следующие дистрибутивы Linux.  Однако агент Linux может выполняться и на других дистрибутивах, помимо следующих:  Если не указано иное, все вспомогательные версии поддерживаются для каждого основного номера версии в списке.  

* Amazon Linux 2012.09–2015.09 (x86/x64)
* CentOS Linux 5, 6 и 7 (x86/x64)  
* Oracle Linux 5, 6 и 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 и 7 (x86/x64)
* Debian GNU/Linux 6, 7 и 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 и 12 (x86/x64)

## <a name="tls-12-protocol"></a>Протокол TLS 1.2
Чтобы обеспечить безопасность данных, передаваемых в передаче в Log Analytics, мы настоятельно рекомендуем настроить агент на использование протокола TLS как минимум версии 1.2. Более старые версии протоколов TLS/SSL оказались уязвимы. Хотя они все еще используются для обеспечения обратной совместимости, применять их **не рекомендуется**.  См. дополнительные сведения о [безопасной отправке данных с помощью TLS 1.2](log-analytics-data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Требования к брандмауэру в сети
Ниже приводятся сведения о конфигурации прокси-сервера и брандмауэра, необходимые для взаимодействия агента Windows и Linux с Log Analytics.  

|Ресурс агента|порты; |Направление |Обход проверки HTTPS|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Порт 443 |Исходящий и входящий|Yes |  
|*.oms.opinsights.azure.com |Порт 443 |Исходящий и входящий|Yes |  
|*.blob.core.windows.net |Порт 443 |Исходящий и входящий|Yes |  
|*.azure-automation.net |Порт 443 |Исходящий и входящий|Yes |  


Если планируется использование Azure Automation Hybrid Runbook Worker для подключения к службе автоматизации и регистрации в ней, чтобы применить runbook в вашей среде, они должны иметь доступ к номеру порта и URL-адресам, описанным в разделе [Настройка сети для Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md#network-planning). 

Агенты Windows и Linux поддерживают обмен данными со службой Log Analytics через прокси-сервер или шлюз OMS по протоколу HTTPS.  Поддерживается анонимная и базовая аутентификация (с именем пользователя и паролем).  Для агента Windows, подключенного непосредственно к службе, конфигурация прокси-сервера указывается во время установки или [после развертывания](log-analytics-agent-manage.md#update-proxy-settings) — в панели управления или с помощью PowerShell.  

Для агента Linux прокси-сервер определяется во время или [после установки](/log-analytics-agent-manage.md#update-proxy-settings), или при изменении файла конфигурации proxy.conf.  Конфигурация прокси-сервера агента Linux имеет следующий синтаксис:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Если прокси-сервер не требует аутентификацию, для агента Linux все равно необходимо указать псевдо-имя пользователя или пароль. Вы можете указать любое имя пользователя или пароль.

|Свойство| ОПИСАНИЕ |
|--------|-------------|
|Протокол | HTTPS |
|user | Необязательное имя пользователя для аутентификации прокси-сервера |
|password | Необязательный пароль для аутентификации прокси-сервера |
|proxyhost | Адрес или полное доменное имя прокси-сервера или шлюза OMS |
|порт | Номер дополнительного порта для прокси сервера или OMS шлюза |

Например: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Если в пароле использованы специальные знаки (например, \@), отобразится ошибка подключения прокси-сервера из-за неправильного анализа значения.  Чтобы обойти эту проблему, закодируйте пароль в URL-адрес, используя специальные инструменты (например, [URLDecode](https://www.urldecoder.org/)).  

## <a name="install-and-configure-agent"></a>Установка и настройка агента 
Подключение локальных компьютеров напрямую с помощью Log Analytics может осуществляться разными способами в зависимости от требований. В следующей таблице описан каждый метод. Эти данные помогут вам определить, какой из методов наиболее подходящий для вашей организации.

|Источник | Метод | ОПИСАНИЕ|
|-------|-------------|-------------|
| Компьютер Windows|- [Установка вручную](log-analytics-agent-windows.md)<br>- [DSC службы автоматизации Azure](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Шаблон Resource Manager с Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Установка агента Microsoft Monitoring Agent из командной строки или автоматически, например с помощью DSC службы автоматизации Azure, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications) или шаблона Azure Resource Manager, если Microsoft Azure Stack уже развернут в центре обработки данных.| 
|Компьютер Linux| [Установка вручную](log-analytics-quick-collect-linux-computer.md)|Установка агента для Linux путем вызова сценария-оболочки, размещенного в GitHub. | 
| System Center Operations Manager|[Интеграция Operations Manager с Log Analytics](log-analytics-om-agents.md) | Настройка интеграции между Operations Manager и Log Analytics для пересылки отчетов о собранных данных с компьютеров Linux и Windows в группу управления.|  

## <a name="next-steps"></a>Дополнительная информация

* Дополнительные сведения об источниках данных, доступных для сбора данных из операционных систем Windows или Linux, см. в статье [Data sources in Log Analytics](log-analytics-data-sources.md) (Источники данных в Log Analytics). 

* Узнайте больше об [операциях поиска по журналу](log-analytics-log-searches.md) , которые можно применять для анализа данных, собираемых из источников данных и решений. 

* Узнайте больше о [решениях](log-analytics-add-solutions.md) , которые расширяют функции службы Log Analytics и собирают данные в репозиторий OMS.