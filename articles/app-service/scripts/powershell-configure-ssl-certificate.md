---
title: Пример сценария Azure PowerShell. Привязка пользовательского SSL-сертификата к веб-приложению | Документация Майкрософт
description: Пример сценария Azure PowerShell для привязки пользовательского SSL-сертификата к веб-приложению.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c13feb22cc02d1a444de412e75e5555645f35b88
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2018
ms.locfileid: "53586924"
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app-using-powershell"></a>Привязка SSL-сертификата к веб-приложению с помощью PowerShell

Этот сценарий создает в службе приложений веб-приложение со связанными ресурсами, а затем привязывает к нему SSL-сертификат имени личного домена. 

При необходимости установите Azure PowerShell с помощью инструкции, приведенной в [руководстве Azure PowerShell](/powershell/azure/overview), а затем выполните команду `Connect-AzureRmAccount`, чтобы создать подключение к Azure. Кроме того, убедитесь в следующем.

- Подключение к Azure установлено с помощью команды `az login`.
- У вас есть доступ к странице конфигурации DNS вашего регистратора доменных имен.
- У вас есть допустимый PFX-файл и пароль для SSL-сертификата, который требуется передать и привязать.

## <a name="sample-script"></a>Пример скрипта

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполнив пример сценария, вы можете удалить группу ресурсов, веб-приложение и все связанные ресурсы, используя следующую команду.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Создает план службы приложений. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Создает веб-приложение. |
| [Set-AzureRmAppServicePlan](/powershell/module/azurerm.websites/set-azurermappserviceplan) | Обновляет план службы приложений для изменения ценовой категории. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Изменяет конфигурацию веб-приложения. |
| [New-AzureRmWebAppSSLBinding](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | Создает привязку SSL-сертификата к веб-приложению. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев Azure PowerShell для веб-приложений службы приложений Azure доступны [здесь](../samples-powershell.md).
