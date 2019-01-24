---
title: Выбор образов виртуальных машин Windows в Azure | Документация Майкрософт
description: Вы можете использовать Azure PowerShell для определения издателя, предложения, номера SKU и версии для образов виртуальных машин из Marketplace.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/08/2018
ms.author: danlep
ms.openlocfilehash: ff4ccdf28be9d28798fff0e9f66bbb2c860166b7
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424546"
---
# <a name="find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Поиск образов виртуальных машин Windows в Azure Marketplace с помощью Azure PowerShell

В этой статье описывается, как с помощью Azure PowerShell находить образы виртуальных машин в Azure Marketplace. При создании виртуальной машины программными средствами с помощью PowerShell, шаблонов Resource Manager или других средств вы можете указать образ в Marketplace.

Вы также можете просмотреть доступные образы и предложения в онлайн-магазине [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/), на [портале Azure](https://portal.azure.com) или с помощью [Azure CLI](../linux/cli-ps-findimage.md). 

Убедитесь, что у вас установлена и настроена последняя версия [модуля Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Таблица часто используемых образов Windows
| ИЗДАТЕЛЬ | ПРЕДЛОЖЕНИЕ | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-with-Containers |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2017-WS2016 |Enterprise |
| MicrosoftRServer |RServer-WS2016 |Enterprise |

## <a name="navigate-the-images"></a>Переход к образам

Один из способов поиска образа в расположении основан на запуске командлетов [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) и [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) по порядку.

1. Получить список издателей образов.
2. Получить список предложений нужного издателя.
3. Получить список номеров SKU для требуемого предложения.

Затем, чтобы отобразить версии для развертывания, выполните команду [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) для выбранного SKU.

1. Отобразите список издателей:

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
    ```

2. Укажите название выбранного издателя и выведите список предложений:

    ```powershell
    $pubName="<publisher>"
    Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
    ```

3. Укажите название выбранного предложения и выведите список SKU:

    ```powershell
    $offerName="<offer>"
    Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
    ```
    
4. Укажите название выбранного SKU и получите версию образа:

    ```powershell
    $skuName="<SKU>"
    Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
Версию образа для развертывания новой виртуальной машины можно выбрать из выходных данных команды `Get-AzureRMVMImage`.

В следующем примере приведена полная последовательность команд и их выходных данных:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Частичные выходные данные приведены ниже.

```
PublisherName
-------------
...
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Для издателя *MicrosoftWindowsServer*:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Выходные данные:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

Для предложения *WindowsServer*:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Выходные данные:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
```

Затем для SKU *2016-Datacenter*:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Теперь выбранного издателя, предложение, номер SKU и версию можно объединить в URN (значения, разделенные ":"). Передайте этот URN с параметром `--image` при создании виртуальной машины с помощью командлета [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). При необходимости вы можете заменить номер версии в URN словом "latest" (последняя), чтобы получить последнюю версию образа.

При развертывании виртуальной машины с помощью шаблона Resource Manager установите отдельные параметры образа в свойствах `imageReference`. Ознакомьтесь со статьей о [справочнике по шаблонам](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Просмотр свойств плана

Чтобы просмотреть сведения о плане покупки образа, выполните командлет `Get-AzureRMVMImage`. Если в выходных данных значением свойства `PurchasePlan` не является `null`, в образе появятся условия использования, которые необходимо принять перед программным развертыванием.  

Например, образ *Windows Server 2016 Datacenter* не содержит дополнительные условия, так как в `PurchasePlan` указано значение `null`.

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Выходные данные:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

В примере ниже показана подобная команда для образа *Виртуальной машины для обработки и анализа данных в Windows 2016*, имеющего следующие свойства `PurchasePlan`: `name`, `product` и `publisher`. Некоторые образы также имеют свойство `promotion code`. Ознакомьтесь со следующими разделами для развертывания этого образа, чтобы принять условия соглашения и включить программное развертывание.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Выходные данные:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Принятие условий

Чтобы просмотреть условия лицензии, используйте командлет [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) и передайте параметры плана приобретения. В выходных данных содержится ссылка на условия для образа в Marketplace, которая показывает, были ли эти условия приняты раннее. Обязательно используйте только строчные буквы в значениях параметров.

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Выходные данные:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Чтобы принять или отклонить условия, используйте командлет [Set-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms). Необходимо принять условия соглашения для каждой подписки в образе. Обязательно используйте только строчные буквы в значениях параметров. 

```powershell
$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

Выходные данные:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : XXXXXXK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBXXXXXX
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Развертывание с помощью параметров плана приобретения

После принятия условий для образа виртуальную машину можно развернуть в подписке. В следующем фрагменте показано, как использовать командлет [Set-AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan), чтобы задать сведения о плане Marketplace для объекта виртуальной машины. Для завершения сценария по созданию параметров сети для виртуальной машины и завершения развертывания см. статью [Примеры PowerShell для виртуальной машины Azure](powershell-samples.md).

```powershell
...

$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information

$publisherName = "microsoft-ads"

$productName = "windows-data-science-vm"

$planName = "windows2016"

$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image

$offerName = "windows-data-science-vm"

$skuName = "windows2016"

$version = "0.2.02"

$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version
...
```
Затем нужно передать конфигурацию виртуальной машины (вместе с объектами конфигурации сети) командлету `New-AzureRmVM`.

## <a name="next-steps"></a>Дополнительная информация
Инструкции по быстрому созданию виртуальной машины с помощью командлета `New-AzureRmVM` на основе полученных данных образа см. в статье [Создание виртуальной машины Windows с помощью PowerShell](quick-create-powershell.md).

Ознакомьтесь с примерами сценариев PowerShell в статье [Создание полностью настроенной виртуальной машины с помощью PowerShell](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


