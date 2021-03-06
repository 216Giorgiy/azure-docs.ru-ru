---
title: Создание и запуск заданий в приложении Azure IoT Central | Документация Майкрософт
description: Задания Azure IoT Central позволяют использовать массовые функции управления устройствами, такие как обновление свойства устройства, настройка или выполнение команды.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/18/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: ec7033719316bb186408ea78f6dabac43c383491
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58199857"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Создание и запуск заданий в приложении Azure IoT Central

Вы можете использовать Microsoft Azure IoT Central для управления подключенными устройствами в масштабе с использованием заданий. Задания позволяют проведения массовых обновлений свойств устройства, параметры и команды. В этой статье рассматривается как приступить к работе с помощью заданий в собственном приложении.

## <a name="create-and-run-a-job"></a>Создание и запуск заданий

В этом разделе показано, как создать и запустить задание. Оно показано, как увеличить скорость вращения для нескольких refrigerated торговые автоматы.

1. В области навигации перейдите к заданиям.

1. Выберите **+ создать** для создания нового задания.

    ![Создание задания](./media/howto-run-a-job/createnewjob.png)

1. Введите имя и описание для идентификации задания, которое вы создаете.

1. Выберите задания для применения к набор устройств. После выбора устройства, см. справа заполнения с устройствами в наборе устройств. Если выбрать набор неработающих устройств, устройства не отображать, и появится сообщение, что разрыве вашего набора устройств.

1. Затем выберите тип задания для определения (параметр, свойства или команду). Выберите **+** рядом с типом задания выбран и добавьте операции.

    ![Настройка задания](./media/howto-run-a-job/configurejob.png)

1. В области справа выберите устройства, вы хотите запустить задание. Если выбрать верхний флажок, все устройства выбраны в наборе данных всего устройства. Установив флажок рядом с **имя**, выбираются все устройства на текущей странице.

1. После выбора устройства, выберите **запуска** или **Сохранить**. Задание появится в основной **заданий** страницы. В этом представлении можно увидеть в настоящее время выполнение задания и журнал любого ранее выполнения заданий. Выполнение задания всегда отображается в верхней части списка. Сохраненные задания можно открыть в любое время продолжить редактирование или запустить.

    ![Просмотр задания](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > В течение 30 дней, можно просматривать журнал выполненных заданий.

1. Чтобы получить общие сведения о задании, выберите задание для просмотра из списка. Данный обзор содержит сведения о задании, устройств и значения состояния устройства. В этом обзоре, также можно выбрать **загрузить сведения о задании** для загрузки CSV-файл из сведениях о задании, включая устройства и их состояние. Эти сведения можно использовать для устранения неполадок.

    ![Просмотр состояния устройства](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Остановка выполнения задания

Чтобы остановить выполняющееся задание, выберите его и щелкните **остановить** на панели. Состояние задания изменится в соответствии с задания будет остановлено.

   ![Остановка задания](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Запуск остановленного задания

Чтобы запустить задание, которое в данный момент остановлена, выберите задание, остановлена. Выберите **запуска** на панели. Состояние задания изменится в соответствии с задание выполняется повторно.

   ![Возобновление выполнения задания](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Задание копирования

Чтобы скопировать существующее задание, вы создали, выберите его на странице основных задания и выберите **копирования**. Новая копия конфигурации задания открывается для редактирования. Можно сохранить или запустить новое задание. Если были внесены изменения в набор выбранного устройства, они все отражаются в этого скопированные задания, которые можно изменить.

   ![задание копирования](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Просмотр состояния задания

После создания задания **состояние** обновления столбцов с последней сообщение о состоянии задания. В следующей таблице перечислены возможные значения состояния:

| Сообщение о состоянии       | Значение состояния                                          |
| -------------------- | ------------------------------------------------------- |
| Завершено            | Это задание было выполнено на всех устройствах.              |
| Сбой               | Это задание завершилось сбоем и выполнено не полностью на устройствах.  |
| Ожидает              | Это задание еще не начала выполняться на устройствах.         |
| Выполнение              | Это задание в данный момент выполняется на устройствах.             |
| Остановлено              | Это задание вручную остановлено пользователем.           |

Сообщение о состоянии следует обзор устройств в задании. В следующей таблице перечислены возможные устройства значения состояния:

| Сообщение о состоянии       | Значение состояния                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Succeeded            | Количество устройств, которые задания выполнены успешно на.       |
| Сбой               | Количество устройств, которые не удалось выполнить задание.       |

### <a name="view-the-device-status"></a>Просмотр состояния устройства

Чтобы просмотреть состояние задания и всех соответствующих устройств, выберите задание. Чтобы скачать CSV-файла, который включает в себя сведения о задании, включая список устройств и их состояние, выберите **загрузить сведения о задании**. Рядом с именем каждого устройства вы видите одну из следующих сообщений о состоянии:

| Сообщение о состоянии       | Значение состояния                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Завершено            | Это задание было выполнено на этом устройстве.                                     |
| Сбой               | Это задание завершилось сбоем на этом устройстве. Сообщение об ошибке отображаются дополнительные сведения.  |
| Ожидает              | Задание еще не выполнена на этом устройстве.                                   |

> [!NOTE]
> Если устройства был удален, нельзя выбрать устройство, и отображаются как удаленные с идентификатором устройства.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как создавать задания в приложении Azure IoT Central, ниже приведены некоторые дальнейшие действия.

- [использовать наборы устройств](howto-use-device-sets.md);
- [Управление устройствами](howto-manage-devices.md)
- [Создание версии шаблона нового устройства](howto-version-devicetemplate.md)
