---
title: Руководство по установке приложений в масштабируемом наборе с помощью Azure PowerShell | Документация Майкрософт
description: Узнайте, как с помощью Azure PowerShell устанавливать приложения в масштабируемые наборы виртуальных машин с использованием расширения пользовательских скриптов.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 4f162dcf58316e6d9f39b71be37abf5626e93c75
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295830"
---
# <a name="tutorial-install-applications-in-virtual-machine-scale-sets-with-azure-powershell"></a>Руководство. Установка приложений в масштабируемые наборы виртуальных машин с помощью Azure PowerShell
Для запуска приложений в экземплярах виртуальных машин в масштабируемом наборе необходимо сначала установить компоненты и необходимые файлы этих приложений. Из предыдущего руководства вы узнали, как создать и использовать настраиваемый образ виртуальной машины для развертывания экземпляров виртуальных машин. Этот настраиваемый образ включал ручную установку и конфигурацию приложения. Также можно автоматизировать установку приложений в масштабируемом наборе после развертывания каждого экземпляра виртуальной машины или обновить приложение, которое уже выполняется в масштабируемом наборе. Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * автоматически устанавливать приложения в масштабируемый набор;
> * использовать расширения пользовательских скриптов;
> * обновлять приложение, работающее в масштабируемом наборе.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Чтобы установить и использовать PowerShell локально для работы с этим руководством, вам понадобится модуль Azure PowerShell 6.0.0 или более поздней версии. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable AzureRM`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzureRmAccount`, чтобы создать подключение к Azure. 


## <a name="what-is-the-azure-custom-script-extension"></a>Что такое расширение пользовательских скриптов Azure?
Расширение настраиваемых сценариев скачивает и выполняет сценарии на виртуальных машинах Azure. Это расширение можно использовать для настройки после развертывания, установки программного обеспечения и других задач настройки или управления. Сценарии можно скачать из службы хранилища Azure или GitHub или передать на портал Azure во время выполнения расширения.

Расширение пользовательских скриптов интегрируется с шаблонами Azure Resource Manager, и его также можно использовать с Azure CLI 2.0, Azure PowerShell, порталом Azure или REST API Дополнительные сведения см. в статье [Расширение Custom Script в ОС Windows](../virtual-machines/windows/extensions-customscript.md).

Чтобы увидеть расширение пользовательских скриптов в действии, создайте масштабируемый набор, который устанавливает веб-сервер IIS и выводит имя узла экземпляра виртуальной машины масштабируемого набора. Определение расширения пользовательских скриптов скачивает пример скрипта из репозитория GitHub, устанавливает необходимые пакеты, а затем записывает имя узла экземпляра виртуальной машины на базовую HTML-страницу.


