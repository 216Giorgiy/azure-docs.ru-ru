---
title: Мониторинг подключения устройств с помощью обозревателя Azure IoT Central
description: Отслеживайте сообщения с устройств и изменения двойников устройств с помощью интерфейса командной строки обозревателя IoT Central.
author: viv-liu
ms.author: viviali
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e510cfbd89ab8dcd8dccd9a8b95a49a057c9b54f
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2019
ms.locfileid: "55825131"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Мониторинг подключения устройств с помощью обозревателя Azure IoT Central

*Этот раздел предназначен для разработчиков и администраторов.*

С помощью интерфейса командной строки (CLI) обозревателя IoT Central можно просматривать сообщения, которые устройства отправляют в IoT Central, и следить за изменениями двойников в центре Интернета вещей. Это средство с открытым кодом позволяет получать более полную информацию о состоянии подключения устройств и диагностировать проблемы с доставкой сообщений от устройств в облако или реакцией устройств на изменение двойников.

## <a name="visit-the-iotc-explorer-repo-in-githubhttpsakamsiotciotcexplorercligithub"></a>[Перейти в репозиторий iotc-explorer в GitHub](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Предварительные требования
+ Node.js версии 8.x или более поздней — https://nodejs.org
+ Вам потребуется попросить администратора приложения создать маркер доступа, который будет использоваться в iotc-explorer

## <a name="installing-iotc-explorer"></a>Установка iotc-explorer

Чтобы установить средство, в командной строке выполните следующую команду:

```
npm install -g iotc-explorer
```

> [!NOTE]
> В Unix-подобных средах команду установки, как правило, необходимо выполнять с помощью `sudo`.

## <a name="running-iotc-explorer"></a>Запуск iotc-explorer

Ниже представлены некоторые часто используемые команды и параметры `iotc-explorer`. Чтобы просмотреть полный список команд и параметров, передайте `--help` в `iotc-explorer` или одну из подкоманд.

### <a name="login"></a>Вход

Прежде чем приступать к работе, необходимо попросить администратора приложения IoT Central создать для вас маркер доступа. Администратору потребуется выполнить указанные ниже действия.
1. Перейдите в раздел **Администрирование/Маркеры доступа**. 
1. Щелкните **Generate** (Создать).
    ![Снимок экрана страницы "Маркеры доступа"](media/howto-use-iotc-explorer-experimental/accesstokenspage.png)

1. Введите имя маркера, нажмите кнопку **Далее** и **скопируйте значение маркера**.
    > [!NOTE]
    > Значение маркера будет показано только один раз, поэтому его необходимо скопировать перед закрытием диалогового окна. После того как диалоговое окно закроется, узнать его будет невозможно.

    ![Снимок экрана диалогового окна, в котором копируется маркер доступа](media/howto-use-iotc-explorer-experimental/copyaccesstoken.png)

Затем этот маркер можно использовать для входа в CLI с помощью команды:

```sh
iotc-explorer login "<Token value>"
```

Если вы не хотите сохранять маркер в журнале оболочки, его можно указывать каждый раз при запросе:

```
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Отслеживание сообщений с устройств

Вы можете отслеживать сообщения, поступающие с конкретного устройства или со всех устройств, где используется приложение, с помощью команды `monitor-messages`. В результате запускается монитор, который будет непрерывно выводить новые сообщения по мере их поступления.

Чтобы непрерывно отслеживать все устройства в приложении, выполните следующую команду:

```
iotc-explorer monitor-messages
```

Выходные данные:

![выходные данные команды monitor-messages](media/howto-use-iotc-explorer-experimental/monitormessages.png)

Чтобы отслеживать определенное устройство, просто добавьте в конце команды идентификатор устройства:

```
iotc-explorer monitor-messages <your-device-id>
```

Вы также можете придать выходным данным машинный формат, добавив к команде параметр `--raw`:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Получение двойника устройства

С помощью команды `get-twin` можно получить содержимое двойника для устройства IoT Central. Для этого выполните следующую команду:

```
iotc-explorer get-twin <your-device-id>
```

Выходные данные:

![выходные данные команды get-twin](media/howto-use-iotc-explorer-experimental/getdevicetwin.png)

Так же как и в случае с командой `monitor-messages`, вы можете получить данные в машинном формате, передав параметр `--raw`:

```
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Дополнительная информация
Теперь, когда вы научились использовать обозреватель IoT Central, предлагаем ознакомиться с [управляющими устройствами IoT Central](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
