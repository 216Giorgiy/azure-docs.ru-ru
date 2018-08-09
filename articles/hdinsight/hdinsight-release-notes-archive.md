---
title: Архивные заметки о выпуске Azure HDInsight
description: Архивные заметки о выпуске и версии Azure HDInsight.
services: hdinsight
editor: jasonwhowell
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: jasonh
ms.openlocfilehash: 34536316fef5f30df4ef1e3c89ddaaafed188c5d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600090"
---
# <a name="archived-release-notes-for-azure-hdinsight"></a>Архивные заметки о выпуске Azure HDInsight

Дополнительные сведения о **последних** обновлениях выпуска Azure HDInsight см. в [заметках о выпуске HDInsight](hdinsight-release-notes.md).

> [!IMPORTANT]
> Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в [статье об управлении версиями HDInsight](hdinsight-component-versioning.md).

## <a name="notes-for-06272018---release-of-new-open-source-versions-adls-gen2-etc-on-hdinsight-36"></a>Заметки о выпуске от 27 июня 2018 года. Выпуск новых версий с открытым кодом для ADLS поколения 2 и т. п. на HDInsight 3.6
Выпуск HDInsight от июня 2018 года — это значимый выпуск, который включает множество обновлений и возможностей для клиентов. Дополнительные сведения см. в [этой записи блога](https://azure.microsoft.com/en-us/blog/enterprises-get-deeper-insights-with-hadoop-and-spark-updates-on-azure-hdinsight/).

Ниже перечислены самые важные моменты. Подробные заметки о выпуске, исправленные ошибки, известные проблемы и многое другое см. в [заметках о выпуске Azure HDInsight](hdinsight-release-notes.md).

- **Обновление Hadoop и других проектов с открытым кодом**. Помимо исправления более чем 1000 ошибок в более чем 20 проектах с открытым кодом, это обновление также содержит новые версии Spark (2.3) и Kafka (1.0).
- **Обновление R Server 9.1 до Служб машинного обучения 9.3**. В этом выпуске мы предоставили специалистам по анализу данных и инженерам лучшие проекты с открытым исходным кодом, инновационными алгоритмами и возможностью быстрого ввода в эксплуатацию. Все они поддерживают использование разных языков программирования наряду с характеристиками Apache Spark. Этот выпуск расширяет возможности, предлагаемые R Server, реализуя поддержку Python. Из-за этого имя кластера изменено с R Server на Службы машинного обучения. 
- **Поддержка Azure Data Lake Storage поколения 2**. HDInsight теперь будет поддерживать предварительный выпуск Azure Data Lake Storage поколения 2. Во всех регионах доступности клиентам будет предоставлена возможность выбрать учетную запись ADSL поколения 2 для хранения кластеров HDInsight.
- **Обновления Корпоративного пакета безопасности HDInsight (предварительная версия).** Добавлена поддержка конечных точек службы виртуальной сети (предварительная версия), хранилища BLOB-объектов Azure, ADLS поколения 1, Cosmos DB и базы данных Azure. 

## <a name="notes-for-03202018---release-of-spark-22-on-hdinsight-36"></a>Заметки о выпуске Spark 2.2 в HDInsight 3.6 от 20.03.2018

- Spark 2.2.0 повышает стабильность Spark Core, SQL и ML. В этом выпуске функция структурированной потоковой передачи реализована а общедоступной версии. Теперь версия Spark 2.2.0 доступна в HDInsight 3.6.


## <a name="notes-for-08012017-release-of-hdinsight"></a>Заметки о выпуске HDinsight от 01.08.2017

| Название | ОПИСАНИЕ | Затронутая область  | Тип кластера  | 
| --- | --- | --- | --- | --- |
| Выпуск Microsoft R Server 9.1 в HDInsight |HDInsight теперь поддерживает подготовку кластеров R Server 9.1 в HDInsight. Дополнительные сведения о выпуске Microsoft R Server 9.1 см. в [этом блоге](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/). |Service |R Server |
| HDInsight 3.6 теперь включает в себя более новые версии стека Hadoop|<ul><li>Подробный список обновленных версий см. в разделе [Компоненты Hadoop, доступные в разных версиях HDInsight](hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions).</li><li>Список ошибок, исправленных в последних версиях стека Hadoop, см. на странице [Apache Patch Information](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/patch_parent.html) (Сведения об исправлениях Apache).</li><li>Список критических изменений, внесенных после выхода версии HDP 2.6.1 (которая теперь доступна в HDInsight 3.6), см. по адресу [https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html).</li><li>Список известных проблем в HDP 2.6.1 см. на странице [Known issues](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/known_issues.html) (Известные проблемы).</li></ul> |Service |Все |Недоступно |
| Обновления в кластерах Interactive Hive (предварительная версия) |<ul><li><b>Улучшение компонента.</b> Реализация кэшированного хранилища метаданных, которое уменьшает нагрузку на серверную часть SQL за счет кэширования метаданных, и повышает производительность всех операций с метаданными.  Этот компонент теперь по умолчанию присутствует на всех кластерах Interactive Hive. Дополнительные сведения можно найти по адресу [https://issues.apache.org/jira/browse/HIVE-16520](https://issues.apache.org/jira/browse/HIVE-16520).</li><li><b>Улучшение компонента.</b> Оптимизирована динамическая загрузка разделов. Дополнительные сведения можно найти по адресу [https://issues.apache.org/jira/browse/HIVE-14204] (https://issues.apache.org/jira/browse/HIVE-14204).</li><li><b>Улучшение компонента.</b> Оптимизация конфигурации для HDInsight на платформе Linux.</li><li><b>Исправление ошибки.</b> `CredentialProviderFactory$getProviders` не является потокобезопасным. Теперь эта проблема решена. Дополнительные сведения можно найти по адресу [https://issues.apache.org/jira/browse/HADOOP-14195](https://issues.apache.org/jira/browse/HADOOP-14195).</li><li><b>Исправление ошибки.</b> Высокий коэффициент загрузки ЦП при использовании API `liststatus` драйвера WASB, что приводило к снижению производительности ATS. Теперь эта проблема решена. Дополнительные сведения можно найти по адресу [https://github.com/Azure/azure-storage-java/pull/154](https://github.com/Azure/azure-storage-java/pull/154).</li></ul> |Service |Interactive Hive (предварительная версия) |
| Обновления в кластерах Hadoop |Повышена надежность операции задания Templeton. Дополнительные сведения можно найти по адресу [https://issues.apache.org/jira/browse/HIVE-15947](https://issues.apache.org/jira/browse/HIVE-15947). |Service |Hadoop |
| Обновления YARN | Теперь HDInsight создает базу данных Ambari размером 250 ГБ (без увеличения стоимости), что дает пользователям дополнительные преимущества. Это изменение призвано предотвратить заполнение ATS и повысить производительность. |Service |Все |
| Обновления Spark | Выпуск Spark 2.1.1. Дополнительные сведения см. в [заметках о выпуске Spark 2.1.1](https://spark.apache.org/releases/spark-release-2-1-1.html). | Service | Spark |

  



## <a name="04062017---general-availability-of-hdinsight-36"></a>Общедоступная версия HDInsight 3.6 от 06.04.2017

* В этом выпуске повышена версия Azure HDInsight до 3.6, которая основана на HDP 2.6. Заметки о выпуске HDP 2.6 доступны [здесь](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html), дополнительные сведения о версиях HDInsight доступны [здесь](hdinsight-component-versioning.md). Версия HDInsight 3.6 доступна для следующих рабочих нагрузок:

    * Hadoop 2.7.3
    * HBase 1.1.2
    * Storm 1.1.0
    * Spark 2.1.0
    * Interactive Hive 2.1.0

* **Поддержка Hive View 2.0**. Это должно улучшить взаимодействие с пользователем в Interactive Hive. Дополнительные сведения см. в [документации по Hortonworks](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html).

* **Повышение производительности с помощью Hive LLAP**. Дополнительные сведения см. в [документации по Hortonworks](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/).

* **Новые функции в Hive**. Ознакомьтесь с [документацией по Hortonworks](https://hortonworks.com/apache/hive/#section_4).

* **Интерфейс командной строки Hive объявлен нерекомендуемым**: использовать интерфейс командной строки Hive не рекомендуется, вместо него пользователям предлагается использовать Beeline. Дополнительные сведения см. в [документации Apache](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline). Инструкции по использованию Beeline с HDInsight см. в разделе [Использование Hive с Hadoop в HDInsight с применением Beeline](hadoop/apache-hadoop-use-hive-beeline.md).

* **Новые возможности в Apache Phoenix и HBase**.
    * Поддержка квоты хранилища: обычно используется в мультитенантных средах для ограничения емкости, выделяемой для хранения данных на уровне таблицы и пространства имен.
    * Улучшение индексирования Phoenix: создание добавочных индексов и перестроение индекса и возобновление индексирования после обнаружения ошибок.
    * Инструмент проверки целостности данных Phoenix: поддерживает проверку схемы, индекса и других метаданных.


* **Проблема с HBase**: при выполнении задания MapReduce для массовой передачи CSV-файлов приведенный ниже синтаксис может привести к ошибке.

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    Вместо этого используйте следующий синтаксис:

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv


## <a name="02282017---release-of-spark-21-on-hdinsight-36-preview"></a>Выпуск Spark 2.1 в HDInsight 3.6 (предварительная версия) от 28.02.2017
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) устраняет многие проблемы со стабильностью и удобством использования, имевшиеся в предыдущих версиях. Также добавлены новые функции для всех рабочих нагрузок Spark, включая Spark Core, SQL, машинное обучение и потоковую передачу.
* Улучшена масштабируемость структурированного потока и добавлена поддержка водяных знаков событий времени и соединителя Kafka 0.10.
* Секционирование Spark SQL теперь обрабатываются с помощью нового механизма масштабируемой обработки секций. Дополнительные сведения об установке новой версии см. [здесь](http://spark.apache.org/releases/spark-release-2-1-0.html).
* Spark 2.1 в Azure HDInsight 3.6 (предварительная версия) в настоящее время не поддерживает подключение инструментов бизнес-аналитики с помощью драйвера ODBC.
* В этой предварительной версии не поддерживается доступ к Azure Data Lake Store из кластеров Spark 2.1.


## <a name="11182016---release-of-spark-201-on-hdinsight-35"></a>Выпуск Spark 2.0.1 в HDInsight 3.5 от 18.11.2016
Выпуск Spark 2.0.1 теперь доступен в кластерах Spark (HDInsight версии 3.5).

## <a name="11162016---release-of-r-server-90-on-hdinsight-35-spark-20"></a>Выпуск R Server 9.0 в HDInsight 3.5 (Spark 2.0) от 16.11.2016
*   Кластеры R Server теперь включают параметры для двух версий: R Server 9.0 в HDI 3.5 (Spark 2.0) и R Server 8.0 в HDI 3.4 (Spark 1.6).
*   R Server 9.0 в HDI 3.5 (Spark 2.0) основан на R 3.3.2 и содержит новые функции источника данных ScaleR под названием RxHiveData и RxParquetData. Они позволяют передавать данные из Hive и Parquet напрямую в таблицы данных Spark для анализа с помощью ScaleR. Дополнительные сведения об этих функциях в R см. в интернет-справке. Ее можно вызвать, выполнив команды **?RxHiveData** и **?RxParquetData**.
*   Теперь выпуск RStudio Server Community Edition устанавливается по умолчанию (с параметром opt-out) в колонке "Настройка кластера" как часть потока подготовки.

## <a name="11092016---release-of-spark-20-on-hdinsight"></a>Выпуск Spark 2.0 в HDInsight от 09.11.2016
* Кластеры Spark 2.0 в HDInsight 3.5 теперь поддерживают службы Livy и Jupyter.

## <a name="10262016---release-of-r-server-on-hdinsight"></a>Выпуск R Server в HDInsight от 26.10.2016
* Универсальный код ресурса (URI), используемый для доступа к граничному узлу, теперь имеет формат **имя_кластера**-ed-ssh.azurehdinsight.net.
* Подготовка кластеров R Server в HDInsight упрощена.
* R Server в HDInsight теперь доступен как обычный тип кластера HDInsight "R Server" и больше не устанавливается как отдельное приложение HDInsight. Теперь граничный узел и двоичные файлы R Server подготавливаются в рамках развертывания кластера R Server. Это ускоряет подготовку и повышает ее надежность. Модель ценообразования для R Server обновлена соответствующим образом.
* Стоимость кластера R Server теперь объединяет цену уровня "Стандартный" и доплату за R Server. Это изменение не влияет на действующие цены для R Server. Оно влияет только на то, как платежи будут представлены в счете. Все имеющиеся кластеры R Server по-прежнему поддерживаются, а шаблоны Resource Manager функционируют до уведомления об устаревании. **Мы советуем обновить скрипты развертывания для использования нового шаблона Resource Manager.**






