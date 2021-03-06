---
title: Упорядочение событий изменения состояния подключения устройства из Центра Интернета вещей Azure с использованием Azure Cosmos DB | Документация Майкрософт
description: В этой статье описывается, как упорядочивать и записывать события изменения состояния подключения устройств из Центра Интернета вещей Azure с использованием Azure Cosmos DB для поддержания последнего состояния подключения.
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: asrastog
ms.openlocfilehash: ff8f8c6656c4cd095749b3e048c72572d113f1ad
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546936"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Упорядочение событий изменения состояния подключения устройства из Центра Интернета вещей Azure с использованием Azure Cosmos DB

Сетка событий Azure поможет вам создавать приложения на основе событий и легко интегрировать события Центра Интернета вещей в бизнес-приложения. В этой статье вы ознакомитесь с настройкой, которую можно использовать для отслеживания и сохранения последнего состояния подключения устройства в Cosmos DB. Мы будем использовать порядковый номер, доступный в событиях отключения и подключения устройства, и сохраним последнее состояние в Cosmos DB. Кроме того, мы используем хранимую процедуру — логику приложения, которая выполняется в коллекции Cosmos DB.

Порядковый номер является строковым представлением шестнадцатеричного числа. Сравнение строк можно использовать для идентификации большего числа. Если вы преобразуете строку в шестнадцатеричную, то число будет 256-битным. Порядковый номер строго возрастает, и номер последнего события будет выше номера других событий. Это полезно, если у вас есть устройство, которое часто подключается и отключается, и вы хотите убедиться, что только последнее событие используется для запуска нижестоящего действия, так как Сетка событий Azure не поддерживает упорядочение событий.

## <a name="prerequisites"></a>Технические условия

* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/).

