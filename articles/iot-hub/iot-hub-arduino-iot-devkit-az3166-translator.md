---
title: Создание переводчика IoT DevKit при помощи Функций Azure и Cognitive Services | Документация Майкрософт
description: Использование микрофона в IoT DevKit для получения голосового сообщения и его преобразование в текст на английском языке при помощи Azure Cognitive Services
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: cd67e612dd020ba600e33ac8baf77bc094d8afd3
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2018
ms.locfileid: "42142739"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>Использование IoT DevKit AZ3166 с Функциями Azure и Cognitive Services для создания переводчика

Из этой статьи вы узнаете, как создать переводчик на основе IoT DevKit с использованием [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/). Это средство записывает голосовое сообщение, которое преобразуется в текст на английском языке, отображающийся на экране DevKit.

[MXChip IoT DevKit](https://aka.ms/iot-devkit) — это универсальная совместимая с Arduino плата со множеством периферийных устройств и датчиков. Вы можете выполнить для нее разработку с помощью [расширения Visual Studio Code для Arduino](https://aka.ms/arduino). Она предоставляется с расширяющимся [каталогом проектов](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), чтобы помочь вам выполнить прототипирование решений Интернета вещей, использующих службы Microsoft Azure.

## <a name="what-you-need"></a>Необходимые элементы

Выполните указания в [руководстве по началу работы](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), чтобы перейти к таким этапам:

* Подключение DevKit к сети Wi-Fi.
* Подготовка среды разработки

Активная подписка Azure. Если у вас еще нет ее, вы можете зарегистрироваться одним из двух способов:

* Вы можете активировать [бесплатную 30-дневную пробную учетную запись Microsoft Azure](https://azure.microsoft.com/free/).
* Запросите [деньги на счете в Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), если у вас есть подписка MSDN или Visual Studio.

## <a name="open-the-project-folder"></a>Открытие папки проекта

Сперва откройте папку проекта. 

### <a name="start-vs-code"></a>Запуск VS Code

- Убедитесь, что плата DevKit подключена к компьютеру.

- Запустите VSCode.

- Подключите DevKit на компьютере.

### <a name="open-the-arduino-examples-folder"></a>Открытие папки с примерами Arduino

Справа последовательно выберите **Arduino Examples (Примеры Arduino) > Examples for MXCHIP AZ3166 (Примеры для MXCHIP AZ3166) > AzureIoT** и щелкните **DevKitTranslator**. Откроется новое окно VS Code с папкой проекта. Если не видите раздел с примерами для MXCHIP AZ3166, проверьте правильность подключения устройства и перезапустите VS Code.  

![Примеры IoT DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

Пример также можно открыть из палитры команд. Нажмите `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) для вызова палитры команд. Введите **Arduino**, затем найдите и выберите **Arduino: Examples** (Arduino: примеры).

## <a name="provision-azure-services"></a>Подготовка служб Azure

В окне решений введите `Ctrl+P` (macOS: `Cmd+P`), а затем — `task cloud-provision`.

В терминале VS Code с помощью указаний в интерактивной командной строке подготовьте все необходимые службы Azure:

![Задача подготовки облака](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="deploy-the-azure-function"></a>Развертывание экземпляра Функции Azure

Чтобы запустить `task cloud-deploy` для развертывания кода решения "Функции Azure", используйте `Ctrl+P` (macOS: `Cmd+P`). Обычно этот процесс занимает от 2 до 5 минут.

![Задача развертывания облака](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

После успешного развертывания экземпляра Функции Azure укажите имя приложения-функции в файле azure_config.h. Имя можно найти на [портале Azure](https://portal.azure.com/):

![Расположение имени приложения-функция Azure](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Если экземпляр Функции Azure не работает должным образом, перейдите в раздел [об ошибке компиляции решения "Функции Azure" в часто задаваемых вопросах об IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) для ее устранения.

## <a name="build-and-upload-the-device-code"></a>Создание и передача кода устройства

1. Запустите `task config-device-connection` при помощи `Ctrl+P` (macOS: `Cmd+P`).

2. В терминале отобразится запрос на использование строки подключения, полученной на шаге выполнения `task cloud-provision`. Также вы можете ввести в строке подключения устройства другую необходимую команду, выбрав для этого **Create New...** (Создать).

3. Терминал предложит перейти в режим настройки. Чтобы сделать это, удерживая нажатой кнопку "A", нажмите и отпустите кнопку "Reset" (Сброс). На экране появится идентификатор DevKit и надпись "Configuration" (Настройка).

   ![Проверка и передача эскиза Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. После выполнения `task config-device-connection` нажмите клавишу `F1`, чтобы загрузить команды VS Code, и выберите `Arduino: Upload`. VS Code начнет проверку и отправку эскиза Arduino.

   ![Проверка и передача эскиза Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

Плата DevKit перезагрузится и начнет выполнение кода.

## <a name="test-the-project"></a>Тестирование проекта

После инициализации приложения следуйте инструкциям на экране DevKit. По умолчанию исходный язык — китайский.

Чтобы выбрать другой язык для перевода:

1. Нажмите кнопку A, что перейти в режим настройки.

2. Нажмите кнопку B, чтобы прокрутить все поддерживаемые исходные языки.

3. Нажмите кнопку A, чтобы подтвердить выбор исходного языка.

4. Нажмите и удерживайте кнопку B при произнесении фразы, затем отпустите кнопку B, чтобы начать перевод.

5. На экране появится текст, переведенный на английский язык.

![Прокрутка для выбора языка](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Результат перевода](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

На экране результатов перевода можно выполнять следующие действия:

- Нажмите кнопки A и B, чтобы прокрутить список исходных языков и выбрать нужный.

- Нажмите кнопку B при произнесении фразы. Чтобы отправить данные голосового ввода и получить переведенный текст, отпустите кнопку B.

## <a name="how-it-works"></a>Принцип работы

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

В эскизе Arduino записывается ваш голос. Затем отправляется HTTP-запрос для активации экземпляра Функции Azure. В экземпляре Функции Azure для перевода вызывается API переводчика речи Cognitive Services. Когда в экземпляр Функции Azure поступает текст перевода, на устройство отправляется сообщение C2D (из облака). После этого перевод отображается на экране.

## <a name="problems-and-feedback"></a>Проблемы и обратная связь

Если вы столкнулись с проблемами, ознакомьтесь с [вопросами и ответами](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) об IoT DevKit или используйте один из следующих каналов связи:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Дальнейшие действия

Теперь вы узнали, как использовать IoT DevKit для перевода при помощи Функций Azure и Cognitive Services. В этой статье вы узнали, как:

> [!div class="checklist"]
> * использовать Visual Studio Code, чтобы автоматизировать подготовку облака;
> * настроить строку подключения для устройства Интернета вещей Azure;
> * Развертывание экземпляра Функции Azure
> * протестировать перевод голосовых сообщений.

Перейдите к следующему руководству:

> [!div class="nextstepaction"]
> [Подключение IoT DevKit AZ3166 к акселератору решений для удаленного мониторинга Интернета вещей Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
