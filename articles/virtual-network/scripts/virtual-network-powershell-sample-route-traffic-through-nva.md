---
title: Пример скрипта Azure PowerShell. Маршрутизация трафика через виртуальный сетевой модуль | Документация Майкрософт
description: Пример скрипта Azure PowerShell для маршрутизации трафика через виртуальный сетевой модуль брандмауэра.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 56b526b186acdd0903910a5a10f5fa3aa3c454df
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650081"
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>Пример скрипта для маршрутизации трафика через виртуальный сетевой модуль

В этом примере скрипта создается виртуальная сеть с интерфейсной и внутренней подсетями. Скрипт также создает виртуальную машину с IP-пересылкой, которая позволяет маршрутизировать трафик между двумя подсетями. После выполнения скрипта вы можете развернуть на виртуальной машине программное обеспечение сети, например брандмауэр.

Вы можете выполнить скрипт из Azure [Cloud Shell](https://shell.azure.com/powershell) или из локальной установки PowerShell. Если вы используете PowerShell локально, для выполнения этого скрипта вам понадобится модуль Az PowerShell 5.4.1 или последующей версии. Выполните командлет `Get-Module -ListAvailable Az`, чтобы узнать установленную версию. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-Az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной сети и групп безопасности сети в этом скрипте используются приведенные ниже команды. Для каждой команды в следующей таблице приведены ссылки на соответствующую документацию:

| Команда | Примечания |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Создает виртуальную сеть Azure и интерфейсную подсеть. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Создает внутреннюю подсеть и подсеть DMZ. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Создает общедоступный IP-адрес для доступа к виртуальной машине из Интернета. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Создает виртуальный сетевой интерфейс и включает IP-пересылку для него. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Создает группу безопасности сети (NSG). |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Создает правила группы безопасности сети, которые разрешают входящий трафик по портам HTTP и HTTPS к виртуальной машине. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)| Связывает группы безопасности сети и таблицы маршрутов с подсетями. |
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)| Создает таблицу маршрутов для всех маршрутов. |
| [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)| Создает маршруты для маршрутизации трафика между подсетями и Интернетом через виртуальную машину. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Создает виртуальную машину и присоединяет к ней сетевой адаптер. Эта команда также указывает образ виртуальной машины и учетные данные администратора. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)  | Удаляет группу ресурсов и все содержащиеся в ней ресурсы. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Дополнительные примеры скриптов см. в статье [Azure PowerShell samples for virtual network](../powershell-samples.md) (Примеры скриптов Azure PowerShell для виртуальной сети).