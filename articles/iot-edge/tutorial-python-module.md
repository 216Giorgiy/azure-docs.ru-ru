---
title: Руководство по созданию пользовательского модуля Python — Azure IoT Edge | Документация Майкрософт
description: В этом руководстве показано, как создать модуль IoT Edge с кодом Python и его развертывание на пограничном устройстве.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 11/25/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: a8edf8d67c55cad856eacf883a6449606e594887
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343774"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Руководство. Разработка модуля IoT Edge с кодом Python и его развертывание на имитированном устройстве

Вы можете использовать модули Azure IoT Edge для развертывания кода, который реализует бизнес-логику непосредственно на устройствах IoT Edge. В этом руководстве рассматриваются создание и развертывание модуля IoT Edge, который фильтрует данные датчика. Вы используете имитированное устройство IoT Edge, созданное при изучении кратких руководств. Из этого руководства вы узнаете, как выполнять следующие задачи:    

> [!div class="checklist"]
> * создание модуля Python для IoT Edge с помощью Visual Studio Code;
> * использование Visual Studio Code и Docker для создания образа Docker и его публикация в реестре;
> * Развертывать модуль на устройстве IoT Edge.
> * Просматривать сформированные данные.


Модуль IoT Edge, создаваемый в этом руководстве, фильтрует данные температуры, которые создаются устройством. Оно отправляет сообщения, только если температура превышает заданное пороговое значение. Такой тип пограничного анализа удобен для сокращения объема данных, передаваемых в облако и сохраняемых в нем. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Предварительные требования

Устройство Azure IoT Edge.

* В качестве устройства Azure IoT Edge можно использовать компьютер, на котором ведется разработка, или виртуальную машину. Для этого выполните действия, описанные в кратком руководстве для устройства [Linux](quickstart-linux.md).
* Модули Python для IoT Edge не поддерживают процессоры устройства Windows.

Облачные ресурсы.

* [Центр Интернета вещей](../iot-hub/iot-hub-create-through-portal.md) ценовой категории "Бесплатный" или "Стандартный" в Azure. 

