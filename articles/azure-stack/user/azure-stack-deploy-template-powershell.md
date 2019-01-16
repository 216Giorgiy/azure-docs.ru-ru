---
title: Развертывание шаблонов в Azure Stack с помощью PowerShell | Документация Майкрософт
description: Развертывание шаблонов в Azure Stack с помощью PowerShell.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 76348ef6984585856565ff39bfa707f1319477bb
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107219"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Развертывание шаблонов в Azure Stack с помощью PowerShell

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

С помощью PowerShell можно развертывать шаблоны Azure Resource Manager в Azure Stack. В этой статье показано, как использовать PowerShell для развертывания шаблона.

## <a name="run-azurerm-powershell-cmdlets"></a>Выполнение командлетов PowerShell для AzureRM

Этот пример использует командлеты PowerShell для **AzureRM** и шаблон, хранящийся на сайте GitHub. Этот шаблон создает виртуальную машину Windows Server 2012 R2 Datacenter.

>[!NOTE]
>Прежде чем выполнять этот пример, убедитесь, что вы [настроили PowerShell](azure-stack-powershell-configure-user.md) для пользователя Azure Stack.

1. Перейдите по адресу [https://aka.ms/AzureStackGitHub](https://aka.ms/AzureStackGitHub) и найдите шаблон **101-simple-windows-vm**. Сохраните шаблон в этом расположении: `C:\templates\azuredeploy-101-simple-windows-vm.json`.
2. Откройте командную строку PowerShell с повышенными привилегиями.
3. Замените `username` и `password` в приведенном ниже скрипте своим именем пользователя и паролем, а затем запустите скрипт.

    ```PowerShell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
    $RGName = "myRG$myNum"
    $myLocation = "local"
   
    # Create resource group for template deployment
    New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
    # Deploy simple IaaS template
    New-AzureRmResourceGroupDeployment `
        -Name myDeployment$myNum `
        -ResourceGroupName $RGName `
        -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
        -NewStorageAccountName mystorage$myNum `
        -DnsNameForPublicIP mydns$myNum `
        -AdminUsername <username> `
        -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
        -VmName myVM$myNum `
        -WindowsOSVersion 2012-R2-Datacenter
    ```

    >[!IMPORTANT]
    >При каждом последующем запуске этого сценария увеличивайте значение параметра `$myNum`, чтобы избежать перезаписи развертывания.

4. Откройте портал Azure Stack, выберите **Обзор** > **Виртуальные машины** и найдите свою новую виртуальную машину (**myDeployment001**).

## <a name="next-steps"></a>Дополнительная информация

- [Развертывание шаблонов с помощью Visual Studio](azure-stack-deploy-template-visual-studio.md)
