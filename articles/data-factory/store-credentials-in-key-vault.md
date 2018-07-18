---
title: Хранение учетных данных в Azure Key Vault | Документация Майкрософт
description: Узнайте, как хранить учетные данные для хранилищ данных, используемых в Azure Key Vault, которые фабрика данных Azure может автоматически извлекать в среду выполнения.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: jingwang
ms.openlocfilehash: e1be16ec6a7536cedf3a27ffacb9c4dffe42bbef
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052421"
---
# <a name="store-credential-in-azure-key-vault"></a>Хранение учетных данных в Azure Key Vault

Учетные данные для хранилищ данных и вычислительных ресурсов можно хранить в [Azure Key Vault](../key-vault/key-vault-whatis.md). Фабрика данных Azure извлекает учетные данные при выполнении действия, которое использует хранилище данных или вычислительный ресурс.

Сейчас эта функция поддерживается для всех видов действий, кроме пользовательских действий. Дополнительные сведения о настройке соединителя см. в разделе "Свойства связанной службы" [в статьях, посвященных каждому типу соединителей](copy-activity-overview.md#supported-data-stores-and-formats).

## <a name="prerequisites"></a>предварительным требованиям

Эта функция основывается на удостоверении службы фабрики данных. Узнайте, как работает [удостоверение службы фабрики данных](data-factory-service-identity.md), и убедитесь, что фабрике данных назначено удостоверение.

>[!TIP]
>В Azure Key Vault, когда вы создадите секрет, **поместите все значения свойства, которые запрашивает связанная служба ADF (например, строка подключения, пароль, ключ субъекта-службы и т. д.)**. Например, для связанной службы хранилища Azure поместите `DefaultEndpointsProtocol=http;AccountName=myAccount;AccountKey=myKey;` как секретный код AKV, затем укажите в поле connectionString из ADF. Для службы Dynamics поместите `myPassword` как секрет AKV, а затем укажите в поле paassword из ADF. Дополнительные сведения о поддерживаемых свойствах см. в статьях о каждом соединителе или вычислительном ресурсе.

## <a name="steps"></a>Действия

Чтобы указать учетные данные, хранимые в Azure Key Vault, необходимо:

1. **Получите удостоверение службы фабрики данных**, скопировав значение идентификатора приложения удостоверения службы (SERVICE IDENTITY APPLICATION ID), созданного вместе с фабрикой. Если вы используете пользовательский интерфейс создания ADF, идентификатор удостоверения службы будет отображаться в окне создания связанной службы Azure Key Vault. Вы также можете получить его на портале Azure, см. раздел [Получение удостоверения службы](data-factory-service-identity.md#retrieve-service-identity).
2. **Предоставьте удостоверению службы доступ к Azure Key Vault.** Откройте хранилище ключей, выберите "Политики доступа" и "Добавить новую". Найдите новый идентификатор приложения удостоверения службы, чтобы предоставить разрешение на **получение** в раскрывающемся списке "Разрешения секретов". Это позволит указанной фабрике получить доступ к секрету в хранилище ключей.
3. **Создайте связанную службу, указывающую на Azure Key Vault.** Дополнительные сведения см. в разделе [Связанная служба Azure Key Vault](#azure-key-vault-linked-service).
4. **Создайте связанную службу хранилища данных, внутри которой указывается соответствующий секрет, хранящийся в хранилище ключей.** См. дополнительные сведения об [указании секрета, хранящегося в хранилище ключей](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Связанная служба Azure Key Vault

Для связанной службы Azure Key Vault поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно |
|:--- |:--- |:--- |
| Тип | Для свойства type необходимо задать значение **AzureKeyVault**. | Yes |
| BaseUrl | Укажите URL-адрес Azure Key Vault. | Yes |

**С помощью пользовательского интерфейса для создания:**

Щелкните **Подключения** -> **Связанные службы** -> **Создать**, найдите "Хранилище ключей Azure":

![Поиск AKV](media/store-credentials-in-key-vault/search-akv.png)

Выберите подготовленное хранилище ключей Azure, в котором хранятся ваши учетные данные. Вы можете выполнить **тестовое подключение**, чтобы убедиться, что ваше подключение AKV действительно. 

![Настройка AKV](media/store-credentials-in-key-vault/configure-akv.png)

**Пример JSON:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Указание секрета, хранящегося в хранилище ключей

Следующие свойства поддерживаются при настройке поля в связанной службе, указывающей секрет хранилища ключей:

| Свойство | ОПИСАНИЕ | Обязательно |
|:--- |:--- |:--- |
| Тип | Для свойства type поля необходимо задать значение **AzureKeyVaultSecret**. | Yes |
| secretName | Имя секрета в Azure Key Vault. | Yes |
| secretVersion | Версия секрета в Azure Key Vault.<br/>Если не указано, используется последняя версия секрета.<br/>Если указано, то он придерживается указанной версии.| Нет  |
| store | Ссылается на связанную службу Azure Key Vault, которая используется для хранения учетных данных. | Yes |

**С помощью пользовательского интерфейса для создания:**

Выберите **Azure Key Vault** для секретных полей при создании подключения к своему хранилищу данных или вычислительному ресурсу. Выберите подготовленную связанную службу Azure Key Vault и укажите **имя секрета**. Вы также можете предоставить версию секрета. 

![Настройка секрета AKV](media/store-credentials-in-key-vault/configure-akv-secret.png)

**Пример JSON: (см. раздел "пароль")** 

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Дополнительная информация
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
