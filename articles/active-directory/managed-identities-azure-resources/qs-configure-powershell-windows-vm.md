---
title: Как настроить управляемые удостоверения для ресурсов Azure на виртуальной машине Azure с помощью PowerShell
description: Пошаговые инструкции по настройке управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью Azure.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: priyamo
ms.openlocfilehash: a92e543ea8a633d4d7dfd1b276fadc0224696153
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169983"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Функция управляемых удостоверений для ресурсов Azure предоставляет службам Azure автоматически управляемые удостоверения в Azure Active Directory. Это удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, не храня какие-либо учетные данные в коде. 

В этой статье вы узнаете, как с помощью PowerShell выполнять приведенные ниже операции с управляемыми удостоверениями для ресурсов Azure на виртуальной машине Azure.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Предварительные требования

- Если вы не работали с управляемыми удостоверениями для ресурсов Azure, изучите [общие сведения](overview.md). **Обратите внимание на [различие между управляемыми удостоверениями, назначаемыми системой и назначаемыми пользователями](overview.md#how-does-it-work)**.
- Если у вас нет учетной записи Azure, [зарегистрируйтесь для получения бесплатной пробной учетной записи](https://azure.microsoft.com/free/), прежде чем продолжать.
- Установите [последнюю версию Azure PowerShell](/powershell/azure/install-az-ps), если это еще не сделано.

## <a name="system-assigned-managed-identity"></a>Управляемое удостоверение, назначаемое системой

В этом разделе вы узнаете, как включить и отключить управляемое удостоверение, назначаемое системой, с помощью Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Включение управляемого удостоверения, назначаемого системой, во время создания виртуальной машины Azure

Чтобы создать виртуальную машину Azure с включенным управляемым удостоверением, назначаемым системой, вашей учетной записи должна быть назначена роль [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor).  Назначать другие роли в каталоге Azure AD не требуется.

1. Используя одно из кратких руководств ниже выполните действия только в нужных разделах ("Вход в Azure", "Создание группы ресурсов", "Создание группы сети", "Создание виртуальной машины").
    
    При выполнении действий, описанных в разделе о создании виртуальной машины, не забудьте внести небольшие изменения в синтаксис командлета [New-AzVMConfig](/powershell/module/az.compute/new-azvm). Чтобы подготовить виртуальную машину с включенным удостоверением, назначаемым системой, добавьте параметр `-AssignIdentity:$SystemAssigned`, как показано ниже.
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Создание виртуальной машины Windows с помощью PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Создание виртуальной машины Linux с помощью PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Необязательно.) Добавьте расширение виртуальной машины для управляемых удостоверений для ресурсов Azure (вывод из эксплуатации запланирован на январь 2019 года), выполнив командлет [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) с параметром `-Type`. Вы можете передать значения ManagedIdentityExtensionForWindows или ManagedIdentityExtensionForLinux (в зависимости от типа виртуальной машины) и задать имя с помощью параметра `-Name`. В параметре `-Settings` указан порт, используемый конечной точкой токена OAuth для получения токена:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Этот шаг необязателен, так как для получения токенов можно также использовать конечную точку службы метаданных экземпляров Azure (IMDS). Расширение виртуальной машины для управляемых удостоверений для ресурсов Azure будет выведено из эксплуатации в январе 2019 года. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Включение управляемого удостоверения, назначаемого системой, для имеющейся виртуальной машины Azure

Чтобы включить назначаемое системой управляемое удостоверение на виртуальной машине, которая была подготовлена без него, вашей учетной записи должна быть назначена роль [Участника виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor).  Назначать другие роли в каталоге Azure AD не требуется.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит виртуальную машину.

   ```powershell
   Connect-AzAccount
   ```

2. Сначала извлеките свойства виртуальной машины с помощью командлета `Get-AzVM`. Затем, чтобы включить управляемое удостоверение, назначаемое системой, используйте параметр `-AssignIdentity` в командлете [Update-AzVM](/powershell/module/az.compute/update-azvm).

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Необязательно.) Добавьте расширение виртуальной машины для управляемых удостоверений для ресурсов Azure (вывод из эксплуатации запланирован на январь 2019 года), выполнив командлет [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) с параметром `-Type`. Вы можете передать значения ManagedIdentityExtensionForWindows или ManagedIdentityExtensionForLinux (в зависимости от типа виртуальной машины) и задать имя с помощью параметра `-Name`. В параметре `-Settings` указан порт, используемый конечной точкой токена OAuth для получения токена. Убедитесь, что указан правильный параметр `-Location`, совпадающий с расположением имеющейся виртуальной машины:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Этот шаг необязателен, так как для получения токенов можно также использовать конечную точку службы метаданных экземпляров Azure (IMDS).