Ресурсы разработки.

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Расширение Azure IoT Edge для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
* [Расширение Visual Studio Code для Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Python](https://www.python.org/downloads/).
* [PIP](https://pip.pypa.io/en/stable/installing/#installation) для установки пакетов Python (обычно входит в состав установки Python).

>[!Note]
>Убедитесь, что папка `bin` расположена в соответствии с требованиями вашей платформы. Обычно это `~/.local/` для UNIX и macOS или `%APPDATA%\Python` для Windows.

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

В этом руководстве описано, как создать модуль с помощью расширения Azure IoT Edge для Visual Studio Code и **образ контейнера** из файлов. Затем вы отправите этот образ в **реестр**, содержащий ваши образы и управляющий ими. Наконец, вы развернете свой образ из реестра для выполнения на устройстве IoT Edge.  

Для хранения образов контейнеров можно использовать любые реестры, совместимые с Docker. Две популярные службы реестров Docker — [Реестр контейнеров Azure](https://docs.microsoft.com/azure/container-registry/) и [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). В этом руководстве используется реестр контейнеров Azure. 

Если вы еще не создали реестр контейнеров, выполните эти действия, чтобы создать реестр в Azure:

1. На [портале Azure](https://portal.azure.com) выберите **Создать ресурс** > **Контейнеры** > **Реестр контейнеров**.

2. Предоставьте следующие значения для создания реестра контейнеров:

   | Поле | Значение | 
   | ----- | ----- |
   | Имя реестра | Укажите уникальное имя. |
   | Подписка | Из раскрывающегося списка выберите подписку. |
   | Группа ресурсов | Мы рекомендуем использовать одну группу ресурсов для всех тестовых ресурсов, которые вы создаете во время работы с краткими руководствами по IoT Edge. Например, **IoTEdgeResources**. |
   | Расположение | Выберите расположение рядом с вами. |
   | Пользователь-администратор | Установите значение **Включить**. |
   | SKU | Выберите **Базовый**. | 

5. Нажмите кнопку **Создать**.

6. После создания реестра контейнеров перейдите к нему, а затем выберите **Ключи доступа**. 

7. Скопируйте значения **Сервер входа**, **Имя пользователя** и **Пароль**. Они потребуются позже в этом руководстве для предоставления доступа к реестру контейнеров. 

## <a name="create-an-iot-edge-module-project"></a>Создание проекта модуля IoT Edge
Описанный ниже процесс позволяет создать модуль Python для IoT Edge с использованием Visual Studio Code и расширения Azure IoT Edge.

### <a name="create-a-new-solution"></a>Создание решения

С помощью пакета Python **cookiecutter** создайте шаблон решений Python, на основе которого можно выполнить сборку. 

1. В Visual Studio Code выберите **Вид** > **Терминал**, чтобы открыть интегрированный терминал VS Code.

2. В окне встроенного терминала введите следующую команду для установки (или обновления) пакета **cookiecutter**, который вы используете для создания шаблона решения IoT Edge в VS Code:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```
   >[!Note]
   >Убедитесь, каталог установки Cookiecutter будет включен в переменную PATH среды, чтобы его можно было вызвать из командной строки. Каталог является частью результата выполнения скрипта установки, например `C:\Users\{user}\AppData\Roaming\Python\Python{version}\Scripts`.
   >
   >Чтобы применить изменения для переменной PATH, перезапустите Visual Studio Code. 

3. Выберите **Представление** > **Палитра команд** для открытия палитры команд VS Code. 

4. В палитре команд введите и выполните команду **Azure: Sign in** (Azure: войти). Следуйте инструкциям, чтобы войти в свою учетную запись Azure. Если вход был выполнен, то этот шаг можно пропустить.

5. В палитре команд введите и выполните команду **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: создать решение IoT Edge). Следуйте инструкциям на экране в палитре команд для создания решения.

   | Поле | Значение |
   | ----- | ----- |
   | Выбор папки | Выберите расположение на компьютере разработчика для VS Code, чтобы создать файлы решения. |
   | Введите название решения. | Введите описательное имя решения или примите имя по умолчанию **EdgeSolution**. |
   | Выбор шаблона модуля | Выберите **Модуль Python**. |
   | Указание имени модуля | Назовите модуль **PythonModule**. |
   | Указание репозитория изображений Docker для модуля | Репозиторий изображений включает в себя имя реестра контейнеров и имя образа контейнера. Образ контейнера предварительно заполняется на последнем шаге. Замените **localhost:5000** на значение сервера входа из реестра контейнеров Azure. Вы можете извлечь сервер входа на странице "Обзор" реестра контейнеров на портале Azure. Окончательная строка выглядит так: \<имя реестра\>.azurecr.io/pythonmodule. |
 
   ![Выбор репозитория образа Docker](./media/tutorial-python-module/repository.png)

Окно VS Code загружает рабочую область решения IoT Edge. Рабочая область решения содержит пять компонентов верхнего уровня. Папка **modules** содержит код Python для модуля, а также файлы Dockerfile для создания модуля в образе контейнера. Файл **\.env** хранит учетные данные реестра контейнеров. Файл **deployment.template.json** содержит сведения, которые среда выполнения IoT Edge использует для развертывания модулей на устройстве. Файл **deployment.debug.template.json** содержит отладочные версии модулей. Работая с этим руководством, вы не будете изменять папку **\.vscode** или файл **\.gitignore**.  

Если вы не указывали реестр контейнеров при создании решения, но приняли значение по умолчанию localhost:5000, у вас не будет файла \.env. 

<!--
   ![Python solution workspace](./media/tutorial-python-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Добавление учетных данных реестра

Файл среды хранит учетные данные для репозитория контейнеров и совместно использует их со средой выполнения IoT Edge. Среде выполнения нужны эти учетные данные, чтобы извлечь частный образ на устройство IoT Edge. 

1. Откройте в обозревателе VS Code файл с расширением ENV. 
2. Обновите поля с **именем пользователя** и **паролем**, скопированные из своего реестра контейнера Azure. 
3. Сохраните этот файл. 

### <a name="update-the-module-with-custom-code"></a>Обновление модуля с помощью пользовательского кода

Каждый шаблон содержит пример кода, который принимает имитируемые данные с датчиков модуля **tempSensor** и направляет их в Центр Интернета вещей. В этом разделе добавьте код, который расширяет **PythonModule**, для анализа сообщений перед их отправкой. 

1. В обозревателе VS Code откройте **modules** > **PythonModule** > **main.py**.

2. В верхней части файла **main.py** импортируйте библиотеку **json**:

    ```python
    import json
    ```

3. Добавьте переменные **TEMPERATURE_THRESHOLD** и **TWIN_CALLBACKS** в разделе глобальных счетчиков. Порог температуры задает значение измеренной температуры компьютера, при превышении которого данные отправляются в Центр Интернета вещей.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Замените функцию **receive_message_callback** следующим кодом:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we forward this message to the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

5. Добавьте новую функцию **module_twin_callback**. Эта функция вызывается при обновлении нужного свойства.

    ```python
    # module_twin_callback is invoked when the module twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

6. В класс **HubManager** добавьте новую строку в метод **__init__** для инициализации функции **module_twin_callback**, которую вы только что добавили:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Сохраните этот файл.

8. В обозревателе VS Code откройте файл **deployment.template.json**. 

   Этот файл указывает **$edgeAgent** развернуть два модуля: **tempSensor**, который моделирует данные устройства, и **PythonModule**. На панели состояния VS Code для устройства IoT Edge указана платформа по умолчанию **amd64**, т. е. модуль **PythonModule** использует версию образа для Linux amd64. При необходимости измените платформу по умолчанию на панели состояния с **amd64** на **arm32v7** или **windows-amd64** в соответствии с архитектурой вашего устройства IoT Edge. Общие сведения о манифестах развертывания см. в статье [Сведения об использовании, настройке и повторном использовании модулей Azure IoT Edge (предварительная версия)](module-composition.md).

   Этот файл также содержит учетные данные реестра. В файле шаблона в качестве имени пользователя и пароля указываются заполнители. При создании манифеста развертывания поля обновляются с помощью значений, добавленных в файл с расширением ENV. 

9. Добавьте двойник модуля **PythonModule** в манифест развертывания. Вставьте следующее содержимое JSON в нижней части раздела **moduleContent** после двойника модуля **$edgeHub**: 

   ```json
       "PythonModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Добавление двойника модуля в шаблон развертывания](./media/tutorial-python-module/module-twin.png)

10. Сохраните этот файл.

## <a name="build-and-push-your-solution"></a>Сборка и отправка решения

В предыдущем разделе вы создали решение IoT Edge и добавили код в **PythonModule**, чтобы отфильтровывать сообщения, где указанная температура компьютера ниже допустимого порогового значения. Теперь необходимо создать решение в качестве образа контейнера и передать его в реестр контейнеров. 

1. Войдите в Docker, введя следующую команду в окне интегрированного терминала Visual Studio Code. Затем можно отправить образ модуля в Реестр контейнеров Azure: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Укажите имя пользователя, пароль и сервер входа, скопированные из Реестра контейнеров Azure при его создании. Вы также можете получить эти значения из раздела **Ключи доступа** в разделе реестра на портале Azure.

2. В обозревателе VS Code щелкните правой кнопкой мыши файл deployment.template.json и выберите **Build and Push IoT Edge solution** (Создать и отправить решение IoT Edge). 

После указания службе Visual Studio Code создать решение, сначала она запишет данные в шаблон развертывания, а затем в новой папке **config** создаст файл deployment.json. Затем выполняются две команды в интегрированном терминале: `docker build` и `docker push`. Они создают ваш код Python, упаковывают его в контейнеры и передают в реестр контейнеров, указанный при инициализации решения. 

Полный адрес образа контейнеров с тегом можно получить из команды `docker build`, выполняемой в интегрированном терминале VS Code. Адрес образа создается на основе информации из файла module.json в формате \<репозиторий\>:\<версия\>-\<платформа\>. В этом руководстве он должен выглядеть так: registryname.azurecr.io/pythonmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Развертывание и запуск решения

При использовании руководства по настройке устройства IoT Edge модуль был развернут с помощью портала Azure. Модули также можно развертывать с помощью расширения "Набор средств для Центра Интернета вещей Azure" (прежнее название — "Набор средств для Интернета вещей Azure") для Visual Studio Code. У вас уже есть манифест развертывания, подготовленный для вашего сценария (см. файл **deployment.json**). Теперь вам осталось выбрать устройство для получения развертывания.

1. В палитре команд VS Code выполните команду **Центр Интернета вещей Azure: Select IoT Hub** (Центр Интернета вещей Azure: выбрать Центр Интернета вещей). 

2. Выберите подписку и Центр Интернета вещей, содержащий устройство IoT Edge, которое нужно настроить. 

3. В обозревателе VS Code разверните раздел **Azure IoT Hub Devices** (Устройства Центра Интернета вещей Azure). 

4. Щелкните имя устройства IoT Edge правой кнопкой мыши, а затем выберите **Create Deployment for Single Device** (Создание развертывания для одного устройства). 

   ![Создание развертывания для одного устройства](./media/tutorial-python-module/create-deployment.png)

5. Выберите файл **deployment.json** в папке **config** и щелкните **Select Edge Deployment Manifest** (Выбрать манифест развертывания Edge). Не используйте файл deployment.template.json. 

6. Нажмите кнопку "Обновить". Вы должны увидеть новый модуль **PythonModule**, работающий вместе с модулем **TempSensor**, а также **$edgeAgent** и **$edgeHub**. 

## <a name="view-generated-data"></a>Просмотр сформированных данных

Когда вы примените манифест развертывания к устройству IoT Edge, в среде выполнения IoT Edge на устройстве начнется сбор сведений о новом развертывании и выполнение операций. Работа всех выполняющихся на устройстве модулей, которые не включены в манифест развертывания, будет остановлена. А модули, которых нет на устройстве, будут запущены. 

Сведения о состоянии устройства IoT Edge можно просмотреть в разделе **Azure IoT Hub Devices** (Устройства Центра Интернета вещей Azure) в обозревателе Visual Studio Code. Разверните раздел со сведениями об устройстве, чтобы просмотреть список развернутых и запущенных модулей. 

На самом устройстве IoT Edge можно просмотреть состояние развертывания модулей с помощью команды `iotedge list`. Вы увидите четыре модуля: два модуля среды выполнения IoT Edge, tempSensor и пользовательский модуль, который вы создали в рамках этого руководства. Запуск всех модулей может занять несколько минут, поэтому выполните команду повторно, если сразу отображаются не все модули. 

Чтобы просмотреть сообщения, создаваемые любым модулем, используйте команду `iotedge logs <module name>`. 

Сообщения можно просматривать после их поступления в центр Интернета вещей с помощью Visual Studio Code. 

1. Для отслеживания данных, поступающих в Центр Интернета вещей, нажмите кнопку с многоточием (**...**), а затем выберите **Start Monitoring D2C Messages** (Начать мониторинг сообщений D2C).
2. Для отслеживания сообщений D2C для конкретного устройства щелкните его правой кнопкой мыши в списке и выберите **Start Monitoring D2C Messages** (Начать мониторинг сообщений D2C).
3. Чтобы перестать отслеживать данные, в палитре команд выполните команду **Azure IoT Hub: Stop monitoring D2C message** (Центр Интернета вещей: остановить мониторинг сообщений D2C). 
4. Для просмотра или обновления двойника модуля щелкните модуль правой кнопкой мыши в списке и выберите **Edit module twin** (Редактирование двойника модуля). Чтобы обновить двойник модуля, сохраните двойник файла JSON, затем щелкните правой кнопкой мыши область редактора и выберите **Update Module Twin** (Обновить двойник модуля).
5. Чтобы просмотреть журналы Docker, установите [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) для VS Code. Локально запущенные модули можно найти в обозревателе Docker. В контекстном меню выберите пункт **Show Logs** (Показывать журналы) для просмотра в интегрированном терминале. 

## <a name="clean-up-resources"></a>Очистка ресурсов 

Если вы планируете перейти к следующей рекомендуемой статье, можно сохранить созданные и повторно используемые ресурсы и конфигурации. Это же устройство IoT Edge также можно использовать в качестве тестового устройства. 

В противном случае чтобы избежать расходов, можно удалить локальные конфигурации и ресурсы Azure, созданные в рамках этой статьи. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

### <a name="delete-local-resources"></a>Удаление локальных ресурсов

Если вы хотите удалить среду выполнения IoT Edge и связанные ресурсы с устройства, выполните следующие команды. 

Удалите среду выполнения IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

При удалении среды выполнения IoT Edge созданные с ее помощью контейнеры остановятся, но будут по-прежнему храниться на устройстве. Просмотрите все контейнеры.

   ```bash
   sudo docker ps -a
   ```

Удалите контейнеры среды выполнения, созданные на устройстве.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Удалите дополнительные контейнеры, которые указаны в выходных данных `docker ps`, ссылаясь на имена контейнеров. 

Удалите среду выполнения контейнера.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы создали модуль IoT Edge с кодом для фильтрации необработанных данных, созданных устройством IoT Edge. Вы можете перейти к следующим руководствам, чтобы узнать о других способах, с помощью которых Azure IoT Edge может помочь вам превратить данные в бизнес-аналитику на пограничном устройстве.

> [!div class="nextstepaction"]
> [Руководство по развертыванию функций Azure в виде модуля](tutorial-deploy-function.md)
> [Руководство по развертыванию Azure Stream Analytics в виде модуля](tutorial-deploy-stream-analytics.md)
