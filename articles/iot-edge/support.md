---
title: Поддерживаемые операционные системы и подсистемы контейнеров в Azure IoT Edge | Документация Майкрософт
description: Узнайте, в каких операционных системах можно запустить управляющую программу и среду выполнения Azure IoT Edge, а также какие подсистемы контейнеров поддерживают рабочие устройства.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/18/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5bc133e81f9917aafb406a6bfb27922cdba48ef5
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58190011"
---
# <a name="azure-iot-edge-supported-systems"></a>Поддерживаемые Azure IoT Edge системы

Есть много способов реализовать поддержку продуктов Azure IoT Edge.

**Отчеты об ошибках.** Основная часть усилий по разработке продукта Azure IoT Edge, прилагается в рамках проекта IoT Edge с открытым кодом. Вы можете сообщить об ошибках на соответствующей [странице](https://github.com/azure/iotedge/issues) для этого проекта. Внесенные исправления очень быстро попадают из проекта в обновление продукта.

**Служба поддержки пользователей корпорации Майкрософт**. Пользователи, приобретающие [план поддержки,](https://azure.microsoft.com/support/plans/) могут обращаться в службу поддержки пользователей корпорации Майкрософт, создав запрос прямо на [портале Azure](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Запросы функций.** Для продукта Azure IoT Edge запросы функций отслеживаются на специальной странице [User Voice](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Подсистемы контейнеров
Чтобы запустить модули, Azure IoT Edge требуется обработчик контейнеров, так как модули реализуются в виде контейнеров. Корпорация Майкрософт предоставляет для этого подсистему контейнеров moby-engine. Она основана на проекте Moby с открытым кодом. Также часто используются подсистемы контейнеров Docker CE и Docker EE. Они также основаны на проекте Moby с открытым кодом и полностью совместимы с Azure IoT Edge. Корпорация Майкрософт поддерживает системы, использующие эти подсистемы контейнеров, по принципу "насколько возможно", но не гарантирует исправление всех возможных проблем, связанных с их использованием. По этой причине мы рекомендуем использовать для рабочей среды только подсистему moby-engine.

<br>
<center>

![Кита как среду выполнения контейнера](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>Операционные системы
Azure IoT Edge работает в большинстве операционных систем, которые можно запускать контейнеры; Тем не менее все эти системы не поддерживаются одинаково. Операционные системы разделены на несколько уровней, которые определяют предоставляемый пользователям уровень поддержки.
* Системы первого уровня можно считать официально поддерживаемыми. Для уровня 1 систем Майкрософт:
    * выполняет автоматические тесты для этой операционной системы;
    * предоставляет для них пакеты установки.
* Системы уровня 2 можно считать совместимыми с Azure IoT Edge, то есть их использование не вызывает особых сложностей. Для систем уровня 2:
    * Майкрософт сделала специальных средств на платформах или знает партнера, успешно работающих Edge Интернета вещей Azure на платформе
    * для этих платформ могут подойти пакеты установки, предназначенные для других платформ.
    
Семейство ОС узла всегда должно совпадать с семейством гостевой ОС, используемой в контейнере модуля. Другими словами, контейнеры Linux можно использовать только в Linux, а контейнеры Windows — только в Windows. При использовании Windows, только процесс поддерживаются изолированных контейнерах, Hyper-V изолированных контейнерах.  

<br>
<center>

![Совпадает с базовой ОС гостевой ОС](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Уровень 1
Общедоступная версия

| Операционная система | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Raspbian-stretch | Нет  | Yes|
| Ubuntu Server 16.04 | Yes | Нет  |
| Ubuntu Server 18.04 | Yes | Нет  |

Общедоступная предварительная версия

| Операционная система | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core сборки 17763 | Yes | Нет  |
| Windows 10 IoT Корпоративная сборки 17763 | Yes | Нет  |
| Windows Server 2019 | Yes | Нет  |

Операционные системы Windows, указанные выше приведены требования для устройств под управлением Windows в контейнеры Windows. Эта конфигурация является единственной поддерживаемой конфигурацией для рабочей среды. Установочные пакеты Azure IoT Edge для Windows на использование контейнеров Linux в Windows; Однако эта конфигурация является только для разработки и тестирования. Контейнеры Linux в Windows не поддерживаются в рабочей среде. Для этого сценария разработки подойдет любая версия Windows 10 сборки 14393 или более поздней и Windows Server 2016 или более поздней версии.

### <a name="tier-2"></a>Уровень 2

| Операционная система | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Yes | Yes |
| Debian 8; | Yes | Yes |
| Debian 9 | Yes | Yes |
| RHEL 7.5 | Yes | Yes |
| Ubuntu 18.04 | Yes | Yes |
| Ubuntu 16.04. | Yes | Yes |
| Wind River 8 | Yes | Нет  |
| Yocto | Yes | Нет  |


## <a name="virtual-machines"></a>Виртуальные машины
Azure IoT Edge может выполняться на виртуальных машинах. С помощью виртуальной машины как IoT Edge устройства всего происходит, когда клиенты хотят расширить существующую инфраструктуру с помощью аналитики edge. Семейство ОС узла виртуальной машины должно совпадать с семейством гостевой ОС, используемой в контейнере модуля. Это требование является таким же, как при запуске непосредственно на устройстве Azure IoT Edge. Azure IoT Edge не зависит от используемой технологии виртуализации и работает на виртуальных машинах на базе таких платформ, как Hyper-V и vSphere.

<br>
<center>

![Azure IoT Edge на виртуальной Машине](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Минимальные системные требования
Azure IoT Edge отлично работает на различных устройствах, от Raspberry Pi3 до серверного оборудования. Выбор подходящее оборудование для вашего сценария зависит от рабочих нагрузок, которые вы хотите запустить. Принятие окончательного решения может быть затруднительно. Но можно легко запустить прототип решения на традиционных переносных или настольных компьютерах.

Эксперименты во время создания прототипов помогут вам сделать окончательный выбор устройства. Включить вопросы, которые следует учитывать: 

* Сколько модулей находятся в рабочей нагрузке?
* Сколько уровней нужно контейнеров к модулям общего ресурса?
* На каком языке написаны модули? 
* Какой объем данных модулей их обработки?
* Нужна ли модули любой специализированного оборудования для ускорения рабочих нагрузок?
* Каковы характеристики надлежащую производительность вашего решения?
* Что такое бюджета оборудования?
