---
title: Применение Azure CLI для пользователей Azure Stack | Документация Майкрософт
description: Узнайте, как использовать межплатформенный интерфейс командной строки (CLI) для развертывания ресурсов и управления ими в Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: c2827a4badd61aeb8de556795834dee39769e85e
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554509"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Применение Azure CLI для пользователей Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Корневой сертификат ЦС можно предоставить пользователям Azure Stack, чтобы они могли использовать Azure CLI на своих виртуальных машинах для разработки. Этот сертификат понадобится пользователям для управления ресурсами с помощью CLI.

* **Корневой сертификат ЦС Azure Stack.** Он нужен, если CLI используется на рабочей станции без Пакета средств разработки Azure Stack.  

* **Конечная точка псевдонимов виртуальных машин.** Она предоставляет псевдонимы, например UbuntuLTS или Win2012Datacenter, которые позволяют при развертывании виртуальной машины в одном параметре указать сведения об издателе образа, предложении, номере SKU и версии.  

В разделах ниже описано, как получить эти значения.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Экспорт корневого сертификата ЦС Azure Stack

Корневой сертификат ЦС Azure Stack можно найти в пакете SDK и на виртуальной машине клиента, где выполняется среда комплекта разработки. Чтобы экспортировать корневой сертификат Azure Stack в PEM-формате, войдите в пакет SDK или на виртуальную машину клиента и выполните следующий скрипт:

```powershell
$label = "<Your Azure Stack CA root certificate name>"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Настройка конечной точки псевдонимов для виртуальных машин

Операторам Azure Stack следует настроить общедоступную конечную точку с файлом псевдонимов виртуальных машин. В файле псевдонимов виртуальных машин хранится общее имя образа в JSON-формате. Имя можно указать в соответствующем параметре команды Azure CLI при развертывании виртуальной машины.  

Прежде чем добавить запись в файл псевдонимов, [скачайте образы из Azure Marketplace](azure-stack-download-azure-marketplace-item.md) или [опубликуйте собственный пользовательский образ](azure-stack-add-vm-image.md). Когда вы публикуете пользовательский образ, запишите сведения об издателе, предложении, номере SKU и версии, которые указываете во время публикации. Если используется образ из Marketplace, эти сведения можно получить с помощью командлета ```Get-AzureVMImage```.  

Мы предлагаем [образец файла псевдонимов](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) с несколькими широко используемыми псевдонимами образов. Вы можете использовать его в качестве отправной точки. Разместите этот файл в расположении, доступном для всех клиентов, которые используют CLI. Например, его можно поместить в учетную запись хранилища BLOB-объектов, а пользователям предоставить соответствующий URL-адрес:

1. Скачайте [образец файла](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) с сайта GitHub.
2. Создайте учетную записи хранения в Azure Stack. Затем создайте контейнер BLOB-объектов. Установите для политики доступа значение public.  
3. Отправьте JSON-файл в только что созданный контейнер. Теперь щелкните имя BLOB-объекта и выберите URL-адрес в разделе его свойств.

## <a name="next-steps"></a>Дополнительная информация

- [Развертывание шаблонов с помощью интерфейса командной строки Azure](azure-stack-deploy-template-command-line.md)
- [Подключение с помощью PowerShell](azure-stack-connect-powershell.md)
- [Управление разрешениями пользователей](azure-stack-manage-permissions.md)
