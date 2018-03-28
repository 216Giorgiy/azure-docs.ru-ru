---
title: Установка пирингового подключения между виртуальными сетями — PowerShell | Документация Майкрософт
description: Сведения о том, как установить пиринговое подключение между виртуальными сетями.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: b067dfd6d50b61614c2f3de2fa0e159cd645f9eb
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Установка пирингового подключения между виртуальными сетями с помощью PowerShell

Виртуальные сети можно подключить друг к другу с помощью пиринговой связи. После установки пиринговой связи ресурсы в обеих виртуальных сетях могут взаимодействовать друг с другом с такой же задержкой и пропускной способностью, как если бы эти ресурсы находились в одной виртуальной сети. В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * создание двух виртуальных сетей;
> * соединение двух виртуальных сетей с помощью пиринговой связи;
> * развертывание виртуальной машины в каждой из виртуальных сетей;
> * взаимодействие между виртуальными машинами.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Чтобы установить и использовать PowerShell локально, для работы с этой статьей вам понадобится модуль Azure PowerShell 5.4.1 или более поздней версии. Выполните командлет ` Get-Module -ListAvailable AzureRM`, чтобы узнать установленную версию. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Login-AzureRmAccount`, чтобы создать подключение к Azure. 

## <a name="create-virtual-networks"></a>Создание виртуальных сетей

Перед созданием виртуальной сети необходимо создать для нее группу ресурсов и другие компоненты, указанные в этой статье. Создайте группу ресурсов с помощью командлета [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Создайте виртуальную сеть с помощью командлета [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). В следующем примере создается виртуальная сеть с именем *myVirtualNetwork1* и префиксом адреса *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Создайте конфигурацию подсети с помощью командлета [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). В следующем примере создается конфигурация подсети с префиксом адреса 10.0.0.0/24:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Запишите конфигурацию подсети в виртуальную сеть с помощью командлета [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), который создает подсеть.

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

Создайте виртуальную сеть с префиксом адреса 10.1.0.0/16 и одной подсетью:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Установка пиринговой связи между виртуальными сетями

Создайте пиринг с помощью командлета [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering). В следующем примере создается пиринговая связь между сетями *myVirtualNetwork1* и *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

В выходных данных, возвращенных после выполнения предыдущей команды, для **состояния пиринга** будет отображаться значение *Инициировано*. Состояние *Инициировано* не изменится, пока не будет создан пиринг сети *myVirtualNetwork2* с сетью *myVirtualNetwork1*. Установите пиринговую связь сети *myVirtualNetwork2* с сетью *myVirtualNetwork1*. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

В выходных данных, возвращенных после выполнения предыдущей команды, для **состояния пиринга** будет отображаться значение *Подключено*. Кроме того, в Azure состояние пиринга *myVirtualNetwork1-myVirtualNetwork2* изменяется на *Подключено*. Убедитесь, что состояние пиринга для *myVirtualNetwork1 myVirtualNetwork2* изменено на *Подключено*, с помощью командлета [Get AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Ресурсы одной виртуальной сети не могут взаимодействовать с ресурсами другой виртуальной сети, пока **состояние пиринга** для обеих виртуальных сетей не будет иметь значение *Подключено*. 

## <a name="create-virtual-machines"></a>Создание виртуальных машин

Создайте виртуальную машину в каждой виртуальной сети, чтобы на следующем шаге организовать взаимодействие между такими машинами.

### <a name="create-the-first-vm"></a>Создание первой виртуальной машины

Создайте виртуальную машину с помощью командлета [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). В приведенном ниже примере в виртуальной сети *myVirtualNetwork1* создается виртуальная машина с именем *myVm1*. Параметр `-AsJob` позволяет создать виртуальную машину в фоновом режиме, чтобы можно было перейти к следующему шагу. Когда появится запрос, введите имя пользователя и пароль, которые вы будете использовать для входа на виртуальную машину.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>Создание второй виртуальной машины

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Создание виртуальной машины занимает несколько минут. Не выполняйте следующие шаги, пока в Azure не будет создана виртуальная машина и не будут возвращены выходные данные в PowerShell.

## <a name="communicate-between-vms"></a>Взаимодействие между виртуальными машинами

Вы можете подключиться к общедоступному IP-адресу виртуальной машины через Интернет. Чтобы получить общедоступный IP-адрес виртуальной машины, выполните командлет [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Приведенный ниже пример возвращает общедоступный IP-адрес виртуальной машины *myVm1*:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

На локальном компьютере выполните следующую команду для создания сеанса удаленного рабочего стола с виртуальной машиной *myVm1*. Замените `<publicIpAddress>` IP-адресом, полученным от предыдущей команды.

```
mstsc /v:<publicIpAddress>
```

Будет создан файл протокола удаленного рабочего стола (RDP-файл). Он будет скачан на ваш компьютер и открыт. Введите имя пользователя и пароль, а затем нажмите кнопку **ОК**. Возможно, понадобится выбрать **More choices** (Дополнительные варианты) и **Use a different account** (Использовать другую учетную запись), чтобы указать учетные данные, введенные при создании виртуальной машины. При входе в систему может появиться предупреждение о сертификате. Чтобы продолжить процесс подключения, нажмите кнопку **Да** или **Продолжить**.

На виртуальной машине *myVm1* с помощью PowerShell разрешите протокол ICMP в брандмауэре Windows, чтобы на следующем шаге нормально работала проверка связи с виртуальной машины *myVm2*:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Хотя проверка связи используется в этой статье для взаимодействия между виртуальными машинами, мы не рекомендуем разрешать протокол ICMP в брандмауэре Windows в рабочей среде.

Для подключения к виртуальной машине *myVm2* введите следующую команду в командной строке на виртуальной машине *myVm1*:

```
mstsc /v:10.1.0.4
```

Так как вы включили проверку связи на виртуальной машине *myVm1*, теперь можно проверить связь с ней с помощью IP-адреса из командной строки на виртуальной машине *myVm2*:

```
ping 10.0.0.4
```

Вы получите четыре ответа. Отключите RDP-сеансы с *myVm1* и *myVm2*.

## <a name="clean-up-resources"></a>Очистка ресурсов

Вы можете удалить ненужную группу ресурсов и все содержащиеся в ней ресурсы, выполнив командлет [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

**<a name="register"></a>Регистрация для использования предварительной версии функции пиринга между виртуальными сетями в разных регионах**

Функция создания пиринговых связей между виртуальными сетями в одном регионе является общедоступной. Функция пиринга между виртуальными сетями в разных регионах сейчас находится на этапе предварительной версии. Доступные регионы перечислены в разделе [Обновления Azure](https://azure.microsoft.com/updates/?product=virtual-network). Чтобы создать пиринг между виртуальными сетями в разных регионах, сначала нужно зарегистрироваться для использования предварительной версии, выполнив описанные ниже действия (в подписке каждой виртуальной сети, для которой требуется пиринг).

1. Зарегистрируйте подписку, в которой находится каждая виртуальная сеть, для которой нужно создать пиринг, чтобы использовать предварительную версию, введя следующие команды:

    ```powershell-interactive
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
2. Убедитесь, что регистрация прошла успешно, выполнив следующую команду:

    ```powershell-interactive    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Если вы попытаетесь установить пиринговую связь между виртуальными сетями в разных регионах до того, как параметр **RegistrationState** в выходных данных предыдущей команды для обеих подписок получит значение **Зарегистрировано**, пиринговая связь не будет установлена.

## <a name="next-steps"></a>Дополнительная информация

Из этой статьи вы узнали, как соединить две виртуальные сети с помощью пиринга. Из этой статьи вы узнали, как с помощью пиринговой связи соединить две виртуальные сети в одном расположении Azure. Пиринг можно использовать и для связи виртуальных сетей из [разных регионов](#register) или [разных подписок Azure](create-peering-different-subscriptions.md#portal), а также для создания [звездообразной топологии сети](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering). Прежде чем настраивать пиринговую связь между рабочими виртуальными сетями, внимательно ознакомьтесь с [общими сведениями о пиринге](virtual-network-peering-overview.md), [сведениями об управлении пирингом](virtual-network-manage-peering.md) и [ограничениями виртуальных сетей](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Теперь вы можете [подключить свой компьютер к виртуальной сети](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) через VPN и взаимодействовать с ресурсами в виртуальной сети или в виртуальных сетях, соединенных пиринговой связью. Перейдите к примерам скриптов для многократного использования, с помощью которых можно выполнять многие задачи, описанные в статьях о виртуальных сетях.

> [!div class="nextstepaction"]
> [Примеры скриптов для виртуальных сетей](../networking/powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
