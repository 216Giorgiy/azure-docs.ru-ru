---
title: Различия T-SQL Управляемого экземпляра Базы данных SQL Azure | Документация Майкрософт
description: В этой статье обсуждаются различия T-SQL между Управляемым экземпляром Базы данных SQL Azure и SQL Server
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 489eccf1b73e7f5df76a3ce681b4479893a9e0ac
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843212"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Различия T-SQL между Управляемым экземпляром Базы данных SQL Azure и SQL Server

Управляемый экземпляр Базы данных SQL Azure обеспечивает высокий уровень совместимости с локальным ядром СУБД SQL Server. В Управляемом экземпляре поддерживается большинство функций SQL Server. Так как в синтаксисе и поведении все еще есть некоторые различия, они перечислены и описаны в этой статье.

- [Различия T-SQL и неподдерживаемые функции](#Differences)
- [Функции с другим поведением в Управляемом экземпляре](#Changes)
- [Временные ограничения и известные проблемы](#Issues)

## <a name="Differences"></a> Отличия T-SQL от базы данных SQL

В этом разделе приведены основные различия в синтаксисе T-SQL и поведении между Управляемым экземпляром и локальным яром СУБД SQL Server, а также неподдерживаемые функции.

### <a name="always-on-availability"></a>Доступность Always On

[Высокий уровень доступности](sql-database-high-availability.md) встроен в Управляемый экземпляр, и пользователи не могут им управлять. Следующие инструкции не поддерживаются:

- [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql);
- [CREATE AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql);
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql);
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql);
- предложение [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) инструкции [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql).

### <a name="auditing"></a>Аудит

Ниже перечислены основные различия между аудитом SQL Управляемого экземпляра, базы данных SQL Azure и SQL Server на локальном компьютере

- В Управляемом экземпляре аудит SQL выполняется на уровне сервера и сохраняет файлы `.xel` в учетной записи хранилища BLOB-объектов Azure.  
- В базе данных SQL Azure аудит SQL выполняется на уровне базы данных.
- В локальной или виртуальной машине SQL Server аудит SQL выполняется на уровне сервера, но сохраняет события в журналы событий файловой системы или Windows.  
  
Аудит XEvent в Управляемом экземпляре поддерживает цели хранилища BLOB-объектов Azure. Журналы файлов и Windows не поддерживаются.

Основные различия в синтаксисе `CREATE AUDIT` для аудита в хранилище BLOB-объектов Azure:

- Предоставляется новый синтаксис `TO URL`, который позволяет указать URL-адрес контейнера хранилища BLOB-объектов Azure, куда будут помещены файлы `.xel`.
- Синтаксис `TO FILE` не поддерживается, так как Управляемый экземпляр не может получить доступ к файловым ресурсам Windows.

Дополнительные сведения можно найти в разделе   

- [CREATE SERVER AUDIT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER AUDIT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Аудит](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="backup"></a>Azure Backup

В Управляемом экземпляре предусмотрено автоматическое резервное копирование, и пользователи могут создавать полные резервные копии `COPY_ONLY` базы данных. Резервные копирования моментальных снимков разностных данных, журналов и файлов не поддерживаются.

- Управляемый экземпляр может выполнять резервное копирование базы данных только в учетную запись хранилища BLOB-объектов.
  - Поддерживается только `BACKUP TO URL`.
  - `FILE`, `TAPE` и устройства резервного копирования не поддерживаются.  
- Большинство общих параметров `WITH` поддерживаются:
  - `COPY_ONLY` является обязательным параметром.
  - `FILE_SNAPSHOT` не поддерживается.
  - Параметры ленты: `REWIND`, `NOREWIND`, `UNLOAD` и `NOUNLOAD` не поддерживаются.
  - Параметры журналов: `NORECOVERY`, `STANDBY` и `NO_TRUNCATE` не поддерживаются.

Ограничения:  

- Управляемый экземпляр может выполнять резервное копирование в базу данных, содержащую до 32 полосковых линий. Такого количества достаточно для баз данных объемом до 4 ТБ, если используется сжатие резервных копий.
- Максимальный размер полосковой линии резервного копирования — 195 ГБ (максимальный размер большого двоичного объекта). Увеличьте количество полосковых линий в команде резервного копирования, чтобы уменьшить размер отдельных полосковых линий и не превышать это ограничение.

> [!TIP]
> Чтобы обойти это ограничение локально, выполните резервное копирование в `DISK` вместо `URL`, отправьте файл резервной копии в большой двоичный объект, а затем восстановите его. Восстановление поддерживает файлы большего размера, так как используется другой тип большого двоичного объекта.  

Сведения о резервном копировании с помощью T-SQL см. в статье [BACKUP (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>Расширение буферного пула

- [Расширение буферного пула](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) не поддерживается.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` не поддерживается. См. статью [ALTER SERVER CONFIGURATION (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="bulk-insert--openrowset"></a>Массовая вставка или OpenRowset

У Управляемого экземпляра нет доступа к файловым ресурсам и папкам Windows, поэтому файлы необходимо импортировать из хранилища BLOB-объектов Azure:

- Во время импорта файлов из хранилища BLOB-объектов Azure `DATASOURCE` является обязательным в команде `BULK INSERT`. См. статью [BULK INSERT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- При считывании содержимого файла из хранилища BLOB-объектов Azure `DATASOURCE` является обязательным в функции `OPENROWSET`. См. статью [OPENROWSET (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="certificates"></a>Сертификаты

У Управляемого экземпляра нет доступа к файловым ресурсам и папкам Windows, поэтому действуют следующие ограничения:

- Файл `CREATE FROM`/`BACKUP TO` не поддерживается для сертификатов.
- Сертификат `CREATE`/`BACKUP` из `FILE`/`ASSEMBLY` не поддерживается. Невозможно использовать файлы закрытых ключей.  

См. статьи [Инструкция CREATE CERTIFICATE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) и [BACKUP CERTIFICATE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
**Обходной путь**: создайте скрипт для сертификата и закрытого ключа, сохраните в виде SQL-файла и создайте из двоичного файла:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="clr"></a>Среда CLR

У Управляемого экземпляра нет доступа к файловым ресурсам и папкам Windows, поэтому действуют следующие ограничения:

- Поддерживается только `CREATE ASSEMBLY FROM BINARY`. См. статью [CREATE ASSEMBLY (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` не поддерживается. См. статью [CREATE ASSEMBLY (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` не может ссылаться на файлы. См. статью [ALTER ASSEMBLY (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="compatibility-levels"></a>Уровни совместимости

- Поддерживаемые уровни совместимости: 100, 110, 120, 130 и 140.  
- Уровни совместимости ниже 100 не поддерживаются.
- Уровень совместимости по умолчанию для новых баз данных — 140. Для восстановленных баз данных уровень совместимости остается неизменным, если он был 100 и выше.

См. статью [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level)

### <a name="credential"></a>Учетные данные

Поддерживаются удостоверения только Azure Key Vault и `SHARED ACCESS SIGNATURE`. Пользователи Windows не поддерживаются.

См. статьи [CREATE CREDENTIAL (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) и [ALTER CREDENTIAL (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Поставщики служб шифрования

У Управляемого экземпляра нет доступа к файлам, поэтому невозможно создать поставщики служб шифрования:

- `CREATE CRYPTOGRAPHIC PROVIDER` не поддерживается. См. статью [CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` не поддерживается. См. статью [ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="collation"></a>Параметры сортировки

Параметр сортировки экземпляра по умолчанию — `SQL_Latin1_General_CP1_CI_AS`. Этот параметр можно указать как параметр создания. См. статью [Параметры сортировки](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="database-options"></a>Параметры базы данных

- Несколько файлов журнала не поддерживаются.
- Объекты в памяти не поддерживаются на уровне службы общего назначения.  
- Для одного экземпляра действует ограничение в 280 файлов. Это значит, что в каждой базе данных может быть не больше 280 файлов. Это ограничение распространяется на файлы данных и файлы журнала.  
- База данных не может содержать файловые группы, содержащие данные файлового потока.  Восстановление завершится со сбоем, если BAK-файл содержит данные `FILESTREAM`.  
- Каждый файл помещается в хранилище Azure класса Premium. Количество операций ввода-вывода и пропускная способность для каждого файла зависят от размера каждого файла, также как и для дисков хранилища Azure класса Premium. См. раздел [Размеры диска хранилища класса "Премиум"](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes).  

#### <a name="create-database-statement"></a>Инструкция CREATE DATABASE

Ниже приведены ограничения `CREATE DATABASE`:

- Невозможно определить файлы и файловые группы.  
- Параметр `CONTAINMENT` не поддерживается.  
- Параметры `WITH` не поддерживаются.  
   > [!TIP]
   > Чтобы обойти эту проблему, используйте `ALTER DATABASE` после `CREATE DATABASE`, чтобы задать параметры базы данных для добавления файлов или задать вложение.  

- Параметр `FOR ATTACH` не поддерживается.
- Параметр `AS SNAPSHOT OF` не поддерживается.

Дополнительные сведения см. в статье [CREATE DATABASE (SQL Server Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Инструкция ALTER DATABASE

Невозможно задать или изменить некоторые свойства файла:

- Путь к файлу невозможно указать в инструкции T-SQL `ALTER DATABASE ADD FILE (FILENAME='path')`. Удалите `FILENAME` из скрипта, так как Управляемый экземпляр автоматически помещает файлы.  
- Невозможно изменить имя файла с помощью инструкции `ALTER DATABASE`.

Следующие параметры задаются по умолчанию и не могут быть изменены.

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

Невозможно изменить следующие параметры:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Изменение имени не поддерживается.

Дополнительные сведения см. в статье [Параметры инструкции ALTER DATABASE для файлов и файловых групп (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Зеркальное отображение базы данных

Зеркальное отображение базы данных не поддерживается.

- Параметры `ALTER DATABASE SET PARTNER` и `SET WITNESS` не поддерживаются.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` не поддерживается.

Дополнительные сведения см. в статьях [Зеркальное отображение базы данных ALTER DATABASE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) и [CREATE ENDPOINT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC

Недокументированные инструкции DBCC, которые включены в SQL Server, не поддерживаются в Управляемом экземпляре.

- `Trace Flags` не поддерживаются. См. статью [DBCC TRACEON - флаги трассировки (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` не поддерживается. См. статью [DBCC TRACEOFF (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` не поддерживается. См. статью [DBCC TRACEON (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Распределенные транзакции

В Управляемом экземпляре сейчас не поддерживаются ни MSDTC, ни [эластичные транзакции](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview).

### <a name="extended-events"></a>Расширенные события

Некоторые из целевых объектов Windows для XEvents не поддерживаются:

- `etw_classic_sync target` не поддерживается. Храните файлы `.xel` в хранилище BLOB-объектов Azure. См. раздел [Целевой объект etw_classic_sync_target](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target).
- `event_file target` не поддерживается. Храните файлы `.xel` в хранилище BLOB-объектов Azure. См. раздел [Целевой объект event_file](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Внешние библиотеки

Внешние библиотеки R и Python в базе данных еще не поддерживаются. См. статью [Изучение служб машины SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>Файловый поток и FileTable

- Данные файлового потока не поддерживаются.
- База данных не может содержать файловые группы с данными `FILESTREAM`.
- `FILETABLE` не поддерживается.
- Таблицы не могут содержать типы `FILESTREAM`.
- Следующие функции не поддерживаются:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Дополнительные сведения см. в статьях [FILESTREAM (SQL Server)](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) и [Таблицы FileTable (SQL Server)](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Полнотекстовый семантический поиск

[Семантический поиск](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) не поддерживается.

### <a name="linked-servers"></a>Связанные службы

Связанные серверы в Управляемом экземпляре поддерживают ограниченное число целевых объектов:

- Поддерживаемые целевые объекты: SQL Server и База данных SQL.
- Неподдерживаемые целевые объекты: файлы, Analysis Services и другие реляционные СУБД.

Операции

- Транзакции записи между несколькими экземплярами не поддерживаются.
- `sp_dropserver` поддерживается для удаления связанного сервера. См. статью [sp_dropserver (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- Функцию `OPENROWSET` можно использовать для выполнения запросов только в экземплярах SQL Server (управляемом, локальном или на виртуальных машинах). См. статью [OPENROWSET (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- Функцию `OPENDATASOURCE` можно использовать для выполнения запросов только в экземплярах SQL Server (управляемом, локальном или на виртуальных машинах). В качестве поставщика поддерживаются только значения `SQLNCLI`, `SQLNCLI11` и `SQLOLEDB`. Например, `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. См. статью [OPENDATASOURCE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="logins--users"></a>Имена для входа и пользователи

- Поддерживаются имена для входа SQL, созданные с помощью `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` и `FROM SID`. См. статью [CREATE LOGIN (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Имена для входа Azure Active Directory (AAD), созданные с помощью синтаксиса [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) или [CREATE USER](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current), поддерживаются (**общедоступная предварительная версия**).
- Имена для входа Windows, созданные с помощью синтаксиса `CREATE LOGIN ... FROM WINDOWS`, не поддерживаются. Используйте пользователей и имена для входа Azure Active Directory.
- Пользователь Azure Active Directory (Azure AD), создавший экземпляр, обладает [неограниченными правами доступа администратора](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Пользователей уровня базы данных Azure Active Directory (Azure AD) без прав администратора можно создать, используя синтаксис `CREATE USER ... FROM EXTERNAL PROVIDER`. См. раздел [Пользователи без прав администратора](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users).

### <a name="polybase"></a>PolyBase

Внешние таблицы, ссылающиеся на файлы в HDFS или хранилище BLOB-объектов Azure, не поддерживаются. Сведения о Polybase см. в статье [Руководство по PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Репликация

Репликация доступна в общедоступной предварительной версии Управляемого экземпляра. Дополнительные сведения о репликации см. в статье [Репликация SQL Server](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>Инструкция RESTORE

- Поддерживаемый синтаксис
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Неподдерживаемый синтаксис
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Источник  
  - `FROM URL` (хранилище BLOB-объектов) — единственный поддерживаемый параметр.
  - `FROM DISK`/`TAPE`/устройство резервного копирования не поддерживается.
  - Резервные наборы данных не поддерживаются.
- Параметры `WITH` не поддерживаются (не `DIFFERENTIAL`, `STATS` и т. д.).
- `ASYNC RESTORE` — восстановление продолжится даже в случае разрыва соединения с клиентом. При разрыве соединения можно проверить представление `sys.dm_operation_status` о состоянии операции восстановления (также как и для СОЗДАНИЯ и УДАЛЕНИЯ базы данных). См. статью [sys.dm_operation_status (база данных SQL Azure)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  

Следующие параметры базы данных задаются или переопределяются и не могут быть изменены:  

- `NEW_BROKER` (если брокер не включен в BAK-файл).  
- `ENABLE_BROKER` (если брокер не включен в BAK-файл).  
- `AUTO_CLOSE=OFF` (если база данных в BAK-файле содержит `AUTO_CLOSE=ON`).  
- `RECOVERY FULL` (если база данных в BAK-файле содержит модель восстановления `SIMPLE` или `BULK_LOGGED`).
- Добавляется оптимизированная для операций в памяти файловая группа, которой присваивается имя XTP, если она не содержится в исходном BAK-файле.  
- Любая имеющаяся файловая группа, оптимизированная для операций в памяти, переименовывается на XTP.  
- Параметры `SINGLE_USER` и `RESTRICTED_USER` преобразуются в `MULTI_USER`.

Ограничения:  

- Файлы `.BAK`, содержащие несколько резервных наборов данных, невозможно восстановить.
- Файлы `.BAK`, содержащие несколько файлов журнала, невозможно восстановить.
- Восстановление завершится со сбоем, если BAK-файл содержит данные `FILESTREAM`.
- Резервные копии, содержащие базы данных с объектами In-memory, сейчас восстановить невозможно.  
- Резервные копии, содержащие базы данных, в которых когда-либо хранились объекты In-memory, сейчас восстановить невозможно.
- Резервные копии, содержащие базы данных в режиме только для чтения, сейчас восстановить невозможно. Скоро это ограничение будет снято.

Сведения об инструкции [Restore см. в статье](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Компонент Service broker

Компонент Service Broker между экземплярами не поддерживается:

- `sys.routes`. Обязательно выберите адрес из sys.routes. Адрес должен быть ЛОКАЛЬНЫМ для каждого маршрута. См. статью [sys.routes (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`. `CREATE ROUTE` можно использовать только с локальным (`LOCAL`) адресом `ADDRESS`. См. статью [CREATE ROUTE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`. Для `ALTER ROUTE` адрес `ADDRESS` может быть только локальным (`LOCAL`). См. статью [ALTER ROUTE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

### <a name="service-key-and-service-master-key"></a>Ключ службы и главный ключ службы

- [Резервное копирование главного ключа](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) не поддерживается (управляется службой базы данных SQL).
- [Восстановление главного ключа](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) не поддерживается (управляется службой базы данных SQL).
- [Резервное копирование главного ключа службы](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) не поддерживается (управляется службой базы данных SQL).
- [Восстановление главного ключа службы](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) не поддерживается (управляется службой базы данных SQL).

### <a name="stored-procedures-functions-triggers"></a>Хранимые процедуры, функции и триггеры

- `NATIVE_COMPILATION` сейчас не поддерживается.
- Следующие параметры [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) не поддерживаются:
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `max text repl size`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` не поддерживается. См. раздел [Примеры](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` не поддерживается. См. раздел [xp_cmdshell (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` не поддерживаются, включая `sp_addextendedproc`  и `sp_dropextendedproc`. См. статью [Основные расширенные хранимые процедуры (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- Параметры `sp_attach_db`, `sp_attach_single_file_db` и `sp_detach_db` не поддерживаются. См. статьи [sp_attach_db (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql) и [sp_detach_db (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` не поддерживается. См. статью [sp_renamedb (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>Агент SQL Server

- Параметры агента SQL Server доступны только для чтения. Процедура `sp_set_agent_properties` не поддерживается в Управляемом экземпляре.  
- Задания
  - Шаги задания T-SQL поддерживаются.
  - Поддерживаются следующие задания репликации:
    - Читатель журнала транзакций.  
    - Моментальный снимок.
    - Распространитель.
  - Шаги задания SSIS поддерживаются.
  - Другие типы шагов заданий пока не поддерживаются, включая:
    - Шаг задания репликации слиянием не поддерживается.  
    - Читатель очереди пока не поддерживается.  
    - Командная оболочка пока не поддерживается.
  - У Управляемого экземпляра нет доступа к внешним ресурсам (например, к сетевым папкам через robocopy).  
  - PowerShell пока не поддерживается.
  - Analysis Services не поддерживаются.
- Уведомления поддерживаются частично.
- Поддерживается уведомление по электронной почте. Необходимо настроить профиль компонента Database Mail. Допускается только один профиль компонента Database Mail, и ему должно быть присвоено имя `AzureManagedInstance_dbmail_profile` в общедоступной предварительной версии (временное ограничение).  
  - Пейджер не поддерживается.  
  - NetSend не поддерживается.
  - Оповещения еще не поддерживаются.
  - Прокси-серверы не поддерживаются.  
- Eventlog не поддерживается.

Следующие функции сейчас не поддерживаются, но будут добавлены в будущем:

- прокси-серверы;
- планирование заданий простоя ЦП;
- включение и отключение агента;
- Оповещения

Сведения об агенте SQL Server см. в статье [Агент SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Таблицы

Следующее не поддерживается:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Сведения о создании и изменении таблиц см. в статьях [Инструкция CREATE TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) и [ALTER TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="Changes"></a> Изменения в поведении

Следующие переменные, функции и представления возвращают различные результаты:

- `SERVERPROPERTY('EngineEdition')` возвращает значение 8. Это свойство уникально идентифицирует Управляемый экземпляр. См. статью [SERVERPROPERTY (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` возвращает значение NULL, так как концепция экземпляра в том виде, в котором она существует в SQL Server, не применяется к Управляемому экземпляру. См. статью [SERVERPROPERTY (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` возвращает полное DNS-имя с возможностью подключения, например my-managed-instance.wcus17662feb9ce98.database.windows.net. См. статью [@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` — возвращает полное DNS-имя с возможностью подключения, такое как `myinstance.domain.database.windows.net` для свойств name и data_source. См. статью [sys.servers (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` возвращает значение NULL, так как концепция службы в том виде, в котором она существует в SQL Server, не применяется к Управляемому экземпляру. См. статью [@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` поддерживается. Возвращает значение NULL, если имя для входа AAD не содержится в sys.syslogins. См. статью [Идентификатор SUSER_ID (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` не поддерживается. Возвращает неверные данные (временная известная проблема). См. статью [SUSER_SID (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` и другие встроенные функции даты и времени всегда возвращают время в часовом поясе UTC. См. статью [GETDATE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Известные проблемы и ограничения

### <a name="tempdb-size"></a>Размер tempdb

`tempdb` разбита на 12 файлов. Максимальный размер каждого файла — 14 ГБ. Этот максимальный размер каждого файла невозможно изменить. В `tempdb` невозможно добавить новые файлы. Скоро это ограничение будет снято. Некоторые запросы могут возвращать ошибку, если для них требуется более 168 ГБ в `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Превышение дискового пространства с небольшими файлами баз данных

Для каждого управляемого экземпляра в хранилище резервируется 35 ТБ для диска Azure уровня "Премиум", а каждый файл базы данных размещается на отдельном физическом диске. Поддерживаются диски размером 128 ГБ, 256 ГБ, 512 ГБ, 1 ТБ или 4 ТБ. Неиспользуемое пространство на диске не оплачивается, но суммарный размер дисков Azure уровня "Премиум" не может превышать 35 ТБ. В некоторых случаях из-за внутренней фрагментации размер Управляемого экземпляра, которому требуется не более 8 ТБ дискового пространства, может превысить ограничение в 35 ТБ.

Например, Управляемый экземпляр использует один файл размером 1,2 ТБ, размещенный на диске объемом 4 ТБ, и 248 файлов по 1 ГБ каждый, размещенные на отдельных дисках объемом по 128 ГБ. В этом случае:

- общий размер выделенного дискового хранилища составляет 1 x 4 ТБ + 248 x 128 ГБ = 35 ТБ;
- общий объем зарезервированного пространства для баз данных в экземпляре составляет 1 x 1,2 ТБ + 248 x 1 ГБ = 1,4 ТБ.

Это свидетельствует о том, что в некоторых обстоятельствах при определенном распределении файлов размер Управляемого экземпляра может неожиданно достичь 35 ТБ, выделенных для присоединенного диска Azure уровня "Премиум".

В этом примере существующие базы данных без проблем будут работать и разрастаться при условии, что в них не будут добавляться новые файлы. При этом создать новые базы данных или восстановить имеющиеся не удастся, так как для новых дисков места недостаточно, даже если общий размер всех баз данных не достигает предельного размера экземпляра. Сообщение об ошибке, поступающее в этом случае, плохо отражает ситуацию.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Неправильная конфигурация ключа SAS во время восстановления базы данных

`RESTORE DATABASE`, читающая BAK-файл, может постоянно выполнять повторные попытки читать BAK-файл и возвращать ошибку после длительного периода времени, если подписанный URL-адрес в `CREDENTIAL` неверен. Выполните инструкцию RESTORE HEADERONLY перед восстановлением базы данных, чтобы убедиться в правильности ключа SAS.
Удалите начальный символ `?` из ключа SAS, созданного на портале Azure.

### <a name="tooling"></a>Инструментарий

В SQL Server Management Studio (SSMS) и SQL Server Data Tools (SSDT) могут возникать некоторые проблемы во время доступа к Управляемому экземпляру.

- Использование пользователей и имен для входа Azure AD (**общедоступная предварительная версия**) в SSDT пока не поддерживается.
- Создание скриптов с пользователями и именами для входа Azure AD (**общедоступная предварительная версия**) не поддерживается в SSMS.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Неправильные имена базы данных в некоторых представлениях, журналах и сообщениях

Несколько системных представлений, счетчиков производительности, сообщений об ошибках, XEvents и записей в журнале ошибок отображают идентификаторы базы данных GUID вместо фактических имен базы данных. Не следует полагаться на эти идентификаторы GUID, так как в будущем они будут заменены на фактические имена базы данных.

### <a name="database-mail-profile"></a>Профиль компонента Database Mail

Допускается только один профиль компонента Database Mail, и ему должно быть присвоено имя `AzureManagedInstance_dbmail_profile`.

### <a name="error-logs-are-not-persisted"></a>Журналы ошибок, которые не сохраняются

Журналы ошибок, которые доступны в управляемом экземпляре, не сохраняются, и их размер не включается в максимальный размер хранилища. Журналы ошибок могут автоматически удаляться в случае отработки отказа.

### <a name="error-logs-are-verbose"></a>Журналы ошибок с подробной информацией

Управляемый экземпляр помещает подробную информацию в журналы ошибок, и многие из них не являются связанными. В будущем объем информации в журналах ошибок будет уменьшен.

**Возможное решение**: для чтения журналов ошибок настройте отфильтровывание некоторых несущественных записей. Дополнительные сведения см. в статье [Azure SQL DB Managed Instance – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/) (Управляемый экземпляр Базы данных Azure SQL — sp_readmierrorlog).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-is-not-supported"></a>Не поддерживается разделение области транзакции на две базы данных внутри одного экземпляра

Класс `TransactionScope` в .Net не работает, если два запроса отправляются двум базам данных из одного экземпляра в той же области транзакции:

```C#
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Несмотря на то что этот код работает с данными в одном экземпляре, для него требуется координатор распределенных транзакций (MSDTC).

**Возможное решение**: вместо двух подключений используйте [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase), чтобы подключиться к другой базе данных в том же контексте.

### <a name="clr-modules-and-linked-servers-sometime-cannot-reference-local-ip-address"></a>Модули среды CLR и связанные серверы иногда не могут ссылаться на локальный IP-адрес

Модули среды CLR, помещенные в Управляемый экземпляр, связанные серверы и распределенные запросы, ссылающиеся на текущий экземпляр, иногда не могут разрешить IP-адрес локального экземпляра. Это временная ошибка.

**Возможное решение**: если есть возможность, используйте в модуле среды CLR контекстные подключения.

## <a name="next-steps"></a>Дополнительная информация

- Сведения об Управляемом экземпляре см. в статье [обзора Управляемого экземпляра](sql-database-managed-instance.md).
- Сведения о функциях и список сравнения см. в статье [Сравнение функций Базы данных SQL Azure и SQL Server](sql-database-features.md).
- Дополнительные сведения см. в инструкции по [созданию Управляемого экземпляра](sql-database-managed-instance-get-started.md).
