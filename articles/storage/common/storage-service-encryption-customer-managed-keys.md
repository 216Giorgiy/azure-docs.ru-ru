---
title: Шифрование службы хранилища Azure с помощью управляемых клиентом ключей в Azure Key Vault | Документация Майкрософт
description: С помощью функции "Шифрование службы хранилища Azure" можно шифровать данные в хранилище BLOB-объектов Azure и службе файлов Azure и расшифровывать эти данные при их извлечении с помощью ключей, которыми управляет клиент.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 10/11/2018
ms.author: lakasa
ms.component: common
ms.openlocfilehash: e2497233ec97ffc88bf13797f62d601d4da373a1
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628499"
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Шифрование службы хранилища с помощью управляемых клиентом ключей в Azure Key Vault

Microsoft Azure прилагает все усилия, чтобы защитить данные согласно обязательствам по безопасности и соответствию, установленным в вашей организации. Один из способов защиты данных с помощью платформы службы хранилища Azure — использование функции "Шифрование службы хранилища" (SSE), которая шифрует данные при записи в хранилище и расшифровывает их при извлечении. Шифрование и расшифровка выполняются автоматически и прозрачно. Для этого используется 256-битный [алгоритм шифрования AES](https://wikipedia.org/wiki/Advanced_Encryption_Standard) — один из наиболее сложных среди доступных блочных шифров.

Можно использовать ключи шифрования с SSE, управляемые корпорацией Майкрософт, или собственные ключи шифрования. В этой статье описывается использование собственных ключей шифрования. Дополнительные сведения об использовании ключей, управляемых корпорацией Майкрософт, и о SSE в целом см. в статье [Шифрование службы хранилища Azure для неактивных данных (предварительная версия)](storage-service-encryption.md).

Функция SSE для хранилища BLOB-объектов Azure и службы файлов Azure интегрирована с Azure Key Vault, чтобы можно было использовать хранилище ключей для управления ключами шифрования. Можно создавать собственные ключи шифрования и хранить их в хранилище ключей либо использовать API-интерфейсы Azure Key Vault для генерации этих ключей. С помощью Azure Key Vault вы можете контролировать ключи и управлять ими, а также выполнять аудит их использования.

> [!Note]  
> Шифрование службы хранилища с помощью управляемых клиентом ключей недоступно для [Управляемых дисков Azure](../../virtual-machines/windows/managed-disks-overview.md). Служба [Шифрование дисков Azure](../../security/azure-security-disk-encryption-overview.md) использует стандартные для отрасли шифровальные решения [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) (Windows) и [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) (Linux), которые интегрированы в Key Vault.

Зачем нужно создавать собственные ключи? Пользовательские ключи обеспечивают большую гибкость, позволяя создавать, сменять, отключать и определять элементы управления доступом, а также предоставляют возможность выполнять аудит ключей шифрования, используемых для защиты данных.

## <a name="get-started-with-customer-managed-keys"></a>Начало работы с ключами, управляемыми клиентом

Чтобы использовать ключи, управляемые клиентом, можно создать хранилище ключей и ключ или использовать имеющиеся. Учетная запись хранения и Key Vault должны быть расположены в одном регионе, но могут находиться в разных подписках.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="step-1-create-a-storage-account"></a>Шаг 1. Создание учетной записи хранения

Если у вас еще нет учетной записи хранения, создайте ее. Дополнительные сведения см. в разделе [Создание учетной записи хранения](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>Шаг 2. Включение SSE для хранилища BLOB-объектов и хранилища файлов

Чтобы включить SSE с помощью ключей, управляемых пользователем, в Azure Key Vault необходимо также активировать две функции защиты ключей: обратимого удаления и Do Not Purge (Не очищать). Эти параметры гарантируют, что ключи не будут случайно или намерено удалены. Максимальный срок хранения ключей составляет 90 дней. Пользователи защищены от злоумышленников и шантажистов.

Если вы хотите активировать ключи, управляемые клиентом, для SSE с помощью программных средств, вы можете использовать [REST API поставщика ресурсов службы хранилища Azure](https://docs.microsoft.com/rest/api/storagerp), [клиентскую библиотеку поставщика ресурсов хранилища для .NET](https://docs.microsoft.com/dotnet/api), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) или [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Чтобы использовать управляемые клиентом ключи с помощью SSE, необходимо назначить удостоверение учетной записи хранения. Чтобы настроить удостоверение, выполните следующую команду PowerShell или Azure CLI:

```powershell
Set-AzStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

```azurecli-interactive
az storage account \
    --account-name <account_name> \
    --resource-group <resource_group> \
    --assign-identity
```

Чтобы включить функции обратимого удаления и Do Not Purge (Не очищать), выполните следующие команды PowerShell или Azure CLI:

```powershell
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

```azurecli-interactive
az resource update \
    --id $(az keyvault show --name <vault_name> -o tsv | awk '{print $1}') \
    --set properties.enableSoftDelete=true

az resource update \
    --id $(az keyvault show --name <vault_name> -o tsv | awk '{print $1}') \
    --set properties.enablePurgeProtection=true
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>Шаг 3. Включение шифрования с использованием управляемых клиентом ключами

По умолчанию SSE использует ключи, управляемые корпорацией Майкрософт. Включить SSE с помощью управляемых пользователем ключей для учетной записи хранения можно на [портале Azure](https://portal.azure.com/). В колоне **параметров** для учетной записи хранения выберите **Шифрование**. Выберите параметр **Использовать собственный ключ**, как показано на следующем рисунке.

![Снимок экрана портала с параметром шифрования](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>Шаг 4. Выбор ключа

Вы можете указать ключ в качестве универсального кода ресурса (URI) или выбрать из хранилища ключей.

#### <a name="specify-a-key-as-a-uri"></a>Указание ключа в качестве URI

Чтобы указать ключ из URI, сделайте следующее.

1. Выберите параметр **Введите URI ключа**.
2. В поле **Ключевой URI** укажите URI.

   ![Снимок экрана портала с окном SSE, в котором выбрано указание универсального кода ресурса (URI) ключа](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

#### <a name="specify-a-key-from-a-key-vault"></a>Указание ключа из хранилища ключей

Чтобы указать ключ из хранилища ключей, сделайте следующее.

1. Выберите параметр **Выбрать в Key Vault**.
2. Выберите хранилище ключей, содержащее ключ, который вы хотите использовать.
3. Выберите ключ из хранилища ключей.

   ![Снимок экрана портала с окном SSE, в котором выбрано использование собственного ключа](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Если в учетной записи хранения нет доступа к хранилищу ключей, можно выполнить команду Azure PowerShell, показанную на рисунке ниже, чтобы предоставить доступ.

![Снимок экрана портала, показывающий отказ в доступе к Key Vault](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Вы также можете предоставить доступ с помощью портала Azure, перейдя к Azure Key Vault на портале Azure и предоставив доступ учетной записи хранения.

Указанный выше ключ можно связать с существующей учетной записи хранения с помощью следующих команд PowerShell:

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```

### <a name="step-5-copy-data-to-storage-account"></a>Шаг 5. Копирование данных в учетную запись хранения

Передайте данные в новую учетную запись хранения для их шифрования. Дополнительные сведения см. в разделе [Часто задаваемые вопросы о шифровании службы хранилища](storage-service-encryption.md#faq-for-storage-service-encryption).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>Шаг 6. Запрос состояния зашифрованных данных

Запросите состояние зашифрованных данных.

## <a name="faq-for-sse-with-customer-managed-keys"></a>Часто задаваемые вопросы о SSE для управляемых клиентом ключей

**Я использую хранилище класса Premium. Могу ли я использовать управляемые клиентом ключи с функцией SSE?**  
Да, функцию SSE можно использовать с ключами, управляемыми корпорацией Майкрософт, и управляемыми клиентом ключами в хранилищах класса "Стандартный" и Premium.

**Можно ли создавать учетные записи хранения с включенной функцией SSE, используя управляемые клиентом ключи, с помощью Azure PowerShell и Azure CLI?**  
Да.

**Насколько дороже обходится служба хранилища Azure, если я использую управляемые клиентом ключи с функцией SSE?**  
Вы платите за использование Azure Key Vault. Дополнительные сведения см. на [странице цен на Key Vault](https://azure.microsoft.com/pricing/details/key-vault/). За использование SSE дополнительная плата не требуется. Это включено для всех учетных записей хранения.

**Доступна ли функция "Шифрование службы хранилища" для Управляемых дисков Azure?**  
Шифрование службы хранилища доступно для Управляемых дисков Azure, но только с ключами, которыми управляет Майкрософт, а не клиенты. Вместо службы "Управляемые диски", которая поддерживает SSE с использованием управляемых клиентами ключей, мы рекомендуем использовать службу [Шифрование дисков Azure](../../security/azure-security-disk-encryption-overview.md). В ней используются стандартные для отрасли решения [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) (Windows) и [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) (Linux), которые интегрированы в Key Vault.

**Чем функция "Шифрование службы хранилища" отличается от шифрования дисков Azure?**  
Шифрование дисков Azure обеспечивает интеграцию решений на основе операционной системы, например BitLocker, DM-Crypt и Azure Key Vault. Шифрование службы хранилища обеспечивает встроенное шифрование на уровне платформы службы хранилища Azure (ниже уровня виртуальной машины).

**Можно ли отменить доступ к ключам шифрования?**
Да, вы можете отменить доступ в любое время. Отменить доступ к ключам можно несколькими способами. Чтобы получить дополнительные сведения, обратитесь к разделам [Azure​RM.​Key​Vault](https://docs.microsoft.com/powershell/module/az.keyvault/) и [Key Vault - az keyvault](https://docs.microsoft.com/cli/azure/keyvault). Отмена доступа фактически заблокирует доступ ко всем большим двоичным объектам в учетной записи хранения, так как ключ шифрования учетной записи станет недоступен службе хранилища Azure.

**Могу ли я создать учетную запись хранения и ключ в разных регионах?**  
Нет, учетная запись хранения, Azure Key Vault и ключ должны находиться в одном регионе.

**Могу ли я включить управляемые клиентом ключи для функции SSE при создании учетной записи хранения?**  
 Нет. При первом создании учетной записи хранения для функции SSE доступны только ключи, управляемые корпорацией Майкрософт. Чтобы использовать управляемые клиентом ключи, вам потребуется обновить свойства учетной записи хранения. Вы можете использовать REST или одну из клиентских библиотек службы хранилища, чтобы обновить учетную запись хранения программными средствами. Также можно обновить свойства учетной записи хранения с помощью портала Azure после ее создания.

**Могу ли я отключить шифрование при использовании управляемых клиентом ключей с функцией SSE?**  
Нет, отключить шифрование невозможно. Для хранилища BLOB-объектов Azure, службы файлов Azure, хранилища очередей Azure и хранилища таблиц Azure шифрование включено по умолчанию. При необходимости вы можете перейти от использования ключей, управляемых корпорацией Майкрософт, на использование управляемых клиентом ключей и наоборот.

**Включается ли SSE при создании учетной записи хранения?**  
Функция SSE включена для всех учетных записей хранения, а также для хранилища BLOB-объектов Azure, службы файлов Azure, хранилища очередей Azure и хранилища таблиц Azure.

**Мне не удается включить функцию SSE с помощью управляемых клиентом ключей в своей учетной записи хранения.**  
Это учетная запись хранения Azure Resource Manager? Классические учетные записи хранения не поддерживаются с управляемыми клиентом ключами. Функцию SSE с управляемыми клиентом ключами можно включить только в учетных записях хранения Resource Manager.

**Что такое функция обратимого удаления и Do Not Purge (Не очищать)? Нужно ли включить этот параметр, чтобы использовать SSE с управляемыми клиентом ключами?**  
Чтобы использовать SSE с управляемыми клиентом ключами, необходимо включить функцию обратимого удаления и Do Not Purge (Не очищать). Эти параметры гарантируют, что ключ не будет случайно или намерено удален. Максимальный срок хранения ключей составляет 90 дней. Пользователи защищены от злоумышленников и шантажистов. Этот параметр нельзя отключить.

**Функция SSE с управляемыми клиентом ключами доступна только в определенных регионах?**  
Функция SSE с управляемыми клиентом ключами доступна во всех регионах для хранилища BLOB-объектов Azure и службы файлов Azure.

**Куда обратиться, чтобы задать вопрос или оставить отзыв?**  
По любым вопросам, связанным с функцией "Шифрование службы хранилища", обращайтесь по адресу [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о комплексном наборе функций, помогающем разработчикам создавать безопасные приложения, см. в разделе [Руководство по безопасности службы хранилища Azure](storage-security-guide.md).
- Общие сведения о Azure Key Vault см. в статье [Что такое хранилище ключей Azure?](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
- О том, как приступить к работе с Azure Key Vault, рассказывается в разделе [Приступая к работе с хранилищем ключей Azure](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
