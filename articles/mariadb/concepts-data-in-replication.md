---
title: Репликация данных в базу данных Azure для MariaDB.
description: В этой статье описывается настройка репликации входных данных в базу данных Azure для MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0a1ead1580f6764fec7d1d18daa38bf093f242f2
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547615"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>Репликация данных в базу данных Azure для MariaDB

Репликация входных данных позволяет синхронизировать данные сервера MariaDB, работающего локально, на виртуальных машинах или в службах баз данных, размещенных другими облачными поставщиками, со службой базы данных Azure для MariaDB. Репликация входных данных основана на функции собственной репликации в MariaDB на основе позиции файла двоичного журнала (binlog). Дополнительные сведения о репликации binlog см. в [этой статье](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Когда следует использовать репликацию входных данных
Ниже приведены основные сценарии применения репликации входных данных.

- **Гибридная синхронизация данных.** С помощью репликации входных данных можно обеспечить синхронизацию данных между локальными серверами и Базой данных Azure для MariaDB. Эта синхронизация полезна при создании гибридных приложений. Этот метод удобен, если у вас есть существующий локальный сервер базы данных, но вы хотите переместить данные в регион, который расположен ближе к пользователям.
- **Многооблачная синхронизация**. Для сложных облачных решений используйте репликацию входных данных для синхронизации данных между Базой данных Azure для MariaDB и различными облачными поставщиками, включая службы виртуальных машин и баз данных, размещенные в этих облаках.

## <a name="limitations-and-considerations"></a>Ограничения и рекомендации

### <a name="data-not-replicated"></a>Нереплицируемые данные
[*Системная база данных MySQL*](https://mariadb.com/kb/en/library/the-mysql-database-tables/) на главном сервере не реплицируется. Изменения учетных записей и разрешений на главном сервере не реплицируются. Если вы создаете на главном сервере учетную запись, которая должна иметь доступ к серверу-реплике, тогда вручную создайте ту же учетную запись на стороне сервера-реплики. Чтобы узнать, какие таблицы хранятся в системной базе данных, ознакомьтесь с [документацией по MariaDB](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Требования
- На главном сервере должна быть установлена система MariaDB версии не ниже 10.2.
- Версии главного сервера и сервера реплики должны совпадать. Например, обе версии должны быть MariaDB 10.2.
- Каждая таблица должна иметь первичный ключ.
- Главный сервер должен использовать ядро InnoDB.
- Пользователь должен иметь разрешения на настройку ведения двоичного журнала и создания новых пользователей на главном сервере.

### <a name="other"></a>Другие
- Репликация данных поддерживается только в ценовых категориях общего назначения и с оптимизацией для операций в памяти.
- Идентификаторы глобальных транзакций (GTID) не поддерживаются.

## <a name="next-steps"></a>Дополнительная информация
- Узнайте, как [настроить репликацию входных данных](howto-data-in-replication.md).
