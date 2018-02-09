---
title: "Схема событий службы \"Сетка событий Azure\""
description: "Описание свойств для событий, используемых со службой \"Сетка событий Azure\""
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: babanisa
ms.openlocfilehash: 2b0039c7b90ef6f003641e096521f84885171c26
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema"></a>Схема событий службы "Сетка событий Azure"

В этой статье описываются свойства и схемы, присутствующие во всех событиях. События включают пять обязательных свойств строки и обязательный объект данных. Свойства являются общими для всех событий от любого издателя. Объект данных содержит свойства, характерные для каждого издателя. В случае с системными разделами эти свойства относятся к поставщику ресурсов, включая службу хранилища Azure или концентраторы событий Azure.

События отправляются в службу "Сетка событий Azure" в массиве, который может содержать несколько объектов событий. Если отправляется только одно событие, длина массива составляет 1. Общий размер массива не может превышать 1 МБ. Размер каждого события в массиве ограничен 64 КБ.

Схему JSON для события сетки событий и полезные данные каждого издателя Azure вы найдете в [хранилище схемы событий](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

## <a name="event-schema"></a>Схема событий

В следующем примере показаны свойства, которые используются все издатели событий:

```json
[
  {
    "topic": string,
    "subject": string,    
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Например, вот схема, опубликованная для события хранения больших двоичных объектов Azure:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```
 
## <a name="event-properties"></a>Свойства события

Все события содержат следующие одинаковые данные верхнего уровня:

| Свойство | type | ОПИСАНИЕ |
| -------- | ---- | ----------- |
| Раздел | строка | Полный путь к ресурсу для источника событий. Это поле защищено от записи. Это значение предоставляет служба "Сетка событий". |
| subject | строка | Определенный издателем путь к субъекту событий. |
| eventType | строка | Один из зарегистрированных типов событий для этого источника событий. |
| eventTime | строка | Время создания события с учетом времени поставщика в формате UTC. |
| id | строка | Уникальный идентификатор события. |
| data | object | Данные события, относящиеся к поставщику ресурсов. |
| dataVersion | строка | Версия схемы объекта данных. Версию схемы определяет издатель. |
| metadataVersion | строка | Версия схемы метаданных события. Служба "Сетка событий" определяет схему свойств верхнего уровня. Это значение предоставляет служба "Сетка событий". |

Дополнительные сведения о свойствах в объекте данных см. в источнике события:

* [Подписки Azure (операции управления)](event-schema-subscriptions.md)
* [Хранилище BLOB-объектов](event-schema-blob-storage.md)
* [Концентраторы событий](event-schema-event-hubs.md)
* [Группы ресурсов (операции управления)](event-schema-resource-groups.md)

Для настраиваемых разделов объект данных определяет издатель. Данные верхнего уровня должны содержать те же поля, что и стандартные определенные события ресурса. При публикации событий в настраиваемые разделы рекомендуется моделировать субъект событий для маршрутизации и фильтрации.

## <a name="next-steps"></a>Дополнительная информация

* См. общие сведения о [службе "Сетка событий Azure"](overview.md).
* Дополнительные сведения о создании подписки на Сетку событий Azure см. в статье [Схема подписки для службы "Сетка событий"](subscription-creation-schema.md).
