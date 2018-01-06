---
title: "Использование удостоверения управляемой службы виртуальной машины Azure для входа"
description: "Пошаговая инструкция с примерами демонстрирует применение субъекта-службы MSI виртуальной машины Azure для входа и доступа к ресурсам из клиентского скрипта."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 6c9e3ce4bbe33d06af64d97e1455ec20902d0ff4
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Использование удостоверения управляемой службы (MSI) виртуальной машины Azure для входа 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]  
Эта статья содержит пример сценария PowerShell и интерфейс командной строки для входа в систему с помощью MSI участника-службы и рекомендации на важные подразделы, такие как обработка ошибок.

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Если вы планируете использовать примеры Azure PowerShell или CLI из этой статьи, убедитесь, что установлена последняя версия [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) или [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Во всех примерах скриптов в этой статье предполагается, что клиент командной строки выполняется на виртуальной машине с поддержкой MSI. Используйте функцию подключения виртуальной машины на портале Azure для удаленного подключения к своей виртуальной машине. Дополнительные сведения о включении MSI на виртуальной машине см. в статье [Настройка управляемого удостоверения службы (MSI) на виртуальной машине Azure с помощью портала Azure](msi-qs-configure-portal-windows-vm.md) или в одном из вариантов статей (с использованием PowerShell, CLI, шаблона или пакета SDK для Azure). 
> - Чтобы избежать ошибок во время доступа к ресурсам, для MSI виртуальной машины необходимо предоставить по меньшей мере роль "Читатель" в соответствующей области (на уровне виртуальной машины или выше), то есть разрешить операции Azure Resource Manager на виртуальной машине. Дополнительные сведения см. в статье [Назначение доступа на основе управляемого удостоверения службы (MSI) для ресурса с помощью портала Azure](msi-howto-assign-access-portal.md).

## <a name="overview"></a>Обзор

Удостоверение MSI предоставляет [субъект-службу](develop/active-directory-dev-glossary.md#service-principal-object), который [создается при включении MSI](msi-overview.md#how-does-it-work) на виртуальной Машине. Субъекту-службе можно предоставить доступ к определенным ресурсам Azure и использовать его в качестве удостоверения для входа и использования ресурсов из скриптов и клиентов командной строки. Чтобы клиентский скрипт мог обращаться к защищенным ресурсам с собственным удостоверением, обычно нужно выполнить следующие условия:  

   - зарегистрировать скрипт в качестве конфиденциального клиента приложения или приложения веб-клиента в Azure AD и получить соответствующее согласие;
   - выполнить вход из скрипта с соответствующим субъектом-службой, используя учетные данные приложения (обычно они встраиваются в скрипт).

Благодаря MSI это больше не требуется. Скрипт может просто выполнить вход от имени субъекта-службы MSI. 

## <a name="azure-cli"></a>Инфраструктура CLI Azure

Следующий сценарий демонстрирует, как:

1. войти в Azure AD с помощью субъекта-службы MSI виртуальной машины;  
2. вызвать Azure Resource Manager и получить идентификатор субъекта-службы виртуальной машины. CLI автоматически управляет получением и использованием маркера. Не забудьте указать правильное имя виртуальной машины вместо `<VM-NAME>`.  

   ```azurecli
   az login --msi
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Следующий сценарий демонстрирует, как:

1. получить маркер доступа MSI для виртуальной машины;  
2. использовать маркер доступа для входа в Azure AD от имени соответствующего субъекта-службы MSI;   
3. вызвать командлет Azure Resource Manager для получения сведений о виртуальной машине. PowerShell автоматически управляет использованием маркера.  

   ```azurepowershell
   # Get an access token for the MSI
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                                 -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
   $content =$response.Content | ConvertFrom-Json
   $access_token = $content.access_token
   echo "The MSI access token is $access_token"

   # Use the access token to sign in under the MSI service principal. -AccountID can be any string to identify the session.
   Login-AzureRmAccount -AccessToken $access_token -AccountId "MSI@50342"

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Идентификаторы ресурсов для служб Azure

Список ресурсов, которые поддерживают Azure AD и были протестированы с MSI и соответствующими идентификаторами ресурсов, см. в разделе [Службы Azure, поддерживающие аутентификацию Azure AD](msi-overview.md#azure-services-that-support-azure-ad-authentication).

## <a name="error-handling-guidance"></a>Руководство по обработке ошибок 

Такие ответы могут обозначать, что MSI виртуальной машины настроен неправильно:

- PowerShell: *Invoke-WebRequest : Unable to connect to the remote server* (Invoke-WebRequest: Unable to connect to the remote server)
- CLI: *MSI: Failed to retrieve a token from 'http://localhost:50342/oauth2/token' with an error of 'HTTPConnectionPool(host='localhost', port=50342)* (Не удалось получить маркер от http://localhost:50342/oauth2/token, получена ошибка HTTPConnectionPool(host='localhost', port=50342)). 

Если вы встретите любую из этих ошибок, найдите виртуальную машину Azure на [портале Azure](https://portal.azure.com) и выполните на ней следующие действия:

- Откройте страницу **Конфигурация** и убедитесь, что для параметра "Удостоверение управляемой службы" задано значение "Да".
- Откройте страницу **Расширения** и убедитесь, что расширение MSI успешно развернуто.

Если какое-либо из условий не выполнено, может потребоваться повторно развернуть MSI для ресурса или устранить неполадки развертывания. Если вам нужна помощь при конфигурации виртуальной машины, см. статью [Настройка управляемого удостоверения службы (MSI) на виртуальной машине Azure с помощью портала Azure](msi-qs-configure-portal-windows-vm.md).

## <a name="related-content"></a>Связанная информация

- Как включить MSI для виртуальной машины Azure, можно узнать из статей [Настройка управляемого удостоверения службы (MSI) на виртуальной машине Azure с помощью PowerShell](msi-qs-configure-powershell-windows-vm.md) и [Настройка управляемого удостоверения службы (MSI) на виртуальной машине Azure с помощью Azure CLI](msi-qs-configure-cli-windows-vm.md).

Оставляйте свои замечания и пожелания в разделе ниже. Они помогают нам улучшать содержимое веб-сайта.








