---
title: включение файла
description: включение файла
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4207031652db7ec4b2ae5468dc159320f6efdbc2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228820"
---
## <a name="create-a-media-services-account"></a>Создание учетной записи служб мультимедиа

Сначала нужно создать учетную запись Служб мультимедиа. Из этой статьи вы узнаете, что необходимо для создания учетной записи с помощью Azure CLI.

### <a name="create-a-resource-group"></a>Создание группы ресурсов

Чтобы создать группу ресурсов, выполните указанную ниже команду. Группа ресурсов Azure — это логический контейнер, в котором происходит развертывание ресурсов, таких как учетные записи Служб мультимедиа Azure и связанные с ними учетные записи хранения, а также управление ими.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Создание учетной записи хранения

При создании учетной записи Служб мультимедиа необходимо предоставить имя ресурса учетной записи хранения Azure. Указанная учетная запись хранения присоединена к учетной записи Служб мультимедиа. 

Необходимо иметь **основную** учетную запись хранения. У вас может быть любое количество **вторичных** учетных записей хранения, связанных с учетной записью Служб мультимедиа. Службы мультимедиа поддерживают учетные записи **общего назначения версии 2** (GPv2) или **общего назначения версии 1** (GPv1). Учетные записи, предназначенные только для большого двоичного объекта, нельзя использовать в качестве **основных**. Дополнительные сведения об учетных записях хранения см. в статье [Варианты учетной записи хранения Azure](../articles/storage/common/storage-account-options.md). 

Дополнительные сведения об использовании учетных записей хранения в Службах мультимедиа см. в [этой статье](../articles/media-services/latest/storage-account-concept.md).

Следующая команда создает учетную запись хранения, которая будет связана с учетной записью Служб мультимедиа. В приведенном ниже скрипте `storageaccountforams` можно заменить своим значением. Имя учетной записи должно содержать не более 24 символов.

```azurecli
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Создание учетной записи служб мультимедиа

Указанная ниже команда Azure CLI создает новую учетную запись Служб мультимедиа. Вы можете заменить следующие значения: `amsaccount` `storageaccountforams` (должно совпадать со значением, заданным для учетной записи хранения) и `amsResourceGroup` (должно совпадать со значением, заданным для группы ресурсов).

```azurecli
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
