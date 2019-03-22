---
title: Подключение универсального клиентского приложения Node.js к Azure IoT Central | Документация Майкрософт
description: Вы узнаете, как разработчик устройства может подключать универсальное устройство Node.js к приложению Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 02/04/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 4d2701f078a26c22f52aebd0ef562dd60eaca923
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58097980"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Подключение универсального клиентского приложения к приложению Azure IoT Central (Node.js)

В этой статье описано, каким образом разработчик устройства может подключить универсальное устройство Node.js, представляющее собой реальное устройство, к приложению Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Перед началом работы

Чтобы выполнить действия, описанные в этой статье, необходимо следующее:

1. Приложение Azure IoT Central. Дополнительные сведения см. в [кратком руководстве по созданию приложения](quick-deploy-iot-central.md).
1. Компьютер для разработки с установленным [Node.js](https://nodejs.org/) 4.0.0 или более поздней версии. Вы можете запустить `node --version` в командной строке, чтобы проверить версию. Node.js доступен для разных операционных систем.

## <a name="create-a-device-template"></a>Создание шаблона устройства

В приложении Azure IoT Central требуется шаблон устройства со следующими определенными измерениями и свойствами устройства.

### <a name="telemetry-measurements"></a>Измерения телеметрии

Добавьте следующие данные телеметрии на странице **Measurements** (Измерения).

| Отображаемое имя | Имя поля  | Units | Min | Max | Число десятичных знаков |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| температура;  | Температура | F     | 60  | 110 | 0              |
| влажность.     | Влажность    | %     | 0   | 100 | 0              |
| Давление     | pressure    | кПа   | 80  | 110 | 0              |

> [!NOTE]
>   Данные телеметрии передаются в формате чисел с плавающей запятой.

Введите имена полей из таблицы в шаблон устройства. Если имена полей не совпадают с именами свойств, указанными в коде для соответствующего устройства, данные телеметрии в приложении не отображаются.

### <a name="state-measurements"></a>Измерения состояния

Добавьте следующие данные состояния на странице **Measurements** (Измерения).

| Отображаемое имя | Имя поля  | Значение 1 | Отображаемое имя | Значение 2 | Отображаемое имя |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Режим вентилятора     | fanmode     | 1       | Выполнение      | 0       | Остановлено      |

> [!NOTE]
>   Для измерения состояния используется строковый тип данных.

Введите имена полей из таблицы в шаблон устройства. Если имена полей не совпадают с именами свойств, указанными в коде для соответствующего устройства, данные состояния в приложении не отображаются.

### <a name="event-measurements"></a>Измерения событий

Добавьте следующие данные событий на странице **Measurements** (Измерения).

| Отображаемое имя | Имя поля  | Уровень серьезности |
| ------------ | ----------- | -------- |
| Перегрев  | overheat    | Ошибка    |

> [!NOTE]
>   Для измерения событий используется строковый тип данных.

### <a name="device-properties"></a>Свойства устройства

Добавьте следующие свойства устройства на **странице свойств**.

| Отображаемое имя        | Имя поля        | Тип данных |
| ------------------- | ----------------- | --------- |
| Серийный номер       | SerialNumber      | текст      |
| Производитель устройства | manufacturer      | текст      |

Введите имена полей из таблицы в шаблон устройства. Если имена полей не совпадают с именами свойств, указанными в коде для соответствующего устройства, приложение не сможет отображать значения свойств устройства.

### <a name="settings"></a>Параметры

Добавьте следующие **числовые** параметры на **странице параметров**.

| Отображаемое имя    | Имя поля     | Units | Десятичные знаки | Min | Max  | Initial |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Скорость вращения вентилятора       | fanSpeed       | Об/мин   | 0        | 0   | 3000 | 0       |
| Установить температуру | setTemperature | F     | 0        | 20  | 200  | 80      |

Введите имена полей из таблицы в шаблон устройства. Если имена полей не совпадают с именами свойств, указанными в коде для соответствующего устройства, устройство не сможет получать значения свойств.

## <a name="add-a-real-device"></a>Добавление реального устройства

В приложении Azure IoT Central добавьте реальное устройство из созданного шаблона приложения и запишите строку подключения к устройству. Пошаговые инструкции по подключению приложения Node.js к IoT Central см. в разделах [Создание строки подключения для реального устройства из приложения](tutorial-add-device.md#generate-connection-string) и [Подготовка клиентского кода](tutorial-add-device.md#prepare-the-client-code) руководства по добавлению устройства.

### <a name="create-a-nodejs-application"></a>Создание приложения Node.js

Ниже показано, как создать клиентское приложение, реализующее реальное устройство, добавленное в приложение. Здесь приложение Node.js представляет реальное устройство. 

1. Создайте на компьютере папку с именем `connected-air-conditioner-adv`. Перейдите к этой папке в среде командной строки.

1. Чтобы инициализировать проект Node.js, выполните следующие команды:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Создайте файл **ConnectedAirConditioner.js** в папке `connected-air-conditioner-adv`.

1. Добавьте следующие инструкции `require` в начало файла **connectedAirConditionerAdv.js**:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Добавьте следующие объявления переменных в файл:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    > [!NOTE]
    > Azure IoT Central перешел с помощью службы подготовки устройств Azure IoT Hub (DPS) для всех подключений устройств, выполните следующие инструкции, чтобы [Получение строки подключения устройства](concepts-connectivity.md#get-a-connection-string) и продолжите выполнение оставшейся части руководства. Подробные инструкции см. в разделе [Подготовка клиентского кода](tutorial-add-device.md#prepare-the-client-code) руководства по добавлению устройств.

    Замените заполнитель `{your device connection string}` строкой подключения к устройству. В этом примере мы инициализируем `targetTemperature` до нуля. При необходимости можно использовать текущие данные с устройства или значение из двойника устройства. 

1. Для отправки данных телеметрии, состояния и событий в приложение Azure IoT Central необходимо добавить в файл следующую функцию:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({ 
        temperature: temperature, 
        humidity: humidity, 
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

1. Для отправки свойств устройства в приложение Azure IoT Central необходимо добавить в файл следующую функцию:

    ```javascript
    // Send device properties.
    function sendDeviceProperties(twin) {
      var properties = {
        serialNumber: '123-ABC',
        manufacturer: 'Contoso'
      };
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Чтобы определить параметры, на которые реагирует устройство, добавьте следующее определение:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Чтобы обработать обновленные параметры в приложении Azure IoT Central, добавьте следующий файл:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Добавьте следующее, чтобы установить подключение к Azure IoT Central и подключить функции в клиентском коде:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Запуск приложения Node.js

В среде командной строки выполните следующую команду:

```cmd/sh
node connectedAirConditionerAdv.js
```

Будучи оператором в приложении Azure IoT Central для реального устройства вы можете делать следующее:

* Просматривать данные телеметрии на странице **Measurements** (Измерения).

    ![Просмотр телеметрии](media/howto-connect-nodejs/viewtelemetry.png)

* Просматривать значения свойств устройства, отправленные из устройства, на странице **Свойства**. Обновление плитки устройства-свойства при успешном выполнении подключения.

    ![Просмотр свойств устройства](media/howto-connect-nodejs/viewproperties.png)

* Задавать скорость вращения вентилятора и целевую температуру на странице **Параметры**. Значения параметров синхронизации, если подключение установлено успешно.

    ![Установка скорости вращения вентилятора](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Дальнейшие действия

Вы узнали, как подключить универсальное клиентское приложение Node.js к Azure IoT Central, а значит вы готовы к следующему шагу.
* [Подготовка и подключение Raspberry Pi](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->
