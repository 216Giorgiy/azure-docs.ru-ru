---
title: Краткое руководство. Горизонтальное масштабирование вычислительных ресурсов в хранилище данных SQL Azure с помощью T-SQL | Документация Майкрософт
description: Масштабируйте вычислительные ресурсы в хранилище данных SQL Azure с помощью T-SQL и SQL Server Management Studio. Масштабируйте вычислительные ресурсы, чтобы повысить производительность, или выполняйте обратное масштабирование, чтобы сократить расходы.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b4e123475679cf1afce09630c157377ee67b5202
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Краткое руководство. Масштабирование вычислительных ресурсов в хранилище данных SQL Azure с помощью T-SQL

Масштабируйте вычислительные ресурсы в хранилище данных SQL Azure с помощью T-SQL и SQL Server Management Studio. [Горизонтально масштабируйте вычислительные ресурсы](sql-data-warehouse-manage-compute-overview.md), чтобы повысить производительность, или уменьшайте их масштаб, чтобы сократить затраты. 

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

## <a name="before-you-begin"></a>Перед началом работы

Скачайте и установите последнюю версию [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).

В этой статье предполагается, что вы ознакомились с [кратким руководством по созданию и подключению хранилища с помощью портала](create-data-warehouse-portal.md). Завершив работу с кратким руководством, вы узнаете, как подключиться к созданному хранилищу данных **mySampleDataWarehouse** и созданному правилу брандмауэра, которое разрешает нашему клиенту получить доступ к установленному серверу.
 
## <a name="create-a-data-warehouse"></a>Создание хранилища данных

Используйте инструкции из краткого руководства [Создание хранилища данных SQL Azure на портале Azure и отправка запросов к этому хранилищу данных](create-data-warehouse-portal.md), чтобы создать хранилище данных **mySampleDataWarehouse**. Завершите работу с кратким руководством, чтобы вы создали правило брандмауэра и смогли подключиться к хранилищу данных из SQL Server Management Studio.

## <a name="connect-to-the-server-as-server-admin"></a>Подключение к серверу от имени администратора сервера

В этом разделе для подключения к серверу SQL Azure используется [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).

1. Откройте среду SQL Server Management Studio.

2. В диалоговом окне **Подключение к серверу** введите следующие значения.

   | Параметр       | Рекомендуемое значение | ОПИСАНИЕ | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Тип сервера | Ядро СУБД | Это обязательное значение |
   | имя сервера; | Полное имя сервера | Ниже приведен пример: **mynewserver-20171113.database.windows.net**. |
   | Authentication | проверка подлинности SQL Server | В рамках работы с этим руководством мы настроили только один тип аутентификации — аутентификацию SQL. |
   | Вход | Учетная запись администратора сервера | Это учетная запись, указанная при создании сервера. |
   | Пароль | Пароль учетной записи администратора сервера | Это пароль, указанный при создании сервера. |

    ![Подключение к серверу](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Щелкните **Подключить**. Откроется окно обозревателя объектов в SSMS. 

5. В обозревателе объектов разверните папку **Базы данных**. Затем разверните папку **mySampleDatabase**, чтобы просмотреть объекты в новой базе данных.

    ![Объекты базы данных](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>Просмотр целевого уровня обслуживания
Параметр целевого уровня обслуживания содержит число единиц DWU для хранилища данных. 

Чтобы просмотреть текущее значение DWU для хранилища данных, сделайте следующее:

1. В разделе подключения к серверу **mynewserver-20171113.database.windows.net** разверните узел **Системные базы данных**.
2. Щелкните правой кнопкой мыши **master** и выберите **Создать запрос**. Откроется окно нового запроса.
3. Выполните следующий запрос для выбора из динамического административного представления sys.database_service_objectives. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. В следующих результатах показано, что для хранилища **mySampleDataWarehouse** задан целевой уровень обслуживания DW400. 

    ![Просмотр текущего значения DWU](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>Масштабирование вычислительных ресурсов
В хранилище данных SQL вы можете увеличивать и уменьшать объем вычислительных ресурсов, изменяя число единиц использования хранилища данных (DWU). В статье [Создание хранилища данных SQL Azure на портале Azure и отправка запросов к этому хранилищу данных](create-data-warehouse-portal.md) мы создали хранилище **mySampleDataWarehouse** и инициализировали его со значением 400 DWU. Ниже описаны шаги по изменению числа единиц DWU для **mySampleDataWarehouse**.

Изменить число единиц использования хранилища данных можно так:

1. Щелкните правой кнопкой мыши **master** и выберите **Создать запрос**.
2. Чтобы изменить целевой уровень обслуживания, используйте инструкцию T-SQL [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database). Выполните следующий запрос, чтобы изменить значение целевого уровня обслуживания на DW300. 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-data-warehouse-state"></a>Проверка состояния хранилища данных

Если хранилище данных остановлено, вы не сможете подключиться к нему с помощью T-SQL. Чтобы просмотреть текущее состояние хранилища данных, используйте командлет PowerShell. Пример см. в разделе о [проверке состояния хранилища данных с помощью Powershell](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>Проверка состояния операции

Чтобы получить сведения о разных операциях управления, выполняемых в хранилище данных SQL, отправьте следующий запрос в динамическом административном представлении [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). Это представление возвращает сведения о разных операциях и их состояние, которое будет иметь значение IN_PROGRESS или COMPLETED.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```


## <a name="next-steps"></a>Дополнительная информация
Вы узнали, как масштабировать вычислительные ресурсы для хранилища данных. Чтобы узнать больше о хранилище данных SQL Azure, перейдите к руководству по загрузке данных.

> [!div class="nextstepaction"]
>[Загрузка данных в хранилище данных SQL](load-data-from-azure-blob-storage-using-polybase.md)