## <a name="create-a-scale-set"></a>Создание масштабируемого набора
Теперь создайте масштабируемый набор виртуальных машин с помощью командлета [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). Чтобы распределить трафик между отдельными экземплярами виртуальных машин, создается еще и подсистема балансировки нагрузки. Подсистема балансировки нагрузки определяет правила передачи трафика на TCP-порт 80, а также разрешает подключение удаленного рабочего стола трафик через TCP-порт 3389 и удаленное взаимодействие PowerShell через TCP-порт 5985. При появлении запроса введите учетные данные администратора для экземпляров виртуальных машин в масштабируемом наборе:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "EastUS" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic"
```

Создание и настройка всех ресурсов и виртуальных машин масштабируемого набора занимает несколько минут.


## <a name="create-custom-script-extension-definition"></a>Создание определения для расширения настраиваемых скриптов
Azure PowerShell использует хэш-таблицу для хранения скачиваемых файлов и выполняемых команд. Ниже используется пример скрипта из репозитория GitHub. Сначала создается объект конфигурации следующим образом.

```azurepowershell-interactive
$customConfig = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}
```

Теперь примените расширение пользовательских скриптов с помощью командлета [Add-AzureRmVmssExtension](/powershell/module/AzureRM.Compute/Add-AzureRmVmssExtension). Ранее определенный объект конфигурации передается расширению. Обновите и запустите расширение на экземплярах виртуальных машин с помощью командлета [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).

```azurepowershell-interactive
# Get information about the scale set
$vmss = Get-AzureRmVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
  -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.9 `
  -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Каждый экземпляр виртуальной машины в масштабируемом наборе скачивает и запускает скрипт из репозитория GitHub. В более сложном примере можно установить несколько компонентов и файлов приложения. Если масштабируемый набор развернут, новые экземпляры виртуальных машин автоматически будут применять одно и то же расширение настраиваемых скриптов и устанавливать нужное приложение.


## <a name="test-your-scale-set"></a>Проверка масштабируемого набора
Чтобы изучить работу веб-сервера, получите общедоступный IP-адрес своей подсистемы балансировки нагрузки с помощью командлета [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Следующий пример кода получает IP-адрес, созданный в группе ресурсов *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Введите в браузер общедоступный IP-адрес подсистемы балансировки нагрузки. Подсистема балансировки нагрузки передаст запрос на один из экземпляров виртуальной машины, как показано в следующем примере:

![Базовая веб-страница IIS](media/tutorial-install-apps-powershell/running-iis.png)

Не закрывайте веб-браузер, чтобы увидеть обновленную версию на следующем шаге.


## <a name="update-app-deployment"></a>Обновление развертывания приложения
На протяжении жизненного цикла масштабируемого набора может понадобиться развернуть обновленную версию приложения. Расширение пользовательских скриптов позволяет ссылаться на обновленный скрипт развертывания и повторно применять расширение к масштабируемому набору. Когда на предыдущем шаге создавался масштабируемый набор, для параметра `-UpgradePolicyMode` было задано значение *Automatic*. Этот параметр позволяет экземплярам виртуальных машин в масштабируемом наборе автоматически обновлять и применять последнюю версию приложения.

Создайте определение конфигурации с именем *customConfigv2*. Это определение позволяет запустить обновленную версию *v2* скрипта установки приложения.

```azurepowershell-interactive
$customConfigv2 = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis-v2.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis-v2.ps1"
}
```

Примените конфигурацию расширения пользовательских скриптов к экземплярам виртуальных машин в своем масштабируемом наборе с помощью командлета [Add-AzureRmVmssExtension](/powershell/module/AzureRM.Compute/Add-AzureRmVmssExtension). Определение *customConfigv2* используется для применения обновленной версии приложения.

```azurepowershell-interactive
# Reapply the Custom Script Extension to install the updated website
$vmss = Add-AzureRmVmssExtension `
  -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.9 `
  -Setting $customConfigv2

# Update the scale set and reapply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Все экземпляры виртуальной машины в масштабируемом наборе автоматически обновляются до последней версии примера веб-страницы. Чтобы просмотреть обновленную версию, обновите веб-сайт в браузере:

![Обновленная веб-страница IIS](media/tutorial-install-apps-powershell/running-iis-updated.png)


## <a name="clean-up-resources"></a>Очистка ресурсов
Чтобы удалить масштабируемый набор и дополнительные ресурсы, удалите группу ресурсов и все входящие в нее ресурсы с помощью команды [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). Параметр `-Force` подтверждает, что вы хотите удалить ресурсы без дополнительного запроса. При использовании параметра `-AsJob` управление возвращается в командную строку без ожидания завершения операции.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Дополнительная информация
Из этого руководства вы узнали, как автоматически устанавливать и обновлять приложения в масштабируемом наборе с помощью Azure PowerShell, в частности, как выполнять такие задачи:

> [!div class="checklist"]
> * автоматически устанавливать приложения в масштабируемый набор;
> * использовать расширения пользовательских скриптов;
> * обновлять приложение, работающее в масштабируемом наборе.

Перейдите к следующему руководству, чтобы узнать, как автоматически масштабировать масштабируемый набор.

> [!div class="nextstepaction"]
> [Автоматическое масштабирование масштабируемых наборов](tutorial-autoscale-powershell.md)
