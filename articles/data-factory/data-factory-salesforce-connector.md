---
title: "Перемещение данных из Salesforce с помощью фабрики данных | Документация Майкрософт"
description: "Узнайте, как перемещать данные из Salesforce с использованием фабрики данных Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 6ec8ac288a4daf6fddd6d135655e62fad7ae17c2
ms.openlocfilehash: 51325cf5f473123c5efeb571f52e04b540b182ad


---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Перемещение данных из Salesforce с помощью фабрики данных Azure
В этой статье описано, как переместить данные из Salesforce в любое хранилище данных, указанное в таблице [поддерживаемых хранилищ данных](data-factory-data-movement-activities.md#supported-data-stores-and-formats) (столбец "Приемник"), с использованием действия копирования в фабрике данных Azure. Эта статья основана на статье о [действиях перемещения данных](data-factory-data-movement-activities.md) , в которой приведены общие сведения о перемещении данных с помощью действия копирования и поддерживаемых сочетаниях хранилищ данных.

Сейчас фабрика данных Azure поддерживает только перемещение данных из Salesforce в другие [поддерживаемые хранилища-приемники](data-factory-data-movement-activities.md#supported-data-stores-and-formats), но не наоборот.

## <a name="supported-versions"></a>Поддерживаемые версии
Этот соединитель поддерживает следующие выпуски Salesforce: Developer Edition, Professional Edition, Enterprise Edition или Unlimited Edition.

## <a name="prerequisites"></a>Предварительные требования
* Требуется включить разрешения API. Дополнительные сведения о включении доступа к API в Salesforce с помощью набора разрешений см. [здесь](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/).
* Чтобы скопировать данные из Salesforce в локальное хранилище данных, в локальной среде необходимо установить шлюз управления данными версии не ниже 2.0.

## <a name="salesforce-request-limits"></a>Ограничения запросов Salesforce
Для Salesforce установлены ограничения на общее число запросов API и одновременных запросов API. Дополнительные сведения см. в разделе "API Request Limits" (Ограничения запросов API) статьи об [ограничениях для разработчика Salesforce](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf). Обратите внимание, что если количество одновременных запросов превышает ограничение, выполняется регулирование и возникают случайные ошибки. В случае превышения ограничения на общее число запросов учетная запись Salesforce блокируется на 24 часа. Кроме того, в обоих случаях вы можете получить ошибку REQUEST_LIMIT_EXCEEDED.

## <a name="copy-data-wizard"></a>Мастер копирования данных
Самый простой способ создать конвейер, копирующий данные из Salesforce в любое поддерживаемое хранилище-приемник, — использовать мастер копирования данных. В статье [Руководство. Создание конвейера с действием копирования с помощью мастера копирования фабрики данных](data-factory-copy-data-wizard-tutorial.md) приведены краткие пошаговые указания по созданию конвейера с помощью мастера копирования данных.

Ниже приведены примеры с определениями JSON, которые можно использовать для создания конвейера с помощью [портала Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) или [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Вы узнаете, как копировать данные из Salesforce в хранилище BLOB-объектов Azure. Тем не менее данные можно копировать в любой из указанных [здесь](data-factory-data-movement-activities.md#supported-data-stores-and-formats) приемников. Это делается с помощью действия копирования в фабрике данных Azure.   

## <a name="sample-copy-data-from-salesforce-to-an-azure-blob"></a>Пример копирования данных из Salesforce в большой двоичный объект Azure
В этом примере данные из Salesforce каждый час копируются в большой двоичный объект Azure. Используемые в этих примерах свойства JSON описаны в разделах после примеров. Данные можно напрямую копировать в любые приемники, указанные в статье о [действиях перемещения данных](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Для этого применяется действие копирования в фабрике данных Azure.

Ниже приведены артефакты фабрики данных, которые необходимо создать для реализации сценария. В разделах после списка приведены подробные сведения об этих действиях.

* Связанная служба типа [Salesforce](#salesforce-linked-service-properties)
* Связанная служба типа [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
* Входной [набор данных](data-factory-create-datasets.md) типа [RelationalTable](#salesforce-dataset-properties).
* Выходной [набор данных](data-factory-create-datasets.md) типа [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
* [Конвейер](data-factory-create-pipelines.md) с действием копирования, в котором используются [RelationalSource](#relationalsource-type-properties) и [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Связанная служба SalesForce**

В этом примере используется связанная служба **Salesforce**. Список свойств, поддерживаемых этой связанной службой, приведен в разделе [Свойства связанной службы Salesforce](#salesforce-linked-service-properties).  Инструкции по получению и сбросу маркера безопасности см. в статье [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) (Получение маркера безопасности).

    {
        "name": "SalesforceLinkedService",
        "properties":
        {
            "type": "Salesforce",
            "typeProperties":
            {
                "username": "<user name>",
                "password": "<password>",
                "securityToken": "<security token>"
            }
        }
    }

**Связанная служба хранения Azure**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Входной набор данных Salesforce**

    {
        "name": "SalesforceInput",
        "properties": {
            "linkedServiceName": "SalesforceLinkedService",
            "type": "RelationalTable",
            "typeProperties": {
                "tableName": "AllDataType__c"  
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Если для параметра **external** задать значение **true**, то фабрика данных воспримет этот набор данных как внешний, который создан не в результате какого-либо действия в фабрике данных.

> [!IMPORTANT]
> Имя API для любых настраиваемых объектов должно содержать приставку __c.
>
>

![Фабрика данных — подключение к Salesforce — имя API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Выходной набор данных большого двоичного объекта Azure**

Данные записываются в новый BLOB-объект каждый час (frequency: hour, interval: 1).

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/alltypes_c"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Конвейер с действием копирования**

Конвейер содержит действие копирования, которое использует входной и выходной наборы данных (см. выше) и выполняется каждый час. В определении JSON конвейера для типа **source** установлено значение **RelationalSource**, а для типа **sink** — значение **BlobSink**.

Список свойств, поддерживаемых RelationalSource, см. в разделе [Свойства типа RelationalSource](#relationalsource-type-properties).

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "SalesforceToAzureBlob",
                "description": "Copy from Salesforce to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "SalesforceInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]
        }
    }

> [!IMPORTANT]
> Имя API для любых настраиваемых объектов должно содержать приставку __c.
>
>

![Фабрика данных — подключение к Salesforce — имя API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="salesforce-linked-service-properties"></a>Свойства связанной службы Salesforce
В таблице ниже приведены описания элементов JSON, которые относятся к связанной службе Salesforce.

| Свойство | Описание | Обязательно |
| --- | --- | --- |
| type |Для свойства type нужно задать значение **Salesforce**. |Да |
| username |Укажите имя пользователя для учетной записи пользователя. |Да |
| password |Укажите пароль для учетной записи пользователя. |Да |
| securityToken |Укажите маркер безопасности для учетной записи пользователя. Инструкции по получению и сбросу маркера безопасности см. в статье [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) (Получение маркера безопасности). Общие сведения о маркере безопасности см. в статье [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm) (Безопасность и API). |Да |

## <a name="salesforce-dataset-properties"></a>Свойства типа "Набор данных Salesforce"
Полный список разделов и свойств, используемых для определения наборов данных, см. в статье [Наборы данных в фабрике данных Azure](data-factory-create-datasets.md). Разделы structure, availability и policy JSON набора данных одинаковы для всех типов наборов данных (SQL Azure, большие двоичные объекты Azure, таблицы Azure и т. д.).

Раздел **typeProperties** во всех типах наборов данных разный. В нем содержатся сведения о расположении данных в хранилище данных. Раздел typeProperties для набора данных типа **RelationalTable** содержит следующие свойства.

| Свойство | Описание | Обязательно |
| --- | --- | --- |
| tableName |Имя таблицы в Salesforce |Нет (если для свойства **RelationalSource** задано значение **query**). |

> [!IMPORTANT]
> Имя API для любых настраиваемых объектов должно содержать приставку __c.
>
>

![Фабрика данных — подключение к Salesforce — имя API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="relationalsource-type-properties"></a>Свойства типа RelationalSource
Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure: создание конвейеров, цепочки действий и расписаний для них](data-factory-create-pipelines.md). Такие свойства, как имя, описание, входные и выходные таблицы, различные политики, доступны для всех типов действий.

С другой стороны, свойства, доступные в разделе typeProperties действия, зависят от конкретного типа действия. Для действия копирования они различаются в зависимости от типов источников и приемников.

В случае действия копирования, если источник относится к типу **RelationalSource** (который содержит Salesforce), в разделе typeProperties доступны следующие свойства.

| Свойство | Описание | Допустимые значения | Обязательно |
| --- | --- | --- | --- |
| query |Используйте пользовательский запрос для чтения данных. |Запрос SQL-92 или запрос, написанный на [объектно-ориентированном языке запросов Salesforce (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) . Пример: `select * from MyTable__c`. |Нет (если для свойства **tableName** задано значение **dataset**). |

> [!IMPORTANT]
> Имя API для любых настраиваемых объектов должно содержать приставку __c.
>
>

![Фабрика данных — подключение к Salesforce — имя API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Советы по запросам
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Получение данных с использованием предложения WHERE в столбце даты и времени
При указании запроса SOQL или SQL обратите внимание на различие в формате даты и времени. Например:

* **Пример SOQL**: $$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd).
* **Пример SQL**: $$Text.Format('SELECT * FROM Account  WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate  < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`.

### <a name="retrieving-data-from-salesforce-report"></a>Извлечение данных из отчета Salesforce
Из отчетов Salesforce можно извлекать данные, указывая запросы в формате `{call "<report name>"}`, например: `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Восстановление удаленных записей из корзины Salesforce
Чтобы запросить из корзины Salesforce обратимо удаленные записи, укажите в своем запросе **"IsDeleted = 1"**. Например,

* чтобы запросить только удаленные записи, укажите "select * from MyTable__c **where IsDeleted= 1**"
* Чтобы запросить все записи, включая существующие и удаленные, укажите "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-salesforce"></a>Сопоставление типов для Salesforce
| Тип данных Salesforce | Тип на основе .NET |
| --- | --- |
| Автонумерация |Строка |
| Флажок |Логический |
| Валюта |Double |
| Дата |DateTime |
| Дата и время |DateTime |
| Email |Строка |
| Идентификатор |Строка |
| Связь для подстановки |Строка |
| Список множественного выбора |Строка |
| Number |Double |
| Процент |Double |
| Номер телефона |Строка |
| Список выбора |Строка |
| текст |Строка |
| Текстовое поле |Строка |
| Текстовое поле (длинное) |Строка |
| Текстовое поле (расширенное) |Строка |
| Текст (зашифрованный) |Строка |
| URL-адрес |Строка |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Производительность и настройка
Ознакомьтесь с [руководством по настройке производительности действия копирования](data-factory-copy-activity-performance.md), в котором описываются ключевые факторы, влияющие на производительность перемещения данных (действие копирования) в фабрике данных Azure, и различные способы оптимизации этого процесса.



<!--HONumber=Nov16_HO3-->


