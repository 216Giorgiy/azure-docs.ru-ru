---
title: Использование определяемых пользователем функций Java с Apache Hive в HDInsight в Azure
description: Узнайте, как создать определяемую пользователем функцию (UDF) на основе Java, которая работает с Apache Hive. В этом примере определяемая пользователем функция преобразует таблицу текстовых строк в нижний регистр.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: 94e9a70707472eb94109ebcc404fd7a1a3074135
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57575752"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>Использование определяемых пользователем функций Java с Apache Hive в HDInsight

Узнайте, как создать определяемую пользователем функцию (UDF) на основе Java, которая работает с Apache Hive. Определяемая пользователем функция Java в этом примере преобразует таблицу текстовых строк в символы нижнего регистра.

## <a name="requirements"></a>Требования

* Кластер HDInsight. 

    > [!IMPORTANT]
    > Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе [Приближается дата прекращения сопровождения HDI версии 3.3](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

    Большинство действий, описываемых в этой статье, можно выполнять в кластерах как на основе Windows, так и на основе Linux. Тем не менее инструкции по отправке скомпилированных определяемых пользователем функций в кластер и их выполнению относятся только к кластерам под управлением Linux. Здесь также приведены ссылки на дополнительные сведения, которые можно использовать при работе с кластерами под управлением Windows.

* Пакет [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/) 8 или более поздней версии (или эквивалентный пакет, например OpenJDK).

* [Apache Maven](https://maven.apache.org/)

* Текстовый редактор или Java IDE.

    > [!IMPORTANT]
    > Если вы создаете файлы Python в клиенте Windows, следует использовать редактор, который использует LF в качестве символа конца строки. Если вы не уверены, используется ли в редакторе символ LF или CRLF, см. раздел "Устранение неполадок", в котором описано, как удалять символы CR.

## <a name="create-an-example-java-udf"></a>Создание примера определяемой пользователем функции Java 

1. Чтобы создать проект Maven, введите следующую команду в командной строке.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > При использовании PowerShell параметры необходимо заключить в кавычки. Например, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Эта команда создает каталог **exampleudf**, который содержит проект Maven.

2. После создания проекта удалите каталог **exampleudf/src/test**, который был создан как часть проекта.

3. Откройте файл **exampleudf/pom.xml** и замените текущую запись `<dependencies>` следующим кодом XML:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    В этих записях указана версия Hadoop и Hive, используемая в кластерах HDInsight 3.6. Сведения о версиях Hadoop и Hive, включенных в HDInsight, можно найти в статье [Что представляют собой различные компоненты Hadoop, доступные в HDInsight?](../hdinsight-component-versioning.md) .

    В конце файла добавьте раздел `<build>` перед строкой `</project>`. Этот раздел должен содержать следующий код XML:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.6  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
    ```

    Эти записи определяют способ построения проекта. В частности, используемую в проекте версию Java и способ построения uberjar для развертывания в кластер.

    После внесения изменений сохраните файл.

4. Переименуйте файл **exampleudf/src/main/java/com/microsoft/examples/App.java** на **ExampleUDF.java**, а затем откройте его в редакторе.

5. Замените содержимое файла **ExampleUDF.java** на следующее и сохраните файл.

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    Этот код реализует определяемую пользователем функцию, которая принимает строковое значение и возвращает строку в нижнем регистре.

## <a name="build-and-install-the-udf"></a>Создание и установка определяемой пользователем функции

1. Выполните следующую команду, чтобы скомпилировать и упаковать определяемую пользователем функцию.

    ```bash
    mvn compile package
    ```

    Эта команда выполняет сборку определяемой пользователем функции и упаковывает ее в файл `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar`.

2. Чтобы скопировать файл в кластер HDInsight, воспользуйтесь командой `scp` .

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight.net
    ```

    Замените `myuser` на имя учетной записи пользователя SSH для кластера. Замените `mycluster` именем кластера. Если для защиты учетной записи SSH используется пароль, отобразится запрос на его ввод. Если используется сертификат, может потребоваться использовать параметр `-i` , чтобы указать соответствующий файл закрытого ключа.

3. Подключитесь к кластеру с помощью SSH.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. В сеансе SSH скопируйте JAR-файл в хранилище HDInsight.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>Использование определяемой пользователем функции из Hive

1. В сеансе SSH используйте следующую команду, чтобы запустить клиент Beeline.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    В этой команде предполагается, что вы использовали для кластера учетную запись входа по умолчанию — **admin** .

2. После появления командной строки `jdbc:hive2://localhost:10001/>` введите следующую команду, чтобы добавить определяемую пользователем функцию в Hive и предоставить ее как функцию.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > В этом примере предполагается, что служба хранилища Azure является хранилищем по умолчанию для кластера. Если же кластер использует Azure Data Lake Storage 2-го поколения, измените значение `wasb:///` на `abfs:///`. Если же кластер использует Azure Data Lake Storage 1-го поколения, измените значение `wasb:///` на `adl:///`.

3. Используйте определяемую пользователем функцию, чтобы преобразовать значения, полученные из таблицы, в строки нижнего регистра.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    В результате выполнения этого запроса из таблице будет выбрана платформа устройства (Android, Windows, iOS и т. д.) и отображены строки, преобразованные в нижний регистр. Выходные данные выглядят следующим образом:

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Дальнейшие действия

Другие способы работы с Hive в HDInsight см. в [этой статье](hdinsight-use-hive.md).

Дополнительные сведения об определяемых пользователем функциях Hive см. в разделе [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) (Операторы Hive и определяемые пользователем функции) вики-сайта Hive на сайте apache.org.
