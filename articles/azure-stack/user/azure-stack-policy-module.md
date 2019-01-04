---
title: Использование модуля политики Azure Stack | Документация Майкрософт
description: Сведения о том,как ограничить подписку Azure, чтобы ее поведение было аналогично подписке Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: sethm
ms.openlocfilehash: b0236a790200feec7f1d16724f351882056b2cd5
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2018
ms.locfileid: "52678531"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Управление политикой Azure с использованием модуля политики Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Модуль политики Azure Stack позволяет настроить подписку Azure с такими же возможностями управления версиями и доступными службами, как и в Azure Stack. Модуль использует командлет [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) для создания политики Azure, которая ограничивает типы ресурсов и службы, доступные в подписке. Затем создайте назначение политики в соответствующей области с помощью командлета [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment). После настройки политики подписку Azure можно использовать для разработки приложений, предназначенных для Azure Stack.

## <a name="install-the-module"></a>Установка модуля

1. Установите необходимую версию модуля PowerShell AzureRM, как описано в шаге 1 руководства по [установке PowerShell для Azure Stack](azure-stack-powershell-install.md).
2. [Скачайте средства Azure Stack из GitHub](azure-stack-powershell-download.md).
3. [Настройте PowerShell для использования с Azure Stack](azure-stack-powershell-configure-user.md).
4. Импортируйте модуль AzureStack.Policy.psm1:

    ```PowerShell
    Import-Module .\Policy\AzureStack.Policy.psm1
    ```

## <a name="apply-policy-to-azure-subscription"></a>Применение политики к подписке Azure

Следующая команда позволяет применить используемую по умолчанию политику Azure Stack к подписке Azure. Перед выполнением этой команды замените `Azure Subscription Name` именем своей подписки Azure.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "Azure Subscription Name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Применение политики к группе ресурсов

Возможно, вам потребуется применить политики более детализировано. Например, у вас могут быть другие ресурсы, выполняющиеся в той же подписке. Областью действия приложения политики можно задать определенную группу ресурсов, которая позволит тестировать приложения для работы с Azure Stack с использованием ресурсов Azure. Перед выполнением этой команды замените `Azure Subscription Name` именем своей подписки Azure.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "Azure Subscription Name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName
```

## <a name="policy-in-action"></a>Политика в действии

После применения политики Azure при попытке развернуть ресурс, который запрещен этой политикой, возникает ошибка.

![Результат сбоя развертывания ресурса из-за ограничения политики](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Дополнительная информация

* [Развертывание шаблонов с помощью PowerShell](azure-stack-deploy-template-powershell.md)
* [Развертывание шаблонов с помощью интерфейса командной строки Azure](azure-stack-deploy-template-command-line.md)
* [Deploy templates in Azure Stack using Visual Studio](azure-stack-deploy-template-visual-studio.md) (Развертывание шаблонов в Azure Stack с помощью Visual Studio)
