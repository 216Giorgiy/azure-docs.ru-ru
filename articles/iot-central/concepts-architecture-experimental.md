---
title: Сведения об архитектуре Azure IoT Central | Документация Майкрософт
description: В этой статье описаны ключевые понятия, связанные с архитектурой Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 89da922b7a17f8a3b6a68a335cfeeffa5a8b236c
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2019
ms.locfileid: "55825234"
---
# <a name="azure-iot-central-architecture"></a>Архитектура Azure IoT Central

В этой статье приведено краткое описание архитектуры Microsoft Azure IoT Central.

![Архитектура верхнего уровня](media/concepts-architecture-experimental/architecture.png)

## <a name="devices"></a>Устройства

Устройства обмениваются данными с приложением Azure IoT Central. Устройство может:

- отправлять измерения (например, данные телеметрии);
- синхронизировать параметры с приложением.

В Azure IoT Central данные, которыми устройство может обмениваться с приложением, задаются в шаблоне устройства. Дополнительные сведения о шаблонах устройства см. в разделе об [управлении метаданными](#metadata-management).

Дополнительные сведения о подключении устройств к приложению Azure IoT Central см. в [этой](concepts-connectivity-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) статье.

## <a name="cloud-gateway"></a>Облачный шлюз

Azure IoT Central использует Центр Интернета вещей Azure как облачный шлюз, позволяющий подключаться к устройству. Центр Интернета вещей позволяет:

- выполнять прием данных в нужном масштабе в облаке;
- управлять устройствами;
- устанавливать надежное подключение.

Дополнительные сведения о Центре Интернета вещей Azure см. в [этой документации](https://docs.microsoft.com/azure/iot-hub/).

Дополнительные сведения о подключении устройств в Azure IoT Central см. в [этой статье](concepts-connectivity-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="data-stores"></a>Хранилища данных

Azure IoT Central хранит данные приложения в облаке. Хранимые данные приложения включают в себя:

- шаблоны устройств;
- удостоверения устройств;
- метаданные устройств;
- данные о пользователях и ролях.

Azure IoT Central использует хранилище временных рядов для данных измерений, отправляемых с устройств. Данные временных рядов, полученные с устройств, используются в службе аналитики.

## <a name="analytics"></a>Analytics

Служба аналитики отвечает за создание пользовательских передаваемых данных, отображаемых в приложении. Оператор может [настраивать аналитику](howto-create-analytics-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json), отображаемую в приложении. Служба аналитики создана на основе службы [Аналитика временных рядов Azure](https://azure.microsoft.com/services/time-series-insights/) и обрабатывает данные измерений, отправляемые с устройств.

## <a name="rules-and-actions"></a>Правила и действия

[Правила и действия](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) вместе позволяют автоматизировать задачи в приложении. Конструктор может определить правила на основе телеметрии устройства (например, если температура превышает указанное пороговое значение). Azure IoT Central использует обработчик потоков, чтобы определить выполнение условий правила. Когда условие правила выполнено, оно активирует действие, установленное конструктором. Например, может отправляться сообщение электронной почты, чтобы известить инженера, что температура устройства слишком высокая.

## <a name="metadata-management"></a>Управление метаданными

В приложении Azure IoT Central шаблоны устройств определяют поведение и возможности типов устройств. Например, шаблон холодильника указывает данные телеметрии, которые холодильник отправляет в приложение.

![Архитектура шаблона](media/concepts-architecture-experimental/template_architecture.png)

В шаблоне устройства:

- **Измерения** указывают данные телеметрии, которые устройство отправляет в приложение.
- **Параметры** указывают конфигурации, которые может задать оператор.
- **Свойства** указывают метаданные, которые может задать оператор.
- **Правила** автоматизируют поведение в приложении на основе данных, отправленных с устройства.
- **Панели мониторинга** представляют собой настраиваемые представления устройства в приложении.

Приложение может иметь одно или несколько имитированных и реальных устройств, созданных на основе каждого шаблона.

## <a name="role-based-access-control-rbac"></a>Управление доступом на основе ролей (RBAC)

[Администратор может определить правила доступа](howto-administer-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) для приложения Azure IoT Central, используя предварительно настроенные роли. Он также может назначить пользователям роли, которые определяют, к каким областям приложения пользователь имеет доступ.

## <a name="security"></a>Безопасность

Возможности безопасности в пределах Azure IoT Central:

- Данные шифруются при передаче и во время хранения.
- Проверка подлинности осуществляется с помощью Azure Active Directory или учетной записи Майкрософт. Поддерживается двухфакторная проверка подлинности.
- Полная изоляция клиентов.
- Безопасность на уровне устройств.

## <a name="ui-shell"></a>Оболочка пользовательского интерфейса

Оболочка пользовательского интерфейса — это современное приложение HTML5 на основе браузера, которое быстро реагирует на запросы.

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда вы изучили архитектуру Azure IoT Central, рекомендуется узнать о [подключении устройств](concepts-connectivity-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) в Azure IoT Central.