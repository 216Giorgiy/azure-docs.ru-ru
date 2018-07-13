---
title: Руководство. Выполнение операций извлечения, преобразования и загрузки (ETL) с использованием Hive в Azure HDInsight
description: Сведения об извлечении данных из необработанного набора данных в формате CSV, преобразовании их с помощью Hive в HDInsight и загрузке преобразованных данных в базу данных SQL Azure с помощью Sqoop.
services: hdinsight
documentationcenter: ''
author: jamesbak
manager: jahogg
tags: azure-portal
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.devlang: na
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: jamesbak
ms.custom: H1Hack27Feb2017,hdinsightactive,mvc
ms.openlocfilehash: 8f5771ac860d40eab979bf9be92b18da8f5d850d
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342374"
---
# <a name="tutorial-extract-transform-and-load-data-using-apache-hive-on-azure-hdinsight"></a>Руководство. Извлечение, преобразование и загрузка данных с помощью Apache Hive в Azure HDInsight

В этом руководстве вы извлечете CSV-файл с необработанными данными, импортируете их в кластер HDInsight, а затем преобразуете данные с помощью Apache Hive в Azure HDInsight. После преобразования вы загрузите эти данные в базу данных SQL Azure с использованием Apache Sqoop. В этой статье используются общедоступные данные о рейсах.

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * загрузка образца данных о рейсах;
> * отправка данных в кластер HDInsight;
> * преобразование данных с помощью Hive;
> * создание таблицы в базе данных SQL Azure;
> * экспорт данных в базу данных SQL Azure с помощью Sqoop.


На следующем рисунке показан стандартный процесс выполнения извлечения, преобразования и загрузки в приложении.

![Выполнение операций ETL с помощью Apache Hive в Azure HDInsight](./media/tutorial-extract-transform-load-hive/hdinsight-etl-architecture.png "Выполнение операций ETL с помощью Apache Hive в Azure HDInsight")

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* **Кластер Hadoop в HDInsight на платформе Linux**. Пошаговые инструкции по созданию нового кластера HDInsight под управлением Linux см. в руководстве по [настройке кластеров в HDInsight с помощью Hadoop, Spark, Kafka и других средств](./quickstart-create-connect-hdi-cluster.md).

* **База данных SQL Azure**. Вы используете базу данных SQL Azure в качестве конечного хранилища данных. Если у вас нет базы данных SQL, см. сведения в статье [Создание базы данных SQL Azure на портале Azure](../../sql-database/sql-database-get-started.md).

