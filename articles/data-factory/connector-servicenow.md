---
title: "Копирование данных из ServiceNow с помощью фабрики данных Azure (бета-версия) | Документация Майкрософт"
description: "Узнайте, как копировать данные из ServiceNow в поддерживаемые хранилища данных в качестве приемников с помощью действия копирования в конвейере фабрики данных Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: jingwang
ms.openlocfilehash: 28ecdc541bc7e95dfa6d7c1b2d984cba0654699f
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2018
---
# <a name="copy-data-from-servicenow-using-azure-data-factory-beta"></a>Копирование данных из ServiceNow с помощью фабрики данных Azure (бета-версия)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из ServiceNow. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

> [!NOTE]
> Эта статья относится к версии 2 фабрики данных, которая в настоящее время доступна в предварительной версии. Если используется служба фабрики данных версии 1, которая является общедоступной версией, ознакомьтесь со статьей [Move data by using Copy Activity](v1/data-factory-data-movement-activities.md) (Перемещение данных с помощью действия копирования).

> [!IMPORTANT]
> Сейчас этот соединитель доступен в бета-версии. Попробуйте поработать с ним и оставьте свой отзыв. Не используйте его в рабочих средах.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из ServiceNow можно скопировать в любое поддерживаемое хранилище данных, используемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Фабрика данных Azure имеет встроенный драйвер для настройки подключения. Поэтому с использованием этого соединителя вам не нужно устанавливать драйверы вручную.

## <a name="getting-started"></a>Приступая к работе

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к соединителю ServiceNow.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы ServiceNow поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно |
|:--- |:--- |:--- |
| Тип | Для свойства type нужно задать значение **ServiceNow**. | Yes |
| endpoint | Конечная точка сервера ServiceNow (`http://ServiceNowData.com`).  | Yes |
| authenticationType | Тип проверки подлинности. <br/>Допустимые значения: **Basic**, **OAuth2**. | Yes |
| Имя пользователя | Имя пользователя, используемое для подключения к серверу ServiceNow для обычной проверки подлинности и OAuth2.  | Нет  |
| password | Пароль, соответствующий имени пользователя для обычной проверки подлинности и OAuth2. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет  |
| clientid | Идентификатор клиента для проверки подлинности OAuth2.  | Нет  |
| clientSecret | Секрет клиента для проверки подлинности OAuth2. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет  |
| useEncryptedEndpoints | Указывает, шифруются ли конечные точки источника данных с помощью протокола HTTPS. По умолчанию используется значение true.  | Нет  |
| useHostVerification | Указывает, следует ли требовать, чтобы имя узла в сертификате сервера совпадало с именем узла сервера при подключении по протоколу SSL. По умолчанию используется значение true.  | Нет  |
| usePeerVerification | Указывает, следует ли проверять удостоверение сервера при подключении по протоколу SSL. По умолчанию используется значение true.  | Нет  |

**Пример.**

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://ServiceNowData.com",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). Этот раздел содержит список свойств, поддерживаемых набором данных ServiceNow.

Чтобы скопировать данные из ServiceNow, установите свойство type набора данных **ServiceNowObject**. В этом типе набора данных нет дополнительных свойств для определенного типа.

**Пример**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником ServiceNow.

### <a name="servicenow-as-source"></a>ServiceNow в качестве источника

Чтобы копировать данные из ServiceNow, установите тип источника **ServiceNowSource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно |
|:--- |:--- |:--- |
| Тип | Свойство type источника действия копирования должно иметь значение **ServiceNowSource**. | Yes |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например, `"SELECT * FROM Actual.alm_asset"`. | Yes |

Указывая схему и столбец для ServiceNow в запросе, обратите внимание на следующее:

- **Схема.** Для создания запроса к ServiceNow необходимо указать схему как `Actual` или `Display`, что можно рассматривать как параметр `sysparm_display_value` со значением true или false при вызове [REST API ServiceNow](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET). 
- **Столбец.** Имя столбца для фактического значения — это `[columne name]_value`, а отображаемое имя — это `[columne name]_display_value`.

**Пример запроса:**
`SELECT distinct col_value, col_display_value FROM Actual.alm_asset` или `SELECT distinct col_value, col_display_value FROM Display.alm_asset`

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "ServiceNowSource",
                "query": "SELECT * FROM Actual.alm_asset"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Дополнительная информация
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
