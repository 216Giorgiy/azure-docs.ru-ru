---
title: Создание виртуальной машины из управляемого образа в Azure | Документация Майкрософт
description: Создание виртуальной машины Windows из универсального управляемого образа с использованием Azure PowerShell или портала Azure в модели развертывания с помощью Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 9157765afaa610d207a47e19b73f80ae3898fd68
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977564"
---
# <a name="create-a-vm-from-a-managed-image"></a>Создание виртуальной машины из управляемого образа

Вы можете создать несколько виртуальных машин из управляемого образа виртуальной машины Azure, используя портал Azure или PowerShell. Управляемый образ виртуальной машины содержит сведения, необходимые для создания виртуальной машины, включая диски ОС и диски данных. Виртуальные жесткие диски (VHD), входящие в образ, включая диски ОС и диски данных, хранятся в виде управляемых дисков. 

Перед созданием виртуальной машины необходимо [создать управляемый образ виртуальной машины](capture-image-resource.md) для использования в качестве исходного образа. 


## <a name="use-the-portal"></a>Использование портала

1. Откройте [портал Azure](https://portal.azure.com).
2. В меню слева выберите **Все ресурсы**. Вы можете отсортировать ресурсы по **типу**, чтобы легко найти образы.
3. Выберите в списке необходимый образ. Откроется страница **Обзор** для образа.
4. В меню выберите **Создать виртуальную машину**.
5. Введите сведения о виртуальной машине. Имя пользователя и пароль понадобятся для входа на виртуальную машину. Когда все будет готово, нажмите кнопку **ОК**. Вы можете создать виртуальную машину в существующей группе ресурсов или выбрать **Создать**, чтобы создать группу ресурсов для хранения данных виртуальной машины.
6. Выберите размер виртуальной машины. Чтобы просмотреть дополнительные размеры, выберите **Просмотреть все** или измените фильтр **Supported disk type** (Поддерживаемые типы диска). 
7. Внесите необходимые изменения в разделе **Параметры** и щелкните **ОК**. 
8. На странице сводных данных вы увидите имя образа в строке **Частный образ**. Щелкните **ОК**, чтобы запустить развертывание виртуальной машины.


## <a name="use-powershell"></a>Использование PowerShell

С помощью PowerShell вы можете создать виртуальную машину из образа, используя упрощенный набор параметров для командлета [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). Образ должен находиться в той же группе ресурсов, где будет создана виртуальная машина.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

Для создания виртуальной машины из образа в упрощенном наборе параметров для [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) нужно указать только имя, группу ресурсов и имя образа. В качестве имени всех ресурсов, которые создаются автоматически, New-AzVm будет использовать значение параметра **-Name**. В этом примере мы предоставим более подробные имена для каждого из ресурсов, которые автоматически создаются при помощи командлета. Также можно создать такие ресурсы, как виртуальная сеть, заранее и передать соответствующее имя ресурса в командлет. New-AzVm будет использовать существующие ресурсы, если их можно найти по имени.

В следующем примере на основе образа с именем *myImage* создается виртуальная машина с именем *myVMFromImage* в группе *myResourceGroup*. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Дополнительная информация
[Руководство. Создание и администрирование виртуальных машин Windows с помощью Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

