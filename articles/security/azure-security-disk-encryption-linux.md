---
title: Включение шифрования дисков Azure для виртуальных машин IaaS под управлением Linux
description: В этой статье приведены инструкции по включению шифрования дисков Microsoft Azure для виртуальных машин IaaS под управлением Linux.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 12/17/2018
ms.custom: seodec18
ms.openlocfilehash: 749c139e35118ac8b83281bd255b152f61accc0d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542566"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms"></a>Включение шифрования дисков Azure для виртуальных машин IaaS под управлением Linux 

Возможно несколько сценариев включения шифрования дисков, и последовательность действий для каждого может отличаться. В следующих разделах сценарии для виртуальных машин IaaS под управлением Linux описаны более подробно. Перед тем как использовать шифрование дисков Azure, необходимо выполнить [эти предварительные требования](azure-security-disk-encryption-prerequisites.md) и [дополнительные требования для виртуальных машин IaaS Linux](azure-security-disk-encryption-prerequisites.md#bkmk_LinuxPrereq).

Создайте [моментальный снимок](../virtual-machines/windows/snapshot-copy-managed-disk.md) и (или) резервную копию перед шифрованием дисков. Резервные копии обеспечивают возможность восстановления в случае любого непредвиденного сбоя во время шифрования. Для виртуальных машин с управляемыми дисками необходимо создать резервную копию до начала шифрования. После создания резервной копии можно зашифровать управляемые диски с помощью командлета Set-AzureRmVMDiskEncryptionExtension с параметром -skipVmBackup. Дополнительные сведения см. в статье [Резервное копирование и восстановление зашифрованных виртуальных машин с помощью службы Azure Backup](../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
> - Если вы уже использовали [шифрование дисков Azure с помощью приложения Azure AD](azure-security-disk-encryption-prerequisites-aad.md) для шифрования этой виртуальной машины, этот способ нужно применять и далее для шифрования виртуальной машины. На этой зашифрованной виртуальной машине нельзя использовать [шифрование дисков Azure](azure-security-disk-encryption-prerequisites.md), так как этот сценарий не работает — переключение из приложения AAD для данной зашифрованной виртуальной машины сейчас не поддерживается.
 > - Для шифрования дисков Azure необходимо, чтобы Key Vault и виртуальные машины были расположены в одном регионе. Создайте и используйте хранилище ключей, которое находится в том же регионе, что и виртуальная машина, которую нужно зашифровать.
> - Шифрование томов операционной системы Linux может занять несколько часов. Обычно для шифрования томов ОС Linux требуется больше времени, чем для томов данных. 
> - Отключение шифрования на виртуальных машинах Linux поддерживается только для томов данных. Если том операционной системы зашифрован, то отключение не поддерживается для томов данных и томов ОС.  


## <a name="bkmk_RunningLinux"> </a> Включение шифрования на виртуальной машине IaaS под управлением Linux, которая уже существует или работает
В этом сценарии шифрование можно включить с помощью шаблона Resource Manager, командлетов PowerShell или команд интерфейса командной строки. Сведения о схеме для расширения виртуальной машины см. в статье [Шифрование дисков Azure для Linux ](../virtual-machines/extensions/azure-disk-enc-linux.md).

>[!IMPORTANT]
 >Нужно обязательно создать образ или резервную копию экземпляра виртуальной машины на основе управляемого диска вне шифрования дисков Azure и перед включением этой функции. Моментальный снимок управляемого диска можно создать с портала или использовать для этого [Azure Backup](../backup/backup-azure-vms-encryption.md). Резервные копии обеспечивают возможность восстановления при любом непредвиденном сбое во время шифрования. После создания резервной копии можно зашифровать управляемые диски с помощью командлета Set-AzureRmVMDiskEncryptionExtension, указав параметр -skipVmBackup. Команда Set-AzureRmVMDiskEncryptionExtension будет завершаться ошибкой при использовании с виртуальными машинами на основе управляемых дисков, пока не будет сделана резервная копия и указан определенный параметр. 
>
>Шифрование или отключение шифрования может привести к перезагрузке виртуальной машины. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Включение шифрования на существующей или работающей виртуальной машине Linux с помощью Azure CLI 
Можно включить шифрование дисков для зашифрованного виртуального жесткого диска, установив программу командной строки [Azure CLI 2.0](/cli/azure). Его можно использовать в браузере с [Azure Cloud Shell](../cloud-shell/overview.md), а также установить на локальном компьютере и использовать в любом сеансе PowerShell. Чтобы включить шифрование на виртуальных машинах IaaS под управлением Linux, которые уже существуют или работают в Azure, используйте следующие команды CLI.

Используйте команду [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable), чтобы включить шифрование на работающей виртуальной машине IaaS в Azure.

-  **Шифрование работающей виртуальной машины:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Шифрование работающей виртуальной машины с использованием KEK:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Синтаксис значения параметра disk-encryption-keyvault — это полная строка идентификатора: /subscriptions/[ИД или GUID подписки]/resourceGroups/[имя группы ресурсов]/providers/Microsoft.KeyVault/vaults/[имя хранилища ключей].</br> </br>
Синтаксис значения параметра key-encryption-key —это полный универсальный код ресурса (URI) для ключа шифрования ключей: https://[имя хранилища ключей].vault.azure.net/keys/[имя KEK]/[ИД KEK]. 

- **Проверка того, что диски зашифрованы:** чтобы проверить состояние шифрования виртуальной машины IaaS, используйте команду [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show). 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Отключение шифрования:** Чтобы отключить шифрование, используйте команду [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). Для виртуальных машин Linux отключение шифрования возможно только для томов данных.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a>Включение шифрования на существующей или работающей виртуальной машине Linux с помощью PowerShell
Используйте командлет [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension), чтобы включить шифрование на работающей виртуальной машине IaaS в Azure. 

-  **Шифрование работающей виртуальной машины:** приведенный ниже сценарий инициализирует переменные и запускает командлет Set-AzureRmVMDiskEncryptionExtension. Группа ресурсов, виртуальная машина и хранилище ключей должны быть уже созданы в качестве необходимых компонентов. Замените MySecureRg, MySecureVM и MySecureVault своими значениями. При шифровании дисков данных, а не диска операционной системы, может потребоваться добавить параметр -VolumeType. 

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Шифрование работающей виртуальной машины с использованием KEK:** При шифровании дисков данных, а не диска операционной системы, может потребоваться добавить параметр -VolumeType. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```

    >[!NOTE]
    > Синтаксис значения параметра disk-encryption-keyvault — это полная строка идентификатора: /subscriptions/[ИД или GUID подписки]/resourceGroups/[имя группы ресурсов]/providers/Microsoft.KeyVault/vaults/[имя хранилища ключей].</br> Синтаксис значения параметра key-encryption-key —это полный универсальный код ресурса (URI) для ключа шифрования ключей: https://[имя хранилища ключей].vault.azure.net/keys/[имя KEK]/[ИД KEK]. 
    
- **Проверка того, что диски зашифрованы:** чтобы проверить состояние шифрования виртуальной машины IaaS, используйте командлет [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus). 
    
     ```azurepowershell-interactive 
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName MySecureRg -VMName MySecureVM
     ```
    
- **Отключение шифрования дисков:** Для отключения шифрования используйте командлет [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Для виртуальных машин Linux отключение шифрования возможно только для томов данных.
     
     ```azurepowershell-interactive 
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Включение шифрования на существующей или работающей виртуальной машине с помощью шаблона

Включить шифрование дисков на существующих или работающих виртуальных машинах IaaS под управлением Linux в Azure можно с помощью [шаблона Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad).

1. На странице шаблона быстрого запуска Azure нажмите кнопку **Deploy to Azure** (Развернуть в Azure).

2. Выберите подписку, группу ресурсов, расположение группы ресурсов, параметры, условия использования и соглашение. Нажмите кнопку **Создать**, чтобы включить шифрование на существующей или работающей виртуальной машине IaaS.

В следующей таблице перечислены параметры шаблона Resource Manager для имеющихся или работающих виртуальных машин.

| Параметр | ОПИСАНИЕ |
| --- | --- |
| vmName | Имя виртуальной машины для выполнения операции шифрования. |
| keyVaultName | Имя хранилища ключей, в которое будет передан ключ BitLocker. Его можно получить с помощью командлета `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` или команды Azure CLI az keyvault list --resource-group "MySecureGroup" |ConvertFrom-JSON`|
| keyVaultResourceGroup | Имя группы ресурсов, содержащей хранилище ключей.|
|  keyEncryptionKeyURL | URL-адрес ключа шифрования ключей, который используется для шифрования созданного ключа BitLocker. Это необязательный параметр, если выбрать **nokek** из раскрывающегося списка UseExistingKek. Если из раскрывающегося списка UseExistingKek выбрано значение **kek**, то потребуется ввести значение _keyEncryptionKeyURL_. |
| volumeType | Тип тома, для которого будет выполняться шифрование. Допустимые значения: _OS_, _Data_ и _All_. 
| forceUpdateTag | Передает уникальное значение, такое как GUID, каждый раз, когда операция должна выполняться принудительно. |
| resizeOSDisk | Если размер раздела ОС будет изменен и займет весь виртуальный жесткий диск ОС до разделения системного тома. |
| location | Расположение для всех ресурсов. |



## <a name="encrypt-virtual-machine-scale-sets"></a>Шифрование масштабируемых наборов виртуальных машин
[Масштабируемые наборы виртуальных машин Azure](../virtual-machine-scale-sets/overview.md) позволяют создавать и администрировать группы идентичных виртуальных машин с балансировкой нагрузки. Число экземпляров виртуальных машин может автоматически увеличиваться или уменьшаться в зависимости от спроса или по определенному расписанию. Выполняйте шифрование масштабируемых наборов виртуальных машин с помощью CLI или Azure PowerShell.

Пример пакетного файла для шифрования дисков данных масштабируемого набора Linux можно найти на сайте [Github](https://github.com/Azure-Samples/azure-cli-samples/tree/master/disk-encryption/vmss). Этот пример создает группу ресурсов и масштабируемый набор Linux, подключает диск данных размером 5 ГБ и шифрует этот масштабируемый набор виртуальных машин.

### <a name="register-for-disk-encryption-preview-using-azure-cli"></a>Регистрация для получения предварительной версии службы шифрования дисков с помощью Azure CLI

Для работы предварительной версии службы шифрования дисков Azure для масштабируемых наборов виртуальных машин необходимо самостоятельно зарегистрировать подписку с помощью команды [az feature register](/cli/azure/feature#az-feature-register). Указанные ниже действия нужно выполнить только при первом использовании предварительной версии функции шифрования дисков.

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Распространение запроса на регистрацию может занять 10 минут. Состояние регистрации можно проверить с помощью команды [az feature show](/cli/azure/feature#az-feature-show). Когда `State` будет иметь значение *Registered*, повторно зарегистрируйте поставщик *Microsoft.Compute* с помощью команды [az provider register](/cli/azure/provider#az-provider-register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Шифрование масштабируемых наборов виртуальных машин с помощью Azure CLI
Используйте [az vmss encryption enable](/cli/azure/vmss/encryption#az-vmss-encryption-enable), чтобы включить шифрование в масштабируемом наборе виртуальных машин Windows. Если для масштабируемого набора задана политика обновления вручную, запустите шифрование с помощью команды [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Группа ресурсов, виртуальная машина и хранилище ключей должны быть уже созданы в качестве необходимых компонентов. 

-  **Шифрование работающего масштабируемого набора виртуальных машин**:
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Шифрование работающего масштабируемого набора виртуальных машин с использованием ключа шифрования ключей для упаковки ключа**:
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
- **Получение состояния шифрования для масштабируемого набора виртуальных машин:** используйте команду [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show).

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Отключение шифрования в масштабируемом наборе виртуальных машин**: используйте команду [az vmss encryption disable](/cli/azure/vmss/encryption#az-vmss-encryption-disable).
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

### <a name="register-for-disk-encryption-preview-using-azure-powershell"></a>Регистрация для получения предварительной версии службы шифрования дисков с помощью Azure Powershell

Для работы предварительной версии службы шифрования дисков Azure для масштабируемых наборов виртуальных машин необходимо самостоятельно зарегистрировать подписку с помощью команды [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). Указанные ниже действия нужно выполнить только при первом использовании предварительной версии функции шифрования дисков.

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Распространение запроса на регистрацию может занять 10 минут. Состояние регистрации можно проверить с помощью команды [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Когда `RegistrationState` будет иметь значение *Registered*, повторно зарегистрируйте поставщик *Miсrosoft.Compute* с помощью команды [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Шифрование масштабируемых наборов виртуальных машин с помощью Azure PowerShell

Используйте командлет [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension), чтобы включить шифрование в масштабируемом наборе виртуальных машин Windows. Группа ресурсов, виртуальная машина и хранилище ключей должны быть уже созданы в качестве необходимых компонентов.

-  **Шифрование работающего масштабируемого набора виртуальных машин**:
    ```powershell
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:
    ```powershell
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

- **Получение состояния шифрования для масштабируемого набора виртуальных машин:** используйте командлет [Get-AzureRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption).
    
    ```powershell
    get-AzureRmVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Отключение шифрования в масштабируемом наборе виртуальных машин**: используйте командлет [Disable-AzureRmVmssDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption). 

    ```powershell
    Disable-AzureRmVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-linux-virtual-machine-scale-sets"></a>Шаблоны Azure Resource Manager для масштабируемых наборов виртуальных машин Linux

Для шифрования или расшифровки масштабируемых наборов виртуальных машин Linux используйте шаблоны Azure Resource Manager и приведенные ниже инструкции:

- [Включите шифрование в масштабируемом наборе виртуальных машин Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux).
- [Разверните масштабируемый набор виртуальных машин Linux с помощью Jumpbox и включите шифрование в этом масштабируемом наборе Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
- [Отключите шифрование в масштабируемом наборе виртуальных машин Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

     1. Нажмите кнопку **Развернуть в Azure**.
     2. Заполните обязательные поля, а затем согласитесь с условиями.
     3. Щелкните **Приобрести**, чтобы развернуть шаблон.

## <a name="bkmk_EFA"> </a>Использование функции EncryptFormatAll для дисков данных на виртуальных машинах IaaS под управлением Linux
Параметр **EncryptFormatAll** сокращает время шифрования дисков данных Linux. Разделы, отвечающие определенным критериям, будут отформатированы (с текущей файловой системой). Затем они будут обратно подключены там, где они были подключены до выполнения команды. Если вы хотите исключить диск данных, который соответствует критериям, можно отключить его перед выполнением команды.

 После выполнения этой команды все диски, которые были подключены ранее, будут отформатированы. Уровень шифрования запускается поверх уже теперь пустого диска. Если выбран этот параметр, временный диск с ресурсами, подключенный к виртуальной машине, также будет зашифрован. Если временный диск сбрасывается, при следующей возможности он будет переформатирован и повторно зашифрован для виртуальной машины с помощью шифрования дисков Azure.

>[!WARNING]
> Параметр EncryptFormatAll не следует использовать, если в томах данных виртуальной машины есть требуемые данные. Вы можете исключить диски из шифрования, отключив их. Сначала следует попробовать параметр EncryptFormatAll на тестовой виртуальной машине, понять его функцию и последствия, а уже потом использовать на рабочих виртуальных машинах. Параметр EncryptFormatAll форматирует диск данных, и все данные на этом диске будут потеряны. Прежде чем продолжить, убедитесь, что диски, которые вы хотите исключить, отключены надлежащим образом. </br></br>
 >Если этот параметр устанавливается при обновлении параметров шифрования, это может привести к перезагрузке перед фактическим шифрованием. В этом случае необходимо также удалить из FSTAB-файла диск, который не нужно форматировать. Аналогично, перед началом операции шифрования нужно добавить в FSTAB-файл раздел, который нужно отформатировать и зашифровать. 

### <a name="bkmk_EFACriteria"> </a> Критерии EncryptFormatAll
Параметр проходит через все разделы и шифрует их, если они соответствуют **всем** из приведенных ниже критериев: 
- это не корневой или загрузочный раздел, а также не раздел операционной системы;
- этот раздел еще не зашифрован;
- это не том BEK;
- это не том RAID;
- это не том LVM;
- он подключен.

Выполняйте шифрование дисков, которые составляют том RAID или LVM, а не самого тома RAID или LVM.

### <a name="bkmk_EFAPSH"> </a> Использование параметра EncryptFormatAll с Azure CLI
Используйте команду [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable), чтобы включить шифрование на работающей виртуальной машине IaaS в Azure.

-  **Шифрование работающей виртуальной машины с использованием параметра EncryptFormatAll:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="bkmk_EFAPSH"> </a> Использование параметра EncryptFormatAll с командлетом PowerShell
Используйте командлет [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) с параметром [EncryptFormatAll](https://www.powershellgallery.com/packages/AzureRM/5.0.0). 

**Шифрование работающей виртуальной машины с использованием параметра EncryptFormatAll:** например, приведенный ниже сценарий инициализирует переменные и запускает командлет Set-AzureRmVMDiskEncryptionExtension с параметром EncryptFormatAll. Группа ресурсов, виртуальная машина и хранилище ключей должны быть уже созданы в качестве необходимых компонентов. Замените MySecureRg, MySecureVM и MySecureVault своими значениями.
  
   ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MySecureVM';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
   ```


### <a name="bkmk_EFALVM"> </a> Использование параметра EncryptFormatAll с диспетчером логических томов (LVM) 
Рекомендуем установить LVM-on-crypt. Для всех следующих примеров замените путь к устройству и точки подключения в соответствии с вашим вариантом использования. Эту настройку можно выполнить следующим образом.

- Добавить диски данных, которые составляют виртуальную машину.
- Форматировать, подключить и добавить эти диски в FSTAB-файл.

    1. Отформатируйте добавленный диск. Здесь мы используем символические ссылки, созданные Azure. Использование символических ссылок позволяет избежать проблем, связанных с изменениями имен устройств. Дополнительные сведения см. в статье [Устранение неполадок при изменении имени устройства виртуальной машины Linux](../virtual-machines/linux/troubleshoot-device-names-problems.md).
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. Подключите диски.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. Добавьте сведения в FSTAB-файл.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. Выполните командлет PowerShell Set-AzureRmVMDiskEncryptionExtension с параметром EncryptFormatAll для шифрования этих дисков.
         ```azurepowershell-interactive
         Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. Настройте диспетчер логических томов поверх новых дисков. Обратите внимание, что зашифрованные диски разблокируются после завершения загрузки виртуальной машины. Таким образом, подключение диспетчера логических томов также придется отложить.


## <a name="bkmk_VHDpre"> </a> Новые виртуальные машины IaaS, при создании которых используются зашифрованные виртуальные жесткие диски и ключи шифрования
В этом сценарии шифрование можно включить с помощью командлетов PowerShell или команд интерфейса командной строки. 

Следуйте инструкциям, приведенным в приложении к этой статье, чтобы подготовить предварительно зашифрованные образы, которые можно использовать в Azure. После создания образа выполните действия, описанные в следующем разделе, чтобы создать зашифрованную виртуальную машину Azure.

* [Подготовка предварительно зашифрованного виртуального жесткого диска Windows](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Подготовка предварительно зашифрованного виртуального жесткого диска Linux](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Нужно обязательно создать образ или резервную копию экземпляра виртуальной машины на основе управляемого диска вне шифрования дисков Azure и перед включением этой функции. Моментальный снимок управляемого диска можно создать с портала или использовать для этого [Azure Backup](../backup/backup-azure-vms-encryption.md). Резервные копии обеспечивают возможность восстановления при любом непредвиденном сбое во время шифрования. После создания резервной копии можно зашифровать управляемые диски с помощью командлета Set-AzureRmVMDiskEncryptionExtension, указав параметр -skipVmBackup. Команда Set-AzureRmVMDiskEncryptionExtension будет завершаться ошибкой при использовании с виртуальными машинами на основе управляемых дисков, пока не будет сделана резервная копия и указан определенный параметр. 
>
>Шифрование или отключение шифрования может привести к перезагрузке виртуальной машины. 



### <a name="bkmk_VHDprePSH"> </a> Использование Azure PowerShell для шифрования виртуальных машин IaaS с предварительно зашифрованными виртуальными жесткими дисками 
Можно включить шифрование дисков для зашифрованного виртуального жесткого диска с помощью командлета PowerShell [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples). Приведенный ниже пример имеет некоторые общие параметры. 

```azurepowershell-interactive
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Включение шифрования на добавленном диске данных

Новый диск данных можно добавить с помощью команды [az vm disk attach](../virtual-machines/linux/add-disk.md) или [через портал Azure](../virtual-machines/linux/attach-disk-portal.md). Прежде чем запустить шифрование, необходимо подключить только что присоединенный диск данных. Нужно запросить шифрование диска данных, так как диск будет неработоспособным во время шифрования. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Включение шифрования на добавленном диске данных с помощью интерфейса командной строки Azure

 Если виртуальная машина ранее была зашифрована со значением "All", то в дальнейшем значение параметра volume-type должно оставаться "All". Значение "All" включает как диски ОС, так и диски данных. Если виртуальная машина была зашифрована с типом тома "OS", в дальнейшем параметр --volume-type следует заменить на "All", чтобы были включены диск ОС и новый диск данных. Если виртуальная машина была зашифрована с типом тома "Data", то в дальнейшем он может оставаться со значением "Data", как это показано ниже. Добавления и присоединения нового диска данных к виртуальной машине недостаточно для подготовки к шифрованию. Вновь подключенный диск также должен быть отформатирован и правильно подключен к виртуальной машине перед включением шифрования. В Linux диск монтируется в /etc/fstab с [постоянным именем блочного устройства](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

В отличие от синтаксиса Powershell, интерфейс командной строки (CLI) не требует предоставления уникальной версии последовательности при включении шифрования. CLI автоматически создает и использует свое собственное уникальное значение версии последовательности.

-  **Шифрование томов данных работающей виртуальной машины:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Шифрование томов данных работающей виртуальной машины с использованием KEK:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Включение шифрования на добавленном диске данных с помощью Azure PowerShell
 При использовании PowerShell для шифрования нового диска для Linux необходимо указать новую версию последовательности. Версия последовательности должна быть уникальной. Приведенный ниже сценарий создает GUID для версии последовательности. 
 

-  **Шифрование томов данных работающей виртуальной машины:** приведенный ниже сценарий инициализирует переменные и запускает командлет Set-AzureRmVMDiskEncryptionExtension. Группа ресурсов, виртуальная машина и хранилище ключей должны быть уже созданы в качестве необходимых компонентов. Замените MySecureRg, MySecureVM и MySecureVault своими значениями. Допустимые значения параметра -VolumeType: "All", "OS" и "Data". Если виртуальная машина была зашифрована с типом тома "OS" или "All", в дальнейшем параметр -VolumeType следует заменить на "All", чтобы были включены диск ОС и новый диск данных.

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
    ```
- **Шифрование томов данных работающей виртуальной машины с использованием KEK:** Допустимые значения параметра -VolumeType: "All", "OS" и "Data". Если виртуальная машина была зашифрована с типом тома "OS" или "All", в дальнейшем параметр -VolumeType следует заменить на "All", чтобы были включены диск ОС и новый диск данных.

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data';

     ```

    >[!NOTE]
    > Синтаксис значения параметра disk-encryption-keyvault — это полная строка идентификатора: /subscriptions/[ИД или GUID подписки]/resourceGroups/[имя группы ресурсов]/providers/Microsoft.KeyVault/vaults/[имя хранилища ключей].</br> Синтаксис значения параметра key-encryption-key —это полный универсальный код ресурса (URI) для ключа шифрования ключей: https://[имя хранилища ключей].vault.azure.net/keys/[имя KEK]/[ИД KEK]. 


## <a name="disable-encryption-for-linux-vms"></a>Отключение шифрования для виртуальных машин Linux
Вы можете отключить шифрование с помощью Azure PowerShell, Azure CLI или шаблона Resource Manager. 

>[!IMPORTANT]
>Отключение шифрования, выполняемого с помощью службы шифрования Azure, на виртуальных машинах Linux поддерживается только для томов данных. Если том операционной системы зашифрован, то отключение не поддерживается для томов данных и томов ОС.  

- **Отключение шифрования дисков с помощью Azure PowerShell.** для отключения шифрования используйте командлет [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Отключение шифрования дисков с помощью Azure CLI:** чтобы отключить шифрование, используйте команду [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Отключение шифрования с помощью шаблона Resource Manager:** используйте шаблон [отключения шифрования на работающей виртуальной машине Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) для отключения шифрования.
     1. Нажмите кнопку **Развернуть в Azure**.
     2. Выберите подписку, группу ресурсов, расположение, виртуальную машину, условия использования и соглашение.
     3.  Нажмите кнопку **покупка**, чтобы отключить шифрование дисков на работающей виртуальной машине Windows. 


## <a name="next-steps"></a>Дополнительная информация
> [!div class="nextstepaction"]
> [Включение шифрования дисков Azure для виртуальных машин Windows IaaS](azure-security-disk-encryption-windows.md)
