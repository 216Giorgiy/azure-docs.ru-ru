---
title: 'IoT DevKit в облаке: подключение платы IoT DevKit AZ3166 к акселератору решений Интернета вещей для удаленного мониторинга | Документация Майкрософт'
description: В этом руководстве вы узнаете, как отправлять данные о состоянии датчиков IoT DevKit AZ3166 в акселератор решений Интернета вещей для удаленного мониторинга и визуализации.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: e900b952ab9bb2054b9e4174670894027cdd2618
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969458"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>Подключение MXChip IoT DevKit AZ3166 к акселератору решений для удаленного мониторинга Интернета вещей


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

В этом руководстве вы узнаете, как запустить пример приложения с помощью своего набора разработки, чтобы обеспечить отправку данных датчиков в акселератор решений.

[MXChip IoT DevKit](https://aka.ms/iot-devkit) — это универсальная совместимая с Arduino плата со множеством периферийных устройств и датчиков. Вы можете выполнить для нее разработку с помощью [расширения Visual Studio Code для Arduino](https://aka.ms/arduino). Она предоставляется с расширяющимся [каталогом проектом](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), чтобы помочь вам выполнить прототипирование решений Интернета вещей, использующих службы Microsoft Azure.

## <a name="what-you-need"></a>Необходимые элементы

Выполните указания в [руководстве по началу работы](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), чтобы перейти к таким этапам:

* Подключение DevKit к сети Wi-Fi.
* Подготовка среды разработки


## <a name="open-the-remotemonitoring-sample"></a>Открытие примера RemoteMonitoring

1. Отключите DevKit на компьютере, если набор подключен.

2. Запустите VSCode.

3. Подключите DevKit на компьютере. VSCode автоматически обнаружит ваш DevKit и откроет следующие страницы:
  * Страница введения в набор разработки.
  * Примеры Arduino: практические примеры по началу работы с набором разработки.

4. Разверните раздел **ARDUINO EXAMPLES** (Примеры Arduino) слева, перейдите в папку **Examples for MXCHIP AZ3166 (Примеры для MXCHIP AZ3166) > AzureIoT** и щелкните **RemoteMonitoringv2**. Откроется новое окно VSCode с папкой проекта.

  ![Открытие проекта удаленного мониторинга](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Если вы закрыли эту область, вы можете открыть ее снова. Нажмите `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) для вызова палитры команд. Введите **Arduino**, затем найдите и выберите **Arduino: Examples** (Arduino: примеры).

## <a name="add-a-new-physical-device"></a>Добавление нового физического устройства

На портале перейдите в раздел **Устройства** и нажмите кнопку **+New Device** (+Новое устройство). 

![Добавление нового устройства](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

Заполните *форму нового устройства*.
1. В разделе *Тип устройства* выберите **Физический**.
2. Определите идентификатор устройства (например, *MXChip* или *AZ3166*).
3. В разделе *Ключ проверки подлинности* выберите **Автоматически создавать ключи**.
4. Нажмите кнопку *Применить*.

![Добавление формы нового устройства](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Подождите, пока портал закончит подготовку нового устройства.

![Подготовка нового устройства ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


После этого отобразится конфигурация нового устройства.
Скопируйте созданную **строку подключения**.

![Строка подключения к устройству](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Эта строка подключения будет использоваться в следующем разделе.





## <a name="build-and-upload-the-device-code"></a>Создание и передача кода устройства

Вернитесь к Visual Studio Code: 

1. Нажмите клавиши `Ctrl+P` (в macOS — `Cmd + P`) и введите **task config-device-connection**.

  ![выбор подписки Azure и Центра Интернета вещей](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. В окне терминала отобразится запрос на использование строки подключения устройства Центра Интернета вещей. Выберите *Создать* и вставьте скопированную строку.

  ![вставка строки подключения](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. Иногда терминал предлагает перейти в режим настройки. Для этого, удерживая клавишу A, нажмите и отпустите кнопку "Сброс", после чего отпустите клавишу A. На экране появится идентификатор DevKit и надпись "Настройка".

  ![Экран устройства DevKit](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > Если вы выполнили указания последнего раздела этого руководства, строка подключения должна сохраниться в вашем буфере обмена. В противном случае перейдите на портал Azure и найдите Центр Интернета вещей группы ресурсов решения "Удаленный мониторинг". Там находятся подключенные устройства Центра Интернета вещей. Скопируйте строку подключения устройства.

  ![поиск строки подключения](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Теперь новое физическое устройство будет отображаться в разделе VS Code Azure IoT Hub Devices (Устройства Центра Интернета вещей Azure).

![Новое устройство Центра Интернета вещей](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Тестирование проекта

При запуске примера приложения DevKit отправляет данные датчиков по сети Wi-Fi в акселераторы решений Интернета вещей. Чтобы просмотреть результат, сделайте следующее:

1. Перейдите в акселератор решений Интернета вещей и щелкните **Панель мониторинга**.

2. В консоли акселератора решений Интернета вещей будет отображаться состояние датчиков DevKit. 

![Данные датчиков в акселераторах решений Интернета вещей](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Если щелкнуть имя датчика (AZ3166), справа от панели мониторинга откроется вкладка, на которой можно просмотреть диаграмму датчиков MX Chip в режиме реального времени.


## <a name="send-a-c2d-message"></a>Отправка сообщения C2D
Решение "Удаленный мониторинг" версии 2 позволяет использовать удаленный метод на устройстве.
В примере кода микросхемы MX содержатся три метода, которые можно увидеть в разделе "Метод", выбрав датчик.

![Методы микросхемы MX](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

Вы можете изменить цвет индикаторов микросхемы MX с помощью метода LedColor. Для этого установите флажок устройства и нажмите кнопку "Расписание". 

![Методы микросхемы MX](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Выберите в раскрывающемся списке метод ChangeColor, укажите имя и щелкните "Применить".

![Раскрывающийся список микросхем MX](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Через несколько секунд в физической микросхеме MX изменится цвет индикатора RGB под клавишей А.

![Индикатор микросхемы MX](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)


## <a name="change-device-id"></a>Изменение идентификатора устройства

Идентификатор устройства в Центре Интернета вещей можно изменить, выполнив шаги в [этом руководстве](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).


## <a name="problems-and-feedback"></a>Проблемы и обратная связь

Если вы столкнулись с проблемами, то можете ознакомиться с [часто задаваемыми вопросами](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) или обратиться к нам по одному из таких каналов:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда вы знаете, как подключить устройство DevKit к акселератору решений Интернета вещей и визуализировать данные датчиков, можно выполнить:

* [Общие сведения об акселераторах решений Интернета вещей](https://docs.microsoft.com/azure/iot-suite/)
* [Connect an MXChip IoT DevKit device to your Microsoft IoT Central application](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit) (Подключение устройства MXChip IoT DevKit к приложению Microsoft IoT Central)
