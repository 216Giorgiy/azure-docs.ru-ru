---
title: "Сброс пароля или конфигурации удаленного рабочего стола для виртуальной машины Windows | Документация Майкрософт"
description: "Узнайте, как сбросить пароль учетной записи или конфигурацию службы удаленного рабочего стола для виртуальной машины Windows с помощью портала Azure или Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: e6cb3a0e259d0889ad8934211152e6832449f67b
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Сброс службы удаленных рабочих столов или ее пароля для входа в систему на виртуальной машине под управлением Windows
Если не удается подключиться к виртуальной машине Windows, можно сбросить пароль локального администратора или конфигурацию службы удаленного рабочего стола. Для сброса пароля можно использовать портал Azure или расширение VMAccess в Azure PowerShell. Если вы используете PowerShell, то убедитесь, что у вас установлен [и настроен последний модуль PowerShell](/powershell/azure/overview) и вы вошли в подписку Azure. Вы также можете [выполнить эти действия для виртуальных машин, созданных с использованием классической модели](reset-rdp.md).

## <a name="ways-to-reset-configuration-or-credentials"></a>Способы сброса конфигурации или учетных данных
В зависимости от ваших потребностей службы удаленных рабочих столов и учетные данные можно сбросить несколькими разными способами:

- [сброс с помощью портала Azure](#azure-portal);
- [сброс с помощью Azure PowerShell](#vmaccess-extension-and-powershell).

## <a name="azure-portal"></a>Портал Azure
Чтобы развернуть меню портала, щелкните три полосы в левом верхнем углу и выберите **Виртуальные машины**.

![Выбор виртуальной машины Azure](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Сброс пароля учетной записи локального администратора**

Выберите виртуальную машину Windows, а затем щелкните **Поддержка и устранение неполадок** > **Сбросить пароль**. Отобразится колонка сброса пароля.

![Страница "Сброс пароля"](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Введите имя пользователя и новый пароль, а затем щелкните **Обновить**. Снова попробуйте подключиться к виртуальной машине.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Сброс конфигурации службы удаленных рабочих столов**

Выберите виртуальную машину Windows, а затем щелкните **Поддержка и устранение неполадок** > **Сбросить пароль**. Отобразится колонка сброса пароля. 

![Сброс конфигурации удаленного рабочего стола](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Выберите **Сбросить только конфигурацию** из раскрывающегося меню и щелкните **Обновить**. Снова попробуйте подключиться к виртуальной машине.


## <a name="vmaccess-extension-and-powershell"></a>Расширение VMAccess и PowerShell
Убедитесь, что у вас [установлен и настроен последний модуль PowerShell](/powershell/azure/overview) и вы вошли в подписку Azure, выполнив командлет `Login-AzureRmAccount`.

### <a name="reset-the-local-administrator-account-password"></a>**Сброс пароля учетной записи локального администратора**
Сбросьте пароль или имя пользователя администратора, выполнив командлет PowerShell [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension). Создайте учетные данные следующим образом.

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Если вы укажете не текущую учетную запись локального администратора виртуальной машины, а другое имя, то расширение VMAccess переименует учетную запись локального администратора, назначит ей указанный пароль и создаст событие закрытия удаленного рабочего стола. Если учетная запись локального администратора на виртуальной машине отключена, расширение VMAccess включит ее.

Следующий пример обновляет виртуальную машину `myVM` в группе ресурсов `myResourceGroup` и задает для нее указанные учетные данные.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a>**Сброс конфигурации службы удаленных рабочих столов**
Сбросьте параметры удаленного доступа к виртуальной машине с помощью командлета PowerShell [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension). Следующий пример сбрасывает параметры расширения доступа `myVMAccess` на виртуальной машине `myVM` в группе ресурсов `myResourceGroup`.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> В любой момент на виртуальной машине может быть только один агент доступа. Чтобы успешно задать свойства агента доступа к виртуальной машине, можно использовать параметр `-ForceRerun`. Если вы используете параметр `-ForceRerun`, то укажите для агента доступа то же имя, что было задано в предыдущей команде.

Если по-прежнему не удается удаленно подключиться к виртуальной машине, ознакомьтесь со статьей [Устранение неполадок с подключением к удаленному рабочему столу на виртуальной машине Azure под управлением Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Дальнейшие действия
Если расширение доступа к виртуальной машине Azure не отвечает и сбросить пароль не удается, вы можете [сбросить локальный пароль Windows в автономном режиме](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Этот метод является более сложным процессом и требует подключения виртуального жесткого диска проблемной виртуальной машины к другой виртуальной машине. Сначала выполните действия, описанные в этой статье, и только потом попробуйте сбросить пароль в автономном режиме в качестве последнего средства.

[Расширения и компоненты виртуальных машин Azure](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Подключение к виртуальной машине Azure с помощью RDP или SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Устранение неполадок с подключением к удаленному рабочему столу виртуальной машины Windows в службе Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