* **Azure CLI 2.0**. Если вы еще не установили Azure CLI, обратитесь к статье [Установка Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* **Клиент SSH**. Дополнительные сведения см. в руководстве по [подключению к HDInsight (Hadoop) с помощью SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Для выполнения действий, описанных в этом документе, необходим кластер HDInsight под управлением Linux. Linux — это единственная операционная система, используемая для работы с Azure HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе [Приближается дата прекращения сопровождения HDI версии 3.3](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="download-the-flight-data"></a>Скачивание данных о рейсах

1. Перейдите на страницу [бюро транспортной статистики при администрации по исследованиям и инновационным технологиям][rita-website].

2. На странице выберите следующие значения:

   | Действие | Значение |
   | --- | --- |
   | Фильтр года |2013 |
   | Период фильтра |Январь |
   | Поля |Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   Очистите все остальные поля.

3. Выберите **Скачать**. Вы получите ZIP-файл с выбранными полями данных.

## <a name="upload-data-to-an-hdinsight-cluster"></a>Отправка данных в кластер HDInsight

Существует множество способов отправки данных в хранилище, связанное с кластером HDInsight. В этом разделе для отправки данных используется `scp`. Дополнительные сведения о других способах отправки данных см. в статье [Use Distcp to copy data between Azure Storage Blobs and Data Lake Storage Gen2 Preview](use-distcp.md) (Копирование данных между большими двоичными объектами службы хранилища Azure и хранилищем Data Lake поколения 2 (предварительная версия) с помощью Distcp).

1. Откройте командную строку и воспользуйтесь следующей командой, чтобы передать ZIP-файл в головной узел кластера HDInsight:

    ```bash
    scp <FILE_NAME>.zip <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net:<FILE_NAME.zip>
    ```

    Замените *FILE_NAME* именем ZIP-файла. Замените *SSH_USER_NAME* именем для входа SSH для кластера HDInsight. Замените *CLUSTER_NAME* именем кластера HDInsight.

   > [!NOTE]
   > Если для аутентификации входа посредством SSH используется пароль, будет предложено ввести пароль. Если используется открытый ключ, может потребоваться использовать параметр `-i` и указать путь к соответствующему закрытому ключу. Например, `scp -i ~/.ssh/id_rsa FILE_NAME.zip USER_NAME@CLUSTER_NAME-ssh.azurehdinsight.net:`.

2. После завершения отправки можно подключиться к кластеру с помощью SSH. Введите приведенную ниже команду в окне командной строки.

    ```bash
    ssh <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net
    ```

3. Чтобы распаковать ZIP-файл, используйте следующую команду:

    ```bash
    unzip <FILE_NAME>.zip
    ```

    Она извлекает *CSV*-файл, размер которого составляет приблизительно 60 МБ.

4. С помощью указанных ниже команд создайте каталог, а затем скопируйте *CSV*-файл в этот каталог.

    ```bash
    hdfs dfs -mkdir -p abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data
    hdfs dfs -put <FILE_NAME>.csv abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data/
    ```

5. Создайте файловую систему хранилища Data Lake поколения 2.

    ```bash
    hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/
    ```

## <a name="transform-data-using-a-hive-query"></a>Преобразование данных с помощью запроса Hive

Существует множество способов запуска задания Hive в кластере HDInsight. В этом разделе используется клиент Beeline. Сведения о других методах выполнения задания Hive см. в статье [Обзор Apache Hive и HiveQL в Azure HDInsight](../../hdinsight/hadoop/hdinsight-use-hive.md).

В рамках задания Hive вы импортируете данные из CSV-файла в таблицу Hive с именем **Delays**.

1. В командной строке SSH, которую вы уже использовали для кластера HDInsight, выполните следующую команду для создания и редактирования нового файла **flightdelays.hql**.

    ```bash
    nano flightdelays.hql
    ```

2. В качестве содержимого файла добавьте следующий текст:

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays
    LOCATION abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/processed
    AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. Чтобы сохранить файл, нажмите клавишу **ESC**, а затем введите `:x`.

3. Для запуска Hive и выполнения файла **flightdelays.hql** используйте следующую команду:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

4. По завершении выполнения сценария __flightdelays.hql__ используйте следующую команду, чтобы открыть интерактивный сеанс Beeline:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. При появлении командной строки `jdbc:hive2://localhost:10001/>` используйте приведенный ниже запрос, чтобы извлечь информацию из импортированных данных о задержке рейсов:

    ```hiveql
    INSERT OVERWRITE DIRECTORY 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Вы получите список городов, рейсы в которых задержаны из-за погодных условий, а также среднее время задержки. Он будет сохранен в `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output`. Позже Sqoop считает данные из этого расположения и экспортирует их в базу данных SQL Azure.

6. Чтобы выйти из Beeline, введите `!quit` в командной строке.

## <a name="create-a-sql-database-table"></a>Создание таблицы базы данных SQL

В этом разделе предполагается, что вы уже создали базу данных SQL Azure. Если у вас еще нет базы данных SQL, создайте ее, ознакомившись со статьей [Создание базы данных SQL Azure на портале Azure](../../sql-database/sql-database-get-started.md).

Если у вас уже имеется база данных SQL, необходимо получить имя сервера. Чтобы найти имя сервера на [портале Azure](https://portal.azure.com), выберите **Базы данных SQL**, а затем отфильтруйте по имени базы данных, которую необходимо использовать. Имя сервера указано в столбце **Имя сервера**.

![Получение сведений о сервере SQL Azure](./media/tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Получение сведений о сервере SQL Azure")

> [!NOTE]
> Существует множество способов подключения к базе данных SQL и создания таблицы. В приведенных ниже действиях используется [FreeTDS](http://www.freetds.org/) из кластера HDInsight.


1. Чтобы установить FreeTDS, выполните следующую команду с помощью SSH-подключения к кластеру:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. После завершения установки используйте следующую команду для подключения к серверу базы данных SQL. Замените **serverName** именем сервера базы данных SQL. Замените **adminLogin** и **adminPassword** именем для входа и паролем для базы данных SQL. Замените **databaseName** именем базы данных.

    ```bash
    TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -p 1433 -D <DATABASE_NAME>
    ```

    При появлении запроса введите пароль администратора базы данных SQL.

    Должен появиться результат, аналогичный приведенному ниже тексту.

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. В командной строке `1>` введите следующее:

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Если вводится инструкция `GO`, то оцениваются предыдущие инструкции. Этот запрос создает таблицу **delays** с кластеризованным индексом.

    Используйте следующий запрос команду для проверки создания таблицы.

    ```hiveql
    SELECT * FROM information_schema.tables
    GO
    ```

    Результат будет аналогичен приведенному ниже:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo             delays        BASE TABLE
    ```

5. В командной строке `exit` at the `1>` , чтобы выйти из служебной программы tsql.


## <a name="export-data-to-sql-database-using-sqoop"></a>Экспорт данных в базу данных SQL с помощью Sqoop

В предыдущих разделах вы скопировали преобразованные данные в папку `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output`. В этом разделе вы с помощью Sqoop экспортируете данные из папки `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` в созданную в базе данных SQL Azure таблицу. 

1. Чтобы проверить, видно ли в Sqoop базу данных SQL, используйте следующую команду:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
    ```

    Эта команда выводит список баз данных, включая базу данных, в которой вы ранее создали таблицу delays.

2. Для экспорта данных из таблицы hivesampletable в таблицу mobiledata используйте следующую команду:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<FILE_SYSTEM_NAME>@.dfs.core.windows.net/tutorials/flightdelays/output' 
    --fields-terminated-by '\t' -m 1
    ```

    Sqoop подключается к базе данных, которая содержит таблицу delays, и экспортирует данные из каталога `/tutorials/flightdelays/output` в эту таблицу.

3. Когда команда sqoop будет выполнена, используйте служебную программу tsql для подключения к базе данных:

    ```bash
    TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
    ```

    Используйте следующие инструкции, чтобы проверить состояние экспорта данных в таблицу delays:

    ```sql
    SELECT * FROM delays
    GO
    ```

    Вы увидите список данных в таблице. Таблица содержит название города и среднее время задержки рейса для этого города. 

    Введите `exit` для выхода из служебной программы tsql.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать дополнительные возможности работы с данными в HDInsight, ознакомьтесь со следующими статьями:

* [Использование Hive с HDInsight][hdinsight-use-hive]
* [Использование Pig с HDInsight][hdinsight-use-pig]
* [Разработка программ MapReduce на Java для Hadoop в HDInsight][hdinsight-develop-mapreduce]
* [Разработка программ MapReduce с потоковой передачей Python для HDInsight][hdinsight-develop-streaming]
* [Использование Oozie с HDInsight][hdinsight-use-oozie]
* [Использование Sqoop с Hadoop в HDInsight][hdinsight-use-sqoop]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: ../../hdinsight/hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]:../../hdinsight/hadoop/hdinsight-use-hive.md
[hdinsight-provision]: ../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: ../../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: ../../hdinsight/hdinsight-upload-data.md
[hdinsight-get-started]: ../../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:../../hdinsight/hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-use-pig]:../../hdinsight/hadoop/hdinsight-use-pig.md
[hdinsight-develop-streaming]:../../hdinsight/hadoop/apache-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]:../../hdinsight/hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
