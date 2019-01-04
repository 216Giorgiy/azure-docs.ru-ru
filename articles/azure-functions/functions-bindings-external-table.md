---
title: Привязка внешних таблиц в службе "Функции Azure" (экспериментальная возможность)
description: Использование привязок внешних таблиц в Функциях Azure
services: functions
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: cshoe
ms.openlocfilehash: 62924488b776a1a89e1abf492db1881a44585b1a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997814"
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Привязка внешних таблиц в службе "Функции Azure" (экспериментальная возможность)

В этой статье объясняется, как работать с табличными данными у различных поставщиков SaaS, таких как SharePoint и Dynamics, в службе "Функции Azure". Служба "Функции Azure" поддерживают входные и выходные привязки внешних таблиц.

> [!IMPORTANT]
> Привязка внешних таблиц — это экспериментальная возможность, общедоступная версия которой, возможно, никогда не выйдет. Она доступна только в службе "Функции Azure" 1.x, и мы не планируем добавлять ее к версии 2.x этого решения. В сценариях с доступом к данным у поставщиков SaaS следует использовать [приложения логики, которые вызывают функции](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>Подключения API

Привязки таблиц проверяют подлинность у сторонних поставщиков SaaS через внешние подключения API. 

При назначении привязки можно создать подключение API или использовать имеющееся в той же группе ресурсов.

### <a name="available-api-connections-tables"></a>Доступные подключения API (таблицы)

|Соединитель|Триггер|Входные данные|Выходные данные|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 for Operations](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Таблицы Google](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 for Financials](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[База данных Oracle](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Common Data Service](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](https://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> В [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list) могут также использоваться подключения к внешним таблицам.

## <a name="creating-an-api-connection-step-by-step"></a>Пошаговые инструкции по созданию подключения API

1. На странице приложения-функции на портале Azure щелкните знак "плюс" (**+**), чтобы создать функцию.

1. В поле **Сценарий** выберите **Экспериментальный**.

1. Выберите **Внешняя таблица**.

1. Выберите язык.

2. В разделе **Подключение к внешней таблице** **создайте** подключение или выберите имеющееся.

1. Настройте параметры нового подключения и выберите **Авторизовать**.

1. Щелкните **Создать**, чтобы создать функцию.

1. Выберите **Интегрировать > Внешняя таблица**.

1. Настройте подключение так, чтобы использовать в нем целевую таблицу. Эти параметры варьируются у разных поставщиков SaaS. Примеры приведены в следующем разделе.

## <a name="example"></a>Пример

В этом примере мы установим подключение к таблице Contact со столбцами Id, LastName и FirstName. В коде перечислены сущности таблицы Contact, а также записаны имена и фамилии.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```

Ниже приведен код скрипта C#.

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retrieve table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

### <a name="sql-server-data-source"></a>Источник данных SQL Server

Ниже приведен сценарий, с помощью которого можно создать таблицу на платформе SQL Server и использовать ее с этим примером. Параметр `dataSetName` имеет значение default.

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets-data-source"></a>Источник данных Google Таблицы

Чтобы создать таблицу и применить ее в этом примере с Google Таблицами, создайте электронную таблицу с листом `Contact`. Соединитель не может использовать отображаемое имя листа. Внутреннее имя (выделено полужирным шрифтом) должно использоваться как dataSetName, например: `docs.google.com/spreadsheets/d/`**`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Добавьте имена столбцов `Id`, `LastName` и `FirstName` в первую строку, а затем заполните следующие строки.

### <a name="salesforce"></a>Salesforce

Чтобы использовать этот пример с Salesforce, параметр `dataSetName` имеет значение default.

## <a name="configuration"></a>Параметр Configuration

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json*.

|свойство function.json | ОПИСАНИЕ|
|---------|----------------------|
|**type** | Нужно задать значение `apiHubTable`. Это свойство задается автоматически при создании триггера на портале Azure.|
|**direction** | Нужно задать значение `in`. Это свойство задается автоматически при создании триггера на портале Azure. |
|**name** | Имя переменной, представляющей элемент события в коде функции. | 
|**подключение**| Определяет параметр приложения, в котором хранится строка подключения API. Параметр приложения создается автоматически при добавлении подключения API в интерфейсе интеграции.|
|**dataSetName**|Имя набора данных с таблицей для чтения.|
|**tableName**|Имя таблицы.|
|**entityId**|Должен быть пустым для привязок таблиц.

Табличный соединитель предоставляет наборы данных, и каждый набор данных содержит таблицы. По умолчанию задано имя набора данных default. Ниже перечислены заголовки для набора данных и таблицы у различных поставщиков SaaS.

|Соединитель|Выборка|Таблица|
|:-----|:---|:---| 
|**SharePoint**|Сайт|Список SharePoint
|**SQL**|База данных|Таблица 
|**Таблица Google**|Электронная таблица|Лист 
|**Excel**|Файл Excel|Лист 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)
