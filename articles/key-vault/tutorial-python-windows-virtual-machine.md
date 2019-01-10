---
title: Руководство. Использование Azure Key Vault с виртуальной машиной Windows в Azure (Python) | Документация Майкрософт
description: Руководство по настройке веб-приложения ASP.NET Core для считывания секрета из Key Vault
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: cced3d363f9eb7418d6f453eccb1bf1d7ac20ead
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2019
ms.locfileid: "53972351"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-python"></a>Руководство. Использование Azure Key Vault с виртуальной машиной Windows (Python)

Azure Key Vault помогает защитить секреты, такие как ключи API, строки подключения к базам данных, необходимые для доступа к приложениям, службам и ИТ-ресурсам.

В этом руководстве описано, как в веб-приложении Azure настроить чтение данных из Azure Key Vault с помощью управляемых удостоверений для ресурсов Azure. Далее вы узнаете:

> [!div class="checklist"]
> * Создать хранилище ключей.
> * сохранение секрета в хранилище ключей;
> * получение секрета из хранилища ключей;
> * Создать виртуальную машину Azure.
> * Включить [управляемое удостоверение](../active-directory/managed-identities-azure-resources/overview.md) для виртуальной машины.
> * Предоставить разрешения, необходимые консольному приложению для чтения данных из хранилища ключей.
> * Получить секреты из Key Vault.

Прежде чем мы продолжим, ознакомьтесь с [основными понятиями](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Предварительные требования
* Для всех платформ.
  * Git ([скачать](https://git-scm.com/downloads)).
  * Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) версии 2.0.4 или более поздней. Он доступен для Windows, Mac и Linux.

В этом руководстве используется Управляемое удостоверение службы.

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Что такое управляемое удостоверение службы, и как это работает?
Прежде чем мы продолжим, давайте разберем MSI. Azure Key Vault может безопасно хранить учетные данные, чтобы их не было в коде, но чтобы получить их, необходимо пройти проверку подлинности в хранилище ключей Azure. Для проверки подлинности в Key Vault необходимы учетные данные! Классическая задача начальной загрузки. Используя магию Azure и Azure AD, MSI предоставляет "загрузочное удостоверение", что значительно упрощает начало работы.

Вот как это работает При включении MSI для службы Azure, такой как виртуальные машины, служба приложений или Функции, Azure создает [субъект-службу](key-vault-whatis.md#basic-concepts) для экземпляра службы в Azure Active Directory и вводит учетные данные для субъекта-службы в экземпляр службы. 

![MSI](media/MSI.png)

Далее код вызывает локальную службу метаданных, доступную в ресурсе Azure, для получения маркера доступа.
Код использует маркер доступа, который он получает от локального MSI_ENDPOINT для проверки подлинности в службе Azure Key Vault. 

## <a name="log-in-to-azure"></a>Вход в Azure

Чтобы войти в Azure с помощью Azure CLI, введите следующую команду:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#az-group-create). Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими.

Выберите имя группы ресурсов для заполнителя.
В следующем примере создается группа ресурсов в регионе "Западная часть США":

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Созданная группа ресурсов будет использоваться далее в этой статье.

## <a name="create-a-key-vault"></a>Создайте хранилище ключей.

Теперь создайте хранилище ключей в группе ресурсов, созданной на предыдущем шаге. Введите следующие сведения:

* Имя хранилища ключей. Имя должно быть строкой длиной от 3 до 24 символов, содержащей только цифры (0–9), буквы (a–z, A–Z) и символ "-".
* Имя группы ресурсов.
* Расположение. **Западная часть США**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
На этом этапе любые операции в этом хранилище ключей может выполнять только учетная запись Azure.

## <a name="add-a-secret-to-the-key-vault"></a>Добавление секрета в хранилище ключей

Теперь мы добавим секрет, чтобы продемонстрировать этот процесс. Вы можете хранить здесь строки подключения SQL и прочие сведения, которые должны храниться безопасно, но быть доступны для приложения.

Введите следующие команды, чтобы создать секрет с именем **AppSecret** в хранилище ключей. Этот секрет будет хранить значение **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Создание виртуальной машины
Чтобы создать виртуальную машину Windows, см. указанные ниже инструкции.

[Интерфейс командной строки Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[Портал](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>Назначение удостоверения виртуальной машине
На этом шаге мы создадим назначаемое системой удостоверение для виртуальной машины, выполнив следующую команду в Azure CLI.

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Запишите указанное ниже удостоверение systemAssignedIdentity. Выходные данные приведенной выше команды выглядят следующим образом: 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-virtual-machine-identity-permission-to-key-vault"></a>Предоставление удостоверению виртуальной машины разрешения на доступ к Key Vault
Теперь мы можем предоставить созданному выше удостоверению разрешение для Key Vault, выполнив следующую команду:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Вход на виртуальную машину

Выполните инструкции из этого [руководства](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="create-and-run-sample-python-app"></a>Создание и запуск примера приложения Python

Ниже приведен пример файла с именем Sample.py. Он использует библиотеку [requests](http://docs.python-requests.org/en/master/) для выполнения вызовов HTTP GET.

## <a name="edit-samplepy"></a>Редактирование файла Sample.py
Откройте созданный файл Sample.py и скопируйте приведенный ниже код.

```
The below is a 2 step process. 
1. Fetch a token from the local MSI endpoint on the VM which in turn fetches a token from Azure Active Directory
2. Pass the token to Key Vault and fetch your secret 
```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets a access_token from Azure Active Directory using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to Key Vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

By running you should see the secret value 
```
python Sample.py
```

The above code shows you how to do operations with Azure Key Vault in an Azure Windows Virtual Machine. 

## Next steps

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)
