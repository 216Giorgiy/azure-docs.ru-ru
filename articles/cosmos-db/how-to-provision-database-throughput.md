---
title: Подготовка пропускной способности базы данных в Azure Cosmos DB
description: Узнайте, как подготовить пропускную способность на уровне базы данных в Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: c648522e689c64de8e7e09b85ca3b6eb26b6945b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477205"
---
# <a name="provision-throughput-on-an-azure-cosmos-database"></a>Подготовка пропускной способности для базы данных Azure Cosmos

В этой статье объясняется, как подготовить пропускную способность для базы данных в Azure Cosmos DB. Вы можете подготовить пропускную способность для [одного](how-to-provision-container-throughput.md) контейнера или для базы данных и разделить ее между контейнерами внутри базы данных. Вы можете подготовить пропускную способность уровня базы данных с помощью портала Azure или пакетов SDK Cosmos DB.

## <a name="provision-throughput-using-azure-portal"></a>Подготовка пропускной способности с помощью портала Azure

### <a id="portal-sql"></a>API SQL (Core)

1. Войдите на [портал Azure](https://portal.azure.com/).

1. [Создайте новую учетную запись Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) или выберите имеющуюся.

1. Откройте панель **обозревателя данных** и выберите **Новая база данных**. Далее заполните форму следующими данными:

   * Введите идентификатор базы данных. 
   * Выберите "Подготовить пропускную способность".
   * Введите пропускную способность, например 1000 ЕЗ.
   * Нажмите кнопку **ОК**.

![Подготовка пропускной способности базы данных с помощью API SQL](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>Подготовка пропускной способности с помощью пакета SDK для .NET

> [!Note]
> Используйте API SQL, чтобы подготовить пропускную способность для всех интерфейсов API. При необходимости также можно использовать указанный ниже пример для API Cassandra.

### <a id="dotnet-all"></a>Все API

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>API Cassandra

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Дополнительная информация

Чтобы узнать о подготовке пропускной способности в Cosmos DB, обратитесь к следующим статьям:

* [Provision throughput for an Azure Cosmos DB container](how-to-provision-container-throughput.md) (Подготовка пропускной способности для контейнера Azure Cosmos DB)
* [Пропускная способность и единицы запросов в Azure Cosmos DB](request-units.md)
