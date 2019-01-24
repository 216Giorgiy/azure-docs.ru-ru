---
title: Пример скрипта Azure CLI. Удаление контейнеров на основе префикса | Документация Майкрософт
description: Сведения об удалении контейнеров больших двоичных объектов службы хранилища Azure на основе префикса имени.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: 85ee6505adafab9587f3583cd4c7182efcc43c11
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853737"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Удаление контейнеров на основе префикса имени

Приведенный в этой статье скрипт сначала создает несколько демонстрационных контейнеров в хранилище BLOB-объектов Azure, а затем удаляет некоторые из их на основе определенного префикса имени.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните команду ниже, чтобы удалить группу ресурсов, оставшиеся контейнеры и все связанные ресурсы.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Ниже приведены команды, с помощью которых скрипт удаляет контейнеры на основе префикса имени. Для каждого элемента в таблице приведены ссылки на документацию по команде.

| Команда | Примечания |
|---|---|
| [az group create](/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Создает учетную запись хранения Azure в указанной группе ресурсов. |
| [az storage container create](/cli/azure/storage/container#az_storage_container_create) | Создает контейнер в хранилище BLOB-объектов Azure. |
| [az storage container list](/cli/azure/storage/container) | Возвращает список контейнеров BLOB-объектов в учетной записи службы хранилища Azure. |
| [az storage container delete](/cli/azure/storage/container#az_storage_container_delete) | Удаляет контейнеры в учетной записи службы хранилища Azure. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры скриптов CLI хранилища см. в статье [Примеры скриптов Azure CLI для хранилища BLOB-объектов Azure](../blobs/storage-samples-blobs-cli.md).
