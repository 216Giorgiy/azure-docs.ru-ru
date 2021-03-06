---
title: Ресурсы в Службах мультимедиа в Azure | Документация Майкрософт
description: В этой статье приведены общие сведения о ресурсах и их использовании в Службах мультимедиа Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/19/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2ec2ddbac5d0368aaf1b46208c9ebb44bf12a622
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447316"
---
# <a name="assets"></a>Активы

В Службах мультимедиа Azure сущность [Ресурс](https://docs.microsoft.com/rest/api/media/assets) содержит цифровые файлы (в том числе видео, аудио, изображения, коллекции эскизов, текстовые дорожки и файлы скрытых титров), а также метаданные об этих файлах. После загрузки цифровых файлов в ресурс их можно использовать в рабочих процессах кодирования, потоковой передачи и анализа содержимого Служб мультимедиа. Дополнительные сведения см. в разделе [Отправка цифровых файлов в ресурсы](#upload-digital-files-into-assets) ниже.

Ресурс сопоставляется с контейнером больших двоичных объектов в [учетной записи хранения Azure](storage-account-concept.md). Файлы ресурса хранятся в этом контейнере как блочные BLOB-объекты. Если в учетной записи используется хранилище общего назначения версии 2 (GPv2), Службы мультимедиа поддерживают уровни больших двоичных объектов. Это хранилище позволяет перемещать файлы в [холодный или архивный уровень хранилища](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Архивный** уровень хранилища подходит для архивации ненужных исходных файлов (например, после шифрования).

**Архивный уровень хранилища** рекомендуется только для очень больших исходных файлов, которые уже были закодированы, а выходные данные задания кодирования были помещены в выходной контейнер больших двоичных объектов. Большие двоичные объекты в выходном контейнере, который вы хотите привязать к ресурсу и использовать для потоковой передачи или анализа содержимого, должны существовать на **горячем** или **холодном** уровне хранилища.

> [!NOTE]
> Свойства ресурса типа Datetime всегда задаются в формате UTC.

## <a name="upload-digital-files-into-assets"></a>Отправка цифровых файлов в ресурсы

Один из стандартных рабочих процессов Служб мультимедиа — отправка, кодирование и потоковая передача файла. В этом разделе описываются общие инструкции.

1. Используйте API Служб мультимедиа версии 3, чтобы создать новый входной ресурс. Эта операция создает контейнер в учетной записи хранения, связанной с вашей учетной записью Служб мультимедиа. API возвращает имя контейнера (например, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Если у вас уже есть контейнер больших двоичных объектов, который необходимо связать с ресурсом, можно указать имя контейнера при создании ресурса. Службы мультимедиа в настоящее время поддерживают только большие двоичные объекты в корневом контейнере, а не с путями в имени файла. Таким образом, контейнер с именем файла input.mp4 подойдет. А контейнер с именем файла videos/inputs/input.mp4 не подойдет.

    Azure CLI можно использовать для передачи непосредственно в любую учетную запись хранения и контейнер, на которые у вас есть права в вашей подписке. <br/>Имя контейнера должно быть уникальным и соответствовать правилам именования хранилища. Имя не обязательно должно соответствовать формату имени контейнера ресурса Служб мультимедиа (Asset-GUID). 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Получите URL-адрес SAS с разрешениями на чтение и запись, которые будут использоваться для передачи цифровых файлов в контейнер ресурса. Можно использовать API Служб мультимедиа для вывода [списка URL-адресов контейнеров ресурса](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Используйте API службы хранилища Azure или пакеты SDK (например, [REST API службы хранилища](../../storage/common/storage-rest-api-auth.md), [пакет SDK для JAVA](../../storage/blobs/storage-quickstart-blobs-java-v10.md) или [пакет SDK для .NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) для передачи файлов в контейнер ресурса. 
4. Используйте API Служб мультимедиа версии 3, чтобы создать преобразование и задание для обработки входного ресурса. Дополнительные сведения см. в статье [Преобразования и задания](transform-concept.md).
5. Потоковая передача содержимого из выходного ресурса.

Полный пример .NET, в котором показано, как создать ресурс, получить доступный для записи URL-адрес SAS для контейнера ресурса в хранилище, отправить файл в контейнер в хранилище с помощью URL-адреса SAS, см. в разделе [Создание входных данных задания из локального файла](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Создание ресурса

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Пример REST см. в разделе [Создание ресурса с помощью REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples).

В примере показано создание **текста запроса**, где можно указать полезные сведения, такие как описание, имя контейнера, учетную запись хранения и другие сведения.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Полный пример см. в разделе [Создание входных данных задания из локального файла](job-input-from-local-file-how-to.md). В Службах мультимедиа версии 3 входные данные задания также могут создаваться из URL-адреса HTTPS (см. раздел [Создание входных данных задания из URL-адреса HTTPS](job-input-from-http-how-to.md)).

## <a name="filtering-ordering-paging"></a>Фильтрации, упорядочивание, разбиение по страницам

Ознакомьтесь с разделом [Фильтрация, упорядочивание и разбиение по страницам сущностей Служб мультимедиа](entities-overview.md).

## <a name="storage-side-encryption"></a>Шифрование на стороне хранилища

Чтобы защитить неактивные ресурсы, их нужно зашифровать на стороне хранилища. В следующей таблице показано, как происходит шифрование на стороне хранилища в Службах мультимедиа.

|Вариант шифрования|ОПИСАНИЕ|Службы мультимедиа версии 2|Службы мультимедиа версии 3|
|---|---|---|---|
|Шифрование хранилища Служб мультимедиа|Шифрование AES-256. Ключами управляют Службы мультимедиа|Поддерживается<sup>(1)</sup>|Не поддерживается<sup>(2)</sup>|
|[Шифрование службы хранилища для неактивных данных](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Шифрование на стороне сервера, предоставляемое службой хранилища Azure. Ключами управляет Azure или клиент|Поддерживаются|Поддерживаются|
|[Шифрование хранилища на стороне клиента](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Шифрование на стороне клиента, предоставляемое службой хранилища Azure. Ключами управляет клиент в Key Vault|Не поддерживается|Не поддерживается|

<sup>1</sup>Хотя Службы мультимедиа поддерживают обработку содержимого в чистом, незашифрованном виде, мы не рекомендуем ее использовать.

<sup>2</sup>В Службах мультимедиа версии 3 для обратной совместимости поддерживается шифрование хранилища (шифрование AES-256), только если ресурсы созданы с помощью Служб мультимедиа версии 2. То есть версия 3 работает с существующими зашифрованными ресурсами хранилища, но создание новых ресурсов не поддерживается.

## <a name="next-steps"></a>Дополнительная информация

* [Краткое руководство по потоковой передаче видеофайлов — .NET](stream-files-dotnet-quickstart.md)
* [Использование средства цифровой видеозаписи в облаке](live-event-cloud-dvr.md)
* [Различия между Службами мультимедиа версий 2 и 3](migrate-from-v2-to-v3.md)
