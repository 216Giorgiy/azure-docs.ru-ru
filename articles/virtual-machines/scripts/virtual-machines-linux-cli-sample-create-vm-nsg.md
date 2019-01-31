---
title: Пример скрипта Azure CLI. Создание двух виртуальных машин с внутренней и внешней группой безопасности сети | Документация Майкрософт
description: Пример скрипта Azure CLI. Создание двух виртуальных машин с внутренней и внешней группой безопасности сети
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7a97c5e0a3f9e7902a6740e0de74271956f13171
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887450"
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Защита сетевого трафика между виртуальными машинами

Этот скрипт создает две виртуальные машины и защищает их входящий трафик. Одна виртуальная машина доступна через Интернет. Ее группа безопасности сети настроена таким образом, чтобы разрешать трафик на портах 22 и 80. Вторая виртуальная машина недоступна через Интернет. Ее группа безопасности сети настроена таким образом, чтобы разрешать трафик только из первой виртуальной машины.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной машины и всех связанных ресурсов этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Создает виртуальную сеть и подсеть Azure. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Создает подсеть. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Создает виртуальную машину и подключает ее к сетевой карте, виртуальной сети, подсети и группе безопасности сети. Эта команда также указывает образ виртуальной машины, который будет использоваться, и учетные данные администратора.  |
| [az network nsg rule list](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_list) | Возвращает сведения о правиле группы безопасности сети. В этом примере имя правила хранится в переменной для дальнейшего использования в скрипте. |
| [az network nsg rule update](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_update) | Обновляет правило NSG. В этом примере внутреннее правило обновляется, чтобы разрешить передачу трафика только из интерфейсной подсети. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры скриптов интерфейса командной строки для виртуальных машин см. в [документации по виртуальным машинам Azure под управлением Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
