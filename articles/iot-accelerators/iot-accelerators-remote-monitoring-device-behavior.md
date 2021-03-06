---
title: Поведение имитированного устройства в решении для удаленного мониторинга в Azure | Документация Майкрософт
description: В этой статье описывается, как определить поведение имитированного устройства в решении для удаленного мониторинга с помощью JavaScript.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: c2151a4b1eb2a853ed343f6720b4f53af5e5e449
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449526"
---
# <a name="implement-the-device-model-behavior"></a>Реализация поведения модели устройства

В статье [Общие сведения о схеме модели устройства](iot-accelerators-remote-monitoring-device-schema.md) описана схема, определяющая модель имитированного устройства. В ней описаны два типа файлов JavaScript, реализующих поведение имитированного устройства:

- Файлы JavaScript **состояния**, которые запускаются через фиксированные промежутки времени для обновления внутреннего состояния устройства.
- Файлы JavaScript **метода**, которые запускаются, когда решение вызывает метод на устройстве.

> [!NOTE]
> Поведение моделей устройств предназначено только для имитированных устройств, размещенных в службе моделирования устройств. Если вам требуется создать реальное устройство, см. статью [Подключение устройства к акселератору решения для удаленного мониторинга (Node.js)](iot-accelerators-connecting-devices.md).

В этой статье раскрываются следующие темы:

>[!div class="checklist"]
> * управление состоянием имитированного устройства;
> * Определение ответа имитированного устройства на вызов метода из решения для удаленного мониторинга
> * отладка сценариев.

[!INCLUDE [iot-accelerators-device-behavior](../../includes/iot-accelerators-device-behavior.md)]

## <a name="next-steps"></a>Дополнительная информация

В этой статье описывается, как определить поведение собственной настраиваемой модели имитированного устройства. Из этой статьи вы узнали, как выполнять следующие задачи:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * управление состоянием имитированного устройства;
> * Определение ответа имитированного устройства на вызов метода из решения для удаленного мониторинга
> * отладка сценариев.

Теперь, когда вы узнали, как определить поведение имитированного устройства, предлагаем ознакомиться со статьей [Создание имитированного устройства](iot-accelerators-remote-monitoring-create-simulated-device.md).

Дополнительные сведения для разработчиков о решении для удаленного мониторинга см. в следующих источниках:

* [Справочник разработчика](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Руководство по устранению неполадок для разработчиков](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