* Активная учетная запись API SQL Azure Cosmos DB. Если вы ее еще не создали, ознакомьтесь с соответствующими инструкциями в разделе [Создание учетной записи базы данных](../cosmos-db/create-sql-api-dotnet.md#create-an-azure-cosmos-db-account).

* Коллекция в базе данных. Пошаговое руководство см. в разделе [Добавление коллекции](../cosmos-db/create-sql-api-dotnet.md#add-a-database-and-a-collection). При создании коллекции, используйте `/id` ключа секции.

* Центр Интернета вещей в Azure. Если вы его еще не создали, ознакомьтесь с соответствующими инструкциями в статье [Подключение устройства к Центру Интернета вещей с помощью .NET](iot-hub-csharp-csharp-getstarted.md).

## <a name="create-a-stored-procedure"></a>Создание хранимой процедуры

Сначала создайте хранимую процедуру и настройте ее для запуска логики, которая сравнивает порядковые номера входящих событий и записывает последнее событие каждого устройства в базе данных.

1. В API SQL для Cosmos DBI выберите **Обозреватель данных** > **Элементы** > **Новая хранимая процедура**.

   ![Создание хранимой процедуры](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Введите **LatestDeviceConnectionState** для идентификатор хранимой процедуры и вставьте следующий код в **тело хранимой процедуры**. Обратите внимание, что этот код должен заменить любой существующий код в тексте хранимой процедуры. Этот код поддерживает одну строку на идентификатор устройства и записывает последнее состояние подключения этого идентификатора устройства, определяя наивысший порядковый номер.

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};

      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Сохраните хранимую процедуру:

    ![Сохранение хранимой процедуры](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Создайте приложение логики

Сначала создайте приложение логики и добавьте триггер службы "Сетка событий", отслеживающий группу ресурсов для виртуальной машины.

### <a name="create-a-logic-app-resource"></a>Создание ресурса приложения логики

1. В [портала Azure](https://portal.azure.com)выберите **+ создать ресурс**выберите **интеграции** и затем **приложение логики**.

   ![Создание приложения логики](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Присвойте уникальное в вашей подписке имя приложению логики и выберите ту же подписку, группу ресурсов и расположение, которые используются для Центра Интернета вещей.

   ![Новое приложение логики](./media/iot-hub-how-to-order-connection-state-events/new-logic-app.png)

3. Выберите **Создать**, чтобы создать приложение логики.

   Вы создали ресурс Azure для приложения логики. После того, как Azure развернет приложение логики, конструктор Logic Apps отобразит несколько распространенных шаблонов, чтоб вы могли быстрее приступить к работе.

   > [!NOTE]
   > Чтобы найти и снова откройте приложение логики, выберите **групп ресурсов** и выберите группу ресурсов, вы используете для этого практического руководства. Выберите новое приложение логики. Откроется конструктор приложений логики.

4. В конструкторе приложений логики прокрутите окно вправо, пока не увидите часто используемых триггеров. В разделе **шаблоны**, выберите **пустое приложение логики** , что можно создавать приложения логики с нуля.

### <a name="select-a-trigger"></a>Выбор триггера

Триггер представляет собой определенное событие, которое запускает приложение логики. В этом руководстве триггер, который инициирует рабочий процесс, получает запрос через HTTP.

1. В панели поиска триггеров и соединителей, введите **HTTP** и нажмите клавишу ВВОД.

2. Выберите в качестве триггера вариант **Запрос — при получении HTTP-запроса**.

   ![Выбор триггера запроса HTTP](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. Выберите **Использовать пример полезной нагрузки, чтобы создать схему**.

   ![Использование примера полезных данных для создания схемы](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Вставьте пример кода JSON ниже в текстовое поле, а затем нажмите кнопку **Готово**:

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

   ![Вставьте пример полезных данных JSON](./media/iot-hub-how-to-order-connection-state-events/paste-sample-payload.png)

5. Вы можете получить всплывающее уведомление: **Не забудьте включить заголовок Content-Type со значением application/json в запросе.** Можно спокойно проигнорировать это уведомление и перейти к следующему разделу.

### <a name="create-a-condition"></a>Создание условия

Условия помогают выполнять определенные действия после удовлетворения определенного условия в рабочем процессе приложения логики. Когда условие выполняется, можно определить нужное действие. В этом руководстве применяется условие, проверяющее, соответствует ли значение eventType подключенному или отключенному устройству. Действие выполнит хранимую процедуру в вашей базе данных.

1. Выберите **+ новый шаг** затем **встроенные**, а затем найдите и выберите **условие**. Щелкните в **выберите значение** и появится всплывающее окно отображения динамического содержимого — поля, которые могут быть выбраны. Заполните поля, как показано ниже, для выполнения этого только для событий устройства подключения и отключения устройства:

   * Выберите значение: **eventType** --выбрать из поля в динамического содержимого, которые отображаются, если щелкнуть это поле.
   * Изменение «равно» для **заканчивается**.
   * Выберите значение: **nected**.

     ![Заполнение условия](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

2. В **при значении true** диалоговое окно, щелкните **добавить действие**.
  
   ![Добавление действия в том случае, если значение равно true](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

3. Поиск Cosmos DB и выберите **Azure Cosmos DB — выполнение хранимой процедуры**

   ![Поиск Cosmos DB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

4. Заполните **cosmosdb-connection** для **имя подключения** и выберите запись в таблице, а затем выберите **создать**. Вы увидите **выполнения хранимой процедуры** панели. Введите значения для полей:

   **Идентификатор базы данных**: ToDoList

   **Идентификатор коллекции**: Items

   **Идентификатор sproc**: LatestDeviceConnectionState

5. Выберите **добавьте параметр**. В появившемся раскрывающемся списке, установите флажки рядом с полем **ключ секции** и **параметров для хранимой процедуры**, затем щелкните в любом месте на экране; он добавляет поле для значения ключа секции и для параметры для хранимой процедуры.

   ![Заполнение действия приложения логики](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Теперь введите значение ключа секции и параметры, как показано ниже. Убедитесь, что поместить в скобки и двойные кавычки как показано. Может понадобиться щелкнуть **добавить динамическое содержимое** получить допустимые значения, можно использовать здесь.

   ![Заполнение действия приложения логики](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure-2.png)

7. В верхней части области, где отображается надпись **для каждого**в разделе **выберите выходные данные из предыдущих шагов**, убедитесь, что он **текст** выбран.

   ![заполнить для каждого приложения логики](./media/iot-hub-how-to-order-connection-state-events/logicapp-foreach-body.png)

8. Сохраните приложение логики.

### <a name="copy-the-http-url"></a>Копирование URL-адреса HTTP

Скопируйте для триггера URL-адрес, с которого приложение логики ожидает передачи данных, а затем закройте конструктор Logic Apps. Этот URL-адрес вы используете для настройки сетки событий.

1. Щелкните поле конфигурации триггера **При получении HTTP-запроса**, чтобы развернуть его.

2. Скопируйте значение **URL-адрес HTTP POST**, нажав кнопку копирования рядом с этим параметром.

   ![Копирование URL-адреса HTTP POST](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Сохраните этот URL-адрес, чтобы использовать его в следующем разделе.

## <a name="configure-subscription-for-iot-hub-events"></a>Настройка подписки на события Центра Интернета вещей

В этом разделе вы настроите Центр Интернета вещей для публикации событий по мере их появления.

1. Найдите нужный Центр Интернета вещей на портале Azure.

2. Выберите **События**.

   ![Открытие сведений сетки событий](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. Выберите **+ подписка на события**.

   ![Создание подписки на события](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Заполните **сведения о подписке на событие**: Укажите описательное имя и выберите **Схема Сетки событий**.

5. Заполните **типы событий** поля. Снимите флажок **подписка на все типы событий** и выберите **устройство подключено** и **устройство отключено** в меню.

   ![Задайте типы событий для поиска](./media/iot-hub-how-to-order-connection-state-events/set-event-types.png)

6. Для **сведения о конечной точке**, выберите тип конечной точки как **веб-перехватчик** щелкните выберите конечную точку и вставьте URL-адрес, скопированный из приложения логики и подтвердите выбор.

   ![Выберите URL-адрес конечной точки](./media/iot-hub-how-to-order-connection-state-events/endpoint-url.png)

7. Формы теперь должен выглядеть аналогично приведенному ниже:

   ![Пример формы подписки на события](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

   Выберите **Создать**, чтобы сохранить подписку на события.

## <a name="observe-events"></a>Просмотр событий

Теперь, когда ваша подписка на события настроена, давайте протестируем, подключив устройство.

### <a name="register-a-device-in-iot-hub"></a>Регистрация устройства в Центре Интернета вещей

1. В Центре Интернета вещей выберите **IoT Devices** (Устройства Интернета вещей).

2. Выберите **+ добавить** в верхней части области.

3. Для **идентификатора устройства** введите `Demo-Device-1`.

4. Щелкните **Сохранить**.

5. Вы можете добавить несколько устройств с разными идентификаторами.

   ![Устройства, добавили в центр](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Нажмите кнопку на устройстве. Теперь строки подключения и ключи будут заполняться. Скопируйте значение из поля **Connection string -- primary key** (Строка подключения — первичный ключ) для последующего использования.

   ![ConnectionString для устройства](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

HostName=test-eventgrid-hub.azure-devices.net;DeviceId=Demo-Device-1; SharedAccessKey = cv8uPNixe7E2R9EHtimoY/PlJfBV/lOYCMajVOp/Cuw =

### <a name="start-raspberry-pi-simulator"></a>Запуск симулятора Raspberry Pi

1. Давайте используем веб-симулятор Raspberry Pi для имитации подключения устройства.

[Запустить симулятор Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Запуск примера приложения в веб-симуляторе Raspberry Pi

Следующие действия активируют событие, связанное с устройством.

1. В области кодирования замените значения заполнителя в 15 строке строку подключения устройства центра Интернета вещей Azure, сохраненный в конце предыдущего раздела.

   ![Вставьте строку подключения устройства](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Запустите приложение, выбрав **запуска**.

Можно увидеть примерно следующие выходные данные, содержащие данные датчика и сообщения, отправленные в центр Интернета вещей.

   ![Запуск приложения](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Нажмите кнопку **Остановить**, чтобы остановить симулятор, и запустите событие **Device Disconnected** (Устройство отключено).

Вы запустили пример приложения, чтобы собрать данные датчика и отправить их в Центр Интернета вещей.

### <a name="observe-events-in-cosmos-db"></a>Просмотр событий в Cosmos DB

Вы можете увидеть результаты выполненной хранимой процедуры в вашем документе Cosmos DB. Это будет выглядеть следующим образом. Каждая строка содержит последнее состояние подключения каждого устройства.

   ![Результат](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Использование Azure CLI

Вместо того чтобы использовать [портал Azure](https://portal.azure.com), шаги для работы с Центром Интернета вещей можно выполнить с помощью Azure CLI. С дополнительными сведениями можно ознакомиться в статьях, посвященных созданию [подписки на события](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) и [устройств Интернета вещей](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) с использованием Azure CLI.

## <a name="clean-up-resources"></a>Очистка ресурсов

В этом руководстве используются ресурсы, за которые в подписке Azure предусмотрена плата. По завершении работы с этим руководством и тестирования результатов отключите или удалите ресурсы, которые больше не нужны.

Чтобы не потерять работу, проделанную в приложении логики, мы рекомендуем отключить ресурсы, а не удалять их.

1. Перейдите в приложение логики.

2. В колонке **Обзор** выберите **Удалить** или **Отключить**.

    В подписке может быть только один бесплатный Центр Интернета вещей. Если для работы с этим руководством вы создали бесплатный Центр Интернета вещей, его можно не удалять, так как никакая плата взиматься не будет.

3. Перейдите в Центр Интернета вещей.

4. В колонке **Обзор** выберите **Удалить**.

    Даже если вы не удалите свой Центр Интернета вещей, вы можете удалить созданную подписку на события.

5. В Центре Интернета вещей выберите **Сетка событий**.

6. Выберите подписку на события, которую нужно удалить.

7. Нажмите кнопку **Удалить**.

Чтобы удалить учетную запись Azure Cosmos DB на портале Azure, щелкните правой кнопкой мыши имя учетной записи и выберите **Удалить учетную запись**. Подробные инструкции см. в разделе [Удаление учетной записи Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/manage-account).

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о [реагировании на события в Центре Интернета вещей, используя службу "Сетка событий" для запуска действий](../iot-hub/iot-hub-event-grid.md).

* [Send email notifications about Azure IoT Hub events using Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md) (Отправка уведомлений электронной почты о событиях в Центре Интернета вещей Azure с помощью Logic Apps)

* Узнайте о дополнительных возможностях службы [Сетка событий](../event-grid/overview.md).