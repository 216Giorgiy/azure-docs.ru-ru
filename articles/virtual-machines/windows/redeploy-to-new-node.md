---
title: "Повторное развертывание виртуальных машин Windows в Azure | Документация Майкрософт"
description: "Повторное развертывание виртуальных машин Windows в Azure для устранения проблем с подключением к удаленному рабочему столу."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 5002010e58a5d8e901770c780f07f9bd625d5eb4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2018
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Повторное развертывание виртуальной машины Windows на новом узле Azure
Если вам не удается подключиться к удаленному рабочему столу или получить доступ к приложению на виртуальной машине Windows Azure, можно попробовать повторно развернуть виртуальную машину. При повторном развертывании виртуальная машина перемещается на новый узел в рамках инфраструктуры Azure. Там она снова включается с сохранением всех параметров конфигурации и связанных ресурсов. В этой статье показано, как повторно развернуть виртуальную машину с помощью Azure PowerShell или портала Azure.

> [!NOTE]
> После развертывания временный диск будет удален, а связанные с виртуальным сетевым интерфейсом динамические IP-адреса будут обновлены. 


## <a name="using-azure-powershell"></a>Использование Azure PowerShell
Убедитесь, что на компьютере установлена последняя версия Azure PowerShell 1.x. Подробнее: [Установка и настройка Azure PowerShell](/powershell/azure/overview).

В следующем примере развертывается виртуальная машина `myVM` в группе ресурсов `myResourceGroup`.

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Дальнейшие действия
При проблемах с подключением к виртуальной машине ознакомьтесь со статьями [Устранение неполадок с подключением к удаленному рабочему столу на виртуальной машине Azure под управлением Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) и [Подробная диагностика подключений к удаленному рабочему столу виртуальной машины Azure под управлением Windows](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). При проблемах с доступом к приложению, выполняющемуся на виртуальной машине, ознакомьтесь со статьей [Устранение неполадок доступа к приложению, выполняющемуся в виртуальной машине Azure](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

