---
title: Просмотр сведений об использовании LUIS в Azure Cloud Shell | Документация Майкрософт
description: Узнайте, как получить сведения об использовании LUIS в Azure Cloud Shell.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2017
ms.author: v-geberr
ms.openlocfilehash: 2de25645e5377efdd53bcc980695804d34db5ee2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382136"
---
# <a name="manage-luis-service-from-azure-cloud-shell"></a>Управление службой LUIS из Azure Cloud Shell
С помощью портала Azure можно использовать командлеты PowerShell для работы с ресурсами LUIS. 

Эти командлеты позволяют [создать](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) подписку LUIS, получить сведения о подписке, в том числе об [использовании](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), а также [удалить](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) подписку. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Учетная запись хранения Cloud Shell и аутентификация
Чтобы использовать PowerShell в [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell) на портале Azure, вам нужна учетная запись хранения Azure. Если у вас нет [учетной записи хранения](https://docs.microsoft.com/en-us/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share), вам будет предложено ее создать. Учетная запись хранения позволяет сохранять скрипты PowerShell в Cloud Shell.  

Также для доступа к любым ресурсам необходимо выполнить аутентификацию Azure в Cloud Shell. 

После получения учетной записи хранения и аутентификации вы можете запускать командлеты PowerShell.

## <a name="open-cloud-shell"></a>Открытие Cloud Shell
Для работы с Cloud Shell на портале Azure всегда используется самая последняя версия PowerShell. 

Чтобы открыть Cloud Shell, воспользуйтесь кнопкой **Запустить Cloud Shell** или откройте в браузере [https://shell.azure.com](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>Сведения об использовании конечной точки LUIS

Командлет PowerShell 6.x `Get-AzureRmCognitiveServicesAccountUsage` предоставляет сведения об использовании служб Microsoft Cognitive Services, включая LUIS. Для [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) требуется указать группу ресурсов и имя ресурса службы. 

Синтаксис команды:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

В следующем примере именем группы ресурсов является `luis-westus-rg`, а именем подписки на службу LUIS — `luis-westus-1`. Оба этих имени выбираются при создании службы LUIS. 

Командлет возвращает сведения об использовании 16 из 10 000 попаданий конечной точки, использованных в 30-дневный период, заканчивающийся 7 июня:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

Сохраните команду в виде файла PowerShell *.ps1 в учетной записи хранения Azure, связанной с Cloud Shell. Ее можно будет выполнить в любое время. 

![Запуск скрипта из хранилища](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

После сохранения скрипта на облачном диске можно выполнить скрипт PowerShell из Cloud Shell в приложении Azure для телефона. 

![Запуск скрипта из хранилища в приложении для телефона](./media/luis-how-to-manage-from-powershell/phone-app.png)