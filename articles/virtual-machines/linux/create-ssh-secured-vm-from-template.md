---
title: Создание виртуальной машины Linux в Azure с помощью шаблона | Документация Майкрософт
description: Как использовать Azure CLI для создания виртуальной машины Linux с помощью шаблона Resource Manager
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 01/03/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c6e4e142025e40e77afee01d70de9cef68eca1f0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013202"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Как создать виртуальную машину Linux с помощью шаблонов Azure Resource Manager

В этой статье показано, как быстро развернуть виртуальную машину Linux с помощью шаблонов Azure Resource Manager и Azure CLI. Эти действия можно также выполнить с помощью [классического Azure CLI](create-ssh-secured-vm-from-template-nodejs.md).


В этой статье показано, как быстро развернуть виртуальную машину Linux с помощью шаблонов Azure Resource Manager и Azure CLI. 

## <a name="templates-overview"></a>Обзор шаблонов
Шаблоны Azure Resource Manager — это файлы JSON, которые определяют инфраструктуру и конфигурацию решения Azure. Этот шаблон можно использовать, чтобы повторно развертывать решение на протяжении всего его жизненного цикла и гарантировать, что ресурсы развертываются в согласованном состоянии. Дополнительные сведения см. в статье [Создание первого шаблона Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). См. дополнительные сведения о синтаксисе JSON при [определении ресурсов в шаблонах Azure Resource Manager](/azure/templates/).

## <a name="create-a-resource-group"></a>Создание группы ресурсов
Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими. Группу ресурсов следует создавать до виртуальной машины. В следующем примере создается группа ресурсов с именем *myResourceGroupVM* в регионе *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Создание виртуальной машины
В следующем примере создается виртуальная машина из [этого шаблона Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) с помощью [az group deployment create](/cli/azure/group/deployment#az_group_deployment_create). Разрешена только аутентификация SSH. При запросе укажите значение собственного открытого ключа SSH, например содержимое *~/.ssh/id_rsa.pub*. Если вам необходимо создать пару ключей SSH, см. сведения в статье [Как создать и использовать пару из открытого и закрытого ключей SSH для виртуальных машин Linux в Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

В предыдущем примере указан шаблон, хранящийся в GitHub. Вы также можете скачать или создать шаблон и указать локальный путь с параметром `--template-file`.


## <a name="connect-to-virtual-machine"></a>Подключение к виртуальной машине
Для подключения по SSH к виртуальной машине получите общедоступный IP-адрес, выполнив команду [az vm show](/cli/azure/vm#az-vm-show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

После этого можно подключиться по SSH к виртуальной машине в обычном режиме. Укажите собственный открытый IP-адрес из предыдущей команды:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Дополнительная информация
В этом примере вы создали базовую виртуальную машину Linux. Дополнительные шаблоны Resource Manager, включающие платформы приложений и позволяющие создать более сложные среды, см. на странице [Шаблоны быстрого запуска Azure](https://azure.microsoft.com/documentation/templates/).

Дополнительные сведения о создании шаблонов, синтаксисе и свойствах JSON для типов ресурсов, которые вы развернули, см. в этих статьях:

* [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups)
* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
* [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