### <a name="add-vm-system-assigned-identity-to-a-group"></a>Добавление назначаемого системой удостоверения виртуальной машины в группу

После включения назначенного системной удостоверения на виртуальной машине, вы можете добавить его в группу.  Следующая процедура добавляет назначенное системой удостоверение в группу.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит виртуальную машину.

   ```powershell
   Connect-AzAccount
   ```

2. Получите и запишите `ObjectID` (как указано в поле `Id` возвращаемых значений) субъекта-службы виртуальной машины.

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. Получите и запишите `ObjectID` (как указано в поле `Id` возвращаемых значений) группы.

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. Добавление субъекта-службы виртуальной машины в группу.

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Отключение управляемого удостоверения, назначаемого системой, на виртуальной машине Azure

Чтобы отключить назначаемое системой управляемое удостоверение на виртуальной машине, вашей учетной записи должна быть назначена роль [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor).  Назначать другие роли в каталоге Azure AD не требуется.

Если на виртуальной машине больше не требуется управляемое удостоверение, назначаемое системой, но по-прежнему требуются управляемые удостоверения, назначаемые пользователем, используйте следующий командлет.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит виртуальную машину.

   ```powershell
   Connect-AzAccount
   ```

2. Получите свойства виртуальной машины с помощью командлета `Get-AzVM` и задайте для параметра `-IdentityType` значение `UserAssigned`.

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Если имеется виртуальная машина, для которой не требуется управляемое удостоверение, назначаемое системой, и у которой нет управляемых удостоверений, назначаемых пользователем, используйте следующие команды.

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Чтобы удалить расширение виртуальной машины для управляемых удостоверений для ресурсов Azure, используйте параметр -Name в командлете [Remove-AzVMExtension](/powershell/module/az.compute/remove-azvmextension), указав то же имя, которое вы использовали при добавлении расширения.

   ```powershell
   Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>Управляемое удостоверение, назначаемое пользователем

В этом разделе вы узнаете, как добавить и удалить управляемое удостоверение, назначаемое пользователем, для виртуальной машины с помощью Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Задание управляемого удостоверения, назначаемого пользователем, для виртуальной машины во время ее создания

Чтобы присвоить назначаемое пользователем удостоверение виртуальной машине, вашей учетной записи должны быть назначены роли [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) и [Оператор управляемого удостоверения](/azure/role-based-access-control/built-in-roles#managed-identity-operator). Назначать другие роли в каталоге Azure AD не требуется.

1. Используя одно из кратких руководств ниже выполните действия только в нужных разделах ("Вход в Azure", "Создание группы ресурсов", "Создание группы сети", "Создание виртуальной машины"). 
  
    При выполнении действий, описанных в разделе о создании виртуальной машины, не забудьте внести небольшие изменения в синтаксис командлета [`New-AzVMConfig`](/powershell/module/az.compute/new-azvm). Добавьте параметры `-IdentityType UserAssigned` и `-IdentityID `, чтобы подготовить виртуальную машину с удостоверением, назначаемым пользователем.  Замените `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>` и `<USER ASSIGNED IDENTITY NAME>` собственными значениями.  Например: 
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Создание виртуальной машины Windows с помощью PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Создание виртуальной машины Linux с помощью PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Необязательно.) Добавьте расширение виртуальной машины для управляемых удостоверений для ресурсов Azure, используя параметр `-Type` в командлете [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension). Вы можете передать значения ManagedIdentityExtensionForWindows или ManagedIdentityExtensionForLinux (в зависимости от типа виртуальной машины) и задать имя с помощью параметра `-Name`. В параметре `-Settings` указан порт, используемый конечной точкой токена OAuth для получения токена. Убедитесь, что указан правильный параметр `-Location`, совпадающий с расположением имеющейся виртуальной машины:
      > [!NOTE]
    > Этот шаг необязателен, так как для получения токенов можно также использовать конечную точку службы метаданных экземпляров Azure (IMDS). Расширение виртуальной машины для управляемых удостоверений для ресурсов Azure будет выведено из эксплуатации в январе 2019 года.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Задание управляемого удостоверения, назначаемого пользователем, для имеющейся виртуальной машины Azure

Чтобы присвоить назначаемое пользователем удостоверение виртуальной машине, вашей учетной записи должны быть назначены роли [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) и [Оператор управляемого удостоверения](/azure/role-based-access-control/built-in-roles#managed-identity-operator). Назначать другие роли в каталоге Azure AD не требуется.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит виртуальную машину.

   ```powershell
   Connect-AzAccount
   ```

2. Создайте управляемое удостоверение, назначаемое пользователем, с помощью командлета [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity).  Обратите внимание на свойство `Id` в выходных данных. Оно потребуется в следующем шаге.

   > [!IMPORTANT]
   > При создании управляемых удостоверений, назначаемых пользователем, можно использовать только буквы, цифры и дефис (0–9, a–z, A–Z и -). Кроме того, чтобы назначение виртуальной машине или VMSS производилось правильно, длина имени не должна превышать 24 символа. Загляните сюда позже, чтобы проверить наличие новой информации. Дополнительные сведения см. в разделе [Часто задаваемые вопросы и известные проблемы](known-issues.md).

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Извлеките свойства виртуальной машины с помощью командлета `Get-AzVM`. Затем, чтобы задать управляемое удостоверение, назначаемое пользователем, для виртуальной машины Azure, используйте параметры `-IdentityType` и `-IdentityID` в командлете [Update-AzVM](/powershell/module/az.compute/update-azvm).  Для параметра `-IdentityId` следует использовать значение `Id` из предыдущего шага.  Замените `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>` и `<USER ASSIGNED IDENTITY NAME>` собственными значениями.

   > [!WARNING]
   > Чтобы сохранить ранее назначенные пользователем управляемые удостоверения, предназначенные для виртуальной машины, запросите свойство `Identity` объекта VM (например, `$vm.Identity`).  Если какие-либо назначенные пользователем управляемые удостоверения возвращаются, включите их в следующую команду вместе с новым удостоверением, которое необходимо назначить виртуальной машине.

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Добавьте расширение виртуальной машины для управляемых удостоверений для ресурсов Azure (вывод из эксплуатации запланирован на январь 2019 года), выполнив командлет [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension) с параметром `-Type`. Вы можете передать значения ManagedIdentityExtensionForWindows или ManagedIdentityExtensionForLinux (в зависимости от типа виртуальной машины) и задать имя с помощью параметра `-Name`. В параметре `-Settings` указан порт, используемый конечной точкой токена OAuth для получения токена. Укажите правильный параметр `-Location`, совпадающий с расположением имеющейся виртуальной машины.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Удаление управляемого удостоверения, назначаемого пользователем, из виртуальной машины Azure

Чтобы удалить назначаемое пользователем удостоверение на виртуальной машине, вашей учетной записи должна быть назначена роль [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor).

Если у виртуальной машины несколько управляемых удостоверений, назначаемых пользователем, с помощью приведенных ниже команд можно удалить все эти удостоверения, кроме последнего. Не забудьте заменить значения параметров `<RESOURCE GROUP>` и `<VM NAME>` собственными. `<USER ASSIGNED IDENTITY NAME>` — это имя управляемого удостоверения, назначаемого пользователем, которое следует оставить в виртуальной машине. Эти сведения можно найти, запросив свойство `Identity` объекта виртуальной машины.  Например, `$vm.Identity`:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Если в виртуальной машине нет управляемого удостоверения, назначаемого системой, и вы хотите удалить из нее все управляемые удостоверения, назначаемые пользователем, используйте следующую команду.

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Если у виртуальной машины есть управляемые удостоверения, назначаемые системой и назначаемые пользователем, вы можете удалить все управляемые удостоверения, назначаемые пользователем, переключившись на использование только управляемого удостоверения, назначаемого системой.

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Дополнительная информация

- [Обзор управляемых удостоверений для ресурсов Azure](overview.md).
- Ниже приведены комплексные краткие руководства по созданию виртуальных машин Azure:
  
  - [Создание виртуальной машины Windows с помощью PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Создание виртуальной машины Linux с помощью PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
