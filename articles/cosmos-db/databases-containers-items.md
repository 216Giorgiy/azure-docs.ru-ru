---
title: Работа с базами данных, контейнерами и элементами Azure Cosmos DB
description: В этой статье описано, как создать и использовать базы данных, контейнеры и элементы Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 8eaca83b7ea89737a63fe56a18505c8df7e93fdc
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678808"
---
# <a name="work-with-databases-containers-and-items"></a>Работа с базами данных, контейнерами и элементами

После создания [учетную запись Azure Cosmos](account-overview.md) в вашей подписке Azure, вы можете управлять данными в вашей учетной записи путем создания базы данных, контейнеры и элементы. В этой статье описаны все эти сущности: базы данных, контейнеры и элементы. Ниже показана иерархия разных сущностей в учетной записи Azure Cosmos:

![Сущности учетной записи Azure Cosmos](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Базы данных Azure Cosmos

Вы можете создать одну или несколько баз данных Azure Cosmos в рамках своей учетной записи. Базы данных является аналогом пространства имен. Это единица управления для набора контейнеры Azure Cosmos. В следующей таблице показано, как база данных Azure Cosmos соотносится с разными сущностями определенных API:

| **Сущность Azure Cosmos DB** | **API SQL** | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- |
|База данных Azure Cosmos | База данных | Пространство ключей | База данных | База данных | Нет данных |

> [!NOTE]
> С помощью учетных записей API таблиц при создании первой таблицы, базу данных по умолчанию создается автоматически в рамках учетной записи Azure Cosmos.

### <a name="operations-on-an-azure-cosmos-database"></a>Операции с базой данных Azure Cosmos

Вы можете взаимодействовать с базой данных Azure Cosmos с помощью API-интерфейсов Cosmos Azure следующим образом:

| **Операция** | **Интерфейс командной строки Azure**|**API SQL**; | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- | --- |
|Перечисление всех баз данных| Yes | Yes | Да (база данных соответствует пространству имен) | Yes | Нет данных | Нет данных |
|Чтение базы данных| Yes | Yes | Да (база данных соответствует пространству имен) | Yes | Нет данных | Нет данных |
|Создание базы данных| Yes | Yes | Да (база данных соответствует пространству имен) | Yes | Нет данных | Нет данных |
|Обновление базы данных| Yes | Yes | Да (база данных соответствует пространству имен) | Yes | Нет данных | Нет данных |


## <a name="azure-cosmos-containers"></a>Контейнеры Azure Cosmos

Контейнер Azure Cosmos — это единица масштабируемости для хранилища и подготовленной пропускной способности. Контейнер горизонтально секционируется и затем реплицируются между несколькими регионами. Добавленные в контейнер элементы и подготовленная для него пропускная способность автоматически распределяются в пределах набора логических секций на основе ключа секции. Дополнительные сведения о секционировании и ключах секций, см. в разделе [это](partition-data.md) статьи. 

При создании контейнера Azure Cosmos пропускную способность можно настроить для использования в одном из следующих режимов:

* Режим **выделенной подготовленной пропускной способности**. Пропускная способность для контейнера зарезервировано исключительно для этого контейнера, и оно поддерживается соглашения об уровне обслуживания. См. дополнительные сведения о том, как [подготовить пропускную способность для контейнера Azure Cosmos](how-to-provision-container-throughput.md).

* Режим **общей подготовленной пропускной способности**. Эти контейнеры совместно используют подготовленной пропускной способности с другими контейнерами в той же базе данных (за исключением этих контейнеров, которые были настроены с выделенной подготовленной пропускной способности). Другими словами подготовленной пропускной способности в базе данных является общим для всех контейнеров «общей пропускной способности». См. дополнительные сведения о том, как [настроить подготовленную пропускную способность для базы данных Azure Cosmos](how-to-provision-database-throughput.md).

Контейнеры Cosmos Azure поддерживают эластичное масштабирование независимо от выбранного режима подготовленной пропускной способности (выделенной или общей).

Контейнер Azure Cosmos — это контейнер элементов, не зависящий от схем. Элементы в контейнере могут иметь произвольные схемы. Например, элемент, представляющий человека, элемент, представляющий автомобиля может размещаться в *том же контейнере*. По умолчанию все элементы, добавляемые в контейнер, автоматически индексируются без необходимости явно управлять индексом или схемой. Можно настроить поведение индексирования, настроив [политика индексации](index-overview.md) в контейнере. 

Можно задать [время жизни (TTL)](time-to-live.md) для выбранных элементов в контейнере Azure Cosmos или для всего контейнера, чтобы корректно удалить эти элементы из системы. Azure Cosmos DB будет автоматически удалять элементы после истечения срока их жизни. Также гарантируется, что запрос к контейнеру не будет возвращать элементы с истекшим сроком жизни в пределах фиксированных границ. См. дополнительные сведения о [настройке срока жизни для контейнера](how-to-time-to-live.md).

С помощью [веб-канала изменений](change-feed.md), вы можете подписаться на журнал операций, который управляется для каждого из логических разделов контейнера. Канал изменений предоставляет журнал всех обновлений, выполненных в контейнере вместе с разнесенными во времени сведениями об измененных элементах. См. в разделе [как построить реактивных приложений с помощью веб-канала изменений](serverless-computing-database.md). Можно также настроить длительность хранения для изменения веб-канала с помощью политики для контейнера канал изменений. 

Вы можете зарегистрировать [хранимые процедуры, триггеры, определяемые пользователем функции (UDF)](stored-procedures-triggers-udfs.md) и [слияния процедуры](how-to-manage-conflicts.md) в контейнер Azure Cosmos. 

Можно указать [ограничение уникального ключа](unique-keys.md) на контейнер Azure Cosmos. Создавая политику уникальных ключей, вы гарантируете уникальность одного или нескольких значений ключа логической секции. Созданный с использованием политики уникальных ключей контейнер запрещает создавать новые или обновленные элементы со значениями, которые дублируют значения, заданные с помощью ограничения уникального ключа. См. дополнительные сведения об [ограничениях уникальных ключей](unique-keys.md).

Контейнер Azure Cosmos соотносится с сущностями определенных API следующим образом:

| **Сущность Azure Cosmos DB** | **API SQL** | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- |
|Контейнер Azure Cosmos | Коллекция | Таблица | Коллекция | График | Таблица |

### <a name="properties-of-an-azure-cosmos-container"></a>Свойства контейнера Azure Cosmos

Контейнер Azure Cosmos имеет набор системных свойств. В зависимости от выбора API некоторые из них могут не предоставляться напрямую. В следующей таблице описаны список системных свойств:

| **Системные свойства** | **Системы, созданный или настраивается пользователем** | **Назначение** | **API SQL**; | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_rid | Генерируемое системой | Уникальный идентификатор контейнера. | Yes | Нет  | Нет  | Нет  | Нет  |
|_etag | Генерируемое системой | Тег сущности, используемый для управления оптимистической блокировкой. | Yes | Нет  | Нет  | Нет  | Нет  |
|_ts | Генерируемое системой | Метка времени последнего обновления контейнера. | Yes | Нет  | Нет  | Нет  | Нет  |
|_self | Генерируемое системой | Адресуемый URI контейнера. | Yes | Нет  | Нет  | Нет  | Нет  |
|id | Настраивается пользователем | Определяемое пользователем уникальное имя контейнера. | Yes | Да | Да | Да | Yes |
|indexingPolicy | Настраивается пользователем | Предоставляет возможность изменить путь индекса, тип индекса и режимом индексирования. | Yes | Нет  | Нет  | Нет | Yes |
|TimeToLive | Настраивается пользователем | Позволяет автоматически удалять элементы из контейнера через определенное время. См. дополнительные сведения о [настройке срока жизни](time-to-live.md). | Yes | Нет  | Нет  | Нет | Yes |
|changeFeedPolicy | Настраивается пользователем | Используется для считывания изменений, внесенных в элементы в контейнере. Дополнительные сведения см. в разделе [веб-канала изменений](change-feed.md) статьи. | Yes | Нет  | Нет  | Нет | Yes |
|uniqueKeyPolicy | Настраивается пользователем | Позволяют гарантировать уникальность одного или нескольких значений в логической секции. Дополнительные сведения см. в разделе [ограничения уникального ключа](unique-keys.md) статьи. | Yes | Нет  | Нет  | Нет | Yes |

### <a name="operations-on-an-azure-cosmos-container"></a>Операции с контейнером Azure Cosmos

Контейнер Azure Cosmos поддерживает следующие операции с использованием любого из API Azure Cosmos.

| **Операция** | **Интерфейс командной строки Azure** | **API SQL**; | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- | --- |
| Перечисление контейнеров в базе данных | Yes | Да | Да | Yes | Нет данных | Нет данных |
| Считывание контейнера | Yes | Да | Да | Yes | Нет данных | Нет данных |
| Создание контейнера | Yes | Да | Да | Yes | Нет данных | Нет данных |
| Обновление контейнера | Yes | Да | Да | Yes | Нет данных | Нет данных |
| Удаление контейнера | Yes | Да | Да | Yes | Нет данных | Нет данных |

## <a name="azure-cosmos-items"></a>Элементы Azure Cosmos

В зависимости от используемого API элемент Azure Cosmos может представлять документа в коллекции, строку в таблице или узел (ребро) в графе. В следующей таблице показано, как элемент Azure Cosmos соотносится с разными сущностями определенных API:

| **Сущность Azure Cosmos DB** | **API SQL** | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- |
|Элемент Azure Cosmos | Документ | Строка | Документ | Узел или ребро | Элемент |

### <a name="properties-of-an-item"></a>Свойства элемента

Каждый элемент Azure Cosmos имеет следующие системные свойства. В зависимости от выбора API некоторые из них могут не предоставляться напрямую.

|**Системные свойства** | **Системы, созданный или настраивается пользователем**| **Назначение** | **API SQL**; | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_№ | Генерируемое системой | Уникальный идентификатор элемента. | Yes | Нет  | Нет  | Нет  | Нет  |
|_etag | Генерируемое системой | Тег сущности, используемый для управления оптимистической блокировкой. | Yes | Нет  | Нет  | Нет  | Нет  |
|_ts | Генерируемое системой | Отметка времени последнего обновления элемента | Yes | Нет  | Нет  | Нет  | Нет  |
|_self | Генерируемое системой | Адресуемый URI элемента. | Yes | Нет  | Нет  | Нет  | Нет  |
|id | Можно использовать | Определяемое пользователем уникальное имя в пределах логической секции. Если пользователь не укажет идентификатор, система автоматически создаст один. | Yes | Да | Да | Да | Yes |
|Определяемые пользователем свойства | Определяемые пользователем маршруты | Определяемые пользователем свойства в собственном формате API (JSON, BSON, CQL и т. д.). | Yes | Да | Да | Да | Yes |

### <a name="operations-on-items"></a>Операции с элементами

Элемент Azure Cosmos поддерживает следующие операции с использованием любого из API Azure Cosmos.

| **Операция** | **Интерфейс командной строки Azure** | **API SQL**; | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**; | **API таблицы**; |
| --- | --- | --- | --- | --- | --- | --- |
| Вставка, замена, удаление, обновление или вставка, чтение | Нет  | Да | Да | Да | Да | Yes |

## <a name="next-steps"></a>Дальнейшие действия

Теперь можно приступать к следующие понятия:

* [Как настроить подготовленную пропускную способность в базе данных Azure Cosmos](how-to-provision-database-throughput.md)
* [Как настроить подготовленную пропускную способность для контейнера Azure Cosmos](how-to-provision-container-throughput.md)
* [Логические секции](partition-data.md)
* [Настройка срока жизни для контейнера Azure Cosmos](how-to-time-to-live.md)
* [Создание реактивных приложений с помощью канала изменений](change-feed.md)
* [Настройка ограничения уникального ключа для контейнера Azure Cosmos](unique-keys.md)
