---
title: "Пользовательские события для службы \"Сетка событий Azure\" с PowerShell | Документация Майкрософт"
description: "Используйте службу \"Сетка событий Azure\" и PowerShell, чтобы иметь возможность публиковать темы и подписываться на эти события."
services: event-grid
keywords: 
author: tfitzmac
ms.author: tomfitz
ms.date: 01/30/2018
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 24366df54fa4fc32ebbff7c1303183707dea17c6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2018
---
# <a name="create-and-route-custom-events-with-azure-powershell-and-event-grid"></a>Создание и перенаправление пользовательских событий с помощью службы Azure PowerShell и "Сетка событий"

"Сетка событий Azure" — это служба обработки событий для облака. В этой статье используется Azure PowerShell, чтобы создать пользовательскую тему, подписаться на тему и активировать событие, чтобы увидеть результат. Как правило, можно отправлять события в конечную точку, реагирующую на событие, например, веб-перехватчик или функция Azure. Однако для упрощения этой статьи можно отправлять события по URL-адресу, где собраны все сообщения. Этот URL-адрес можно создать с помощью сторонних средств от [RequestBin](https://requestb.in/) или [Hookbin](https://hookbin.com/).

>[!NOTE]
>Средства **RequestBin** и **Hookbin** не предназначены для использования с высокой пропускной способностью. Эти средства используются здесь исключительно для демонстрации. Если вы одновременно отправляете несколько событий, в средстве могут отобразиться не все события.

После завершения можно увидеть, что данные события сохранены в конечную точку.

![Данные событий](./media/custom-event-quickstart-powershell/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

В этой статье требуется последняя версия Azure PowerShell. Если вам необходимо выполнить установку или обновление, см. статью [об установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Темами событий сетки являются ресурсы Azure, которые необходимо поместить в группу ресурсов Azure. Группа ресурсов Azure — это логическая коллекция, в которой выполняется развертывание и администрирование ресурсов Azure.

Создайте группу ресурсов с помощью команды [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

В следующем примере создается группа ресурсов с именем *gridResourceGroup* в расположении *westus2*.

```powershell
New-AzureRmResourceGroup -Name gridResourceGroup -Location westus2
```

## <a name="create-a-custom-topic"></a>Создание пользовательской темы

Тема содержит определяемую пользователем конечную точку, в которой можно размещать свои события. В следующем примере создается тема в вашей группе ресурсов. Замените `<topic_name>` уникальным именем для вашей темы. Имя раздела должно быть уникальным, так как оно представлено записью службы доменных имен (DNS).

```powershell
New-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Location westus2 -Name <topic_name>
```

## <a name="create-a-message-endpoint"></a>Создание конечной точки сообщения

Перед подпиской на раздел необходимо создать конечную точку для сообщения о событии. Прежде чем записывать код для реагирования на событие, давайте создадим конечную точку, которая собирает сообщения, чтобы можно было их просматривать. RequestBin и Hookbin — это сторонние средства с открытым кодом, которые позволяют создать конечную точку и просматривать запросы, отправленные на нее. Выберите [RequestBin](https://requestb.in/) и щелкните **Создать RequestBin** или выберите [Hookbin](https://hookbin.com/) и щелкните **Создать новую конечную точку**.  Скопируйте URL-адрес ячейки, так как он понадобится при подписке на тему.

## <a name="subscribe-to-a-topic"></a>Оформление подписки на тему

Подпишитесь на тему, чтобы определить в сетке событий Azure, какие события вам необходимо отслеживать. В следующем примере выполняется подписка на созданный раздел и передается URL-адрес из RequestBin или Hookbin в качестве конечной точки для уведомления о событии. Замените `<event_subscription_name>` уникальным именем для вашей подписки, а `<endpoint_URL>` — значением из предыдущей темы. Указав конечную точку при подписке, служба "Сетка событий Azure" обрабатывает маршрутизацию событий для этой конечной точки. Для `<topic_name>` используйте созданное ранее значение.

```powershell
New-AzureRmEventGridSubscription -EventSubscriptionName <event_subscription_name> -Endpoint <endpoint_URL> -ResourceGroupName gridResourceGroup -TopicName <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Отправка события в тему

Теперь необходимо активировать событие, чтобы увидеть, как Сетка событий Azure распределяет сообщение к вашей конечной точке. Сначала получите URL-адрес и ключ для темы. Еще раз используйте имя раздела для `<topic_name>`.

```powershell
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName gridResourceGroup -Name <topic-name>
```

Для упрощения мы настроим пример данных событий для отправки в раздел. Как правило, приложение или служба Azure отправит данные события. В следующем примере используется хэш-таблица для создания данных событий `htbody` с последующим преобразованием в объект полезных данных JSON с правильным форматом`$body`:

```powershell
$eventID = Get-Random 99999

#Date format should be SortableDateTimePattern (ISO 8601)
$eventDate = Get-Date -Format s

#Construct body using Hashtable
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/motorcycles"
    eventTime= $eventDate   
    data= @{
        make="Ducati"
        model="Monster"
    }
    dataVersion="1.0"
}

#Use ConvertTo-Json to convert event body from Hashtable to JSON Object
#Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
$body = "["+(ConvertTo-Json $htbody)+"]"
```

При просмотре `$body` вы увидите событие полностью. Элемент `data` JSON отображает полезные данные события. Любое значение JSON с правильным форматом может быть в этом поле. Кроме того, можно использовать поле темы для дополнительной маршрутизации и фильтрации.

Теперь отправьте событие в раздел.

```powershell
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Кроме того, вы активировали событие, а служба "Сетка событий" отправила сообщение в конечную точку, настроенную вами при подписке. Перейдите по URL-адресу, созданному ранее. Или нажмите кнопку "Обновить" в открытом браузере. Вы видите отправленные события.

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2018-01-25T15:58:13",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы планируете продолжить работу с этим событием, не очищайте ресурсы, созданные при работе с этой статьей. Если вы не планируете продолжать работу, удалите все созданные ресурсы в этой статье.

```powershell
Remove-AzureRmResourceGroup -Name gridResourceGroup
```

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда вы знаете, как создавать темы и подписки на события, ознакомьтесь с дополнительными сведениями о сетке событий, которые могут помочь вам:

- [An introduction to Azure Event Grid](overview.md) (Общие сведения о службе "Сетка событий Azure")
- [Перенаправление событий хранилища BLOB-объектов в пользовательскую конечную веб-точку (предварительная версия)](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Отслеживание изменений виртуальной машины с помощью Azure Logic Apps и службы "Сетка событий Azure")
- [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md)
