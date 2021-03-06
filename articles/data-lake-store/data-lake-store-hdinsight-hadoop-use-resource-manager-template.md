---
title: Создание HDInsight с Azure Data Lake Storage 1-го поколения с помощью шаблонов Azure | Документы Майкрософт
description: Создание кластеров HDInsight для работы с Azure Data Lake Storage 1-го поколения с помощью шаблонов Azure Resource Manager
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: b09ca2cc358107c5f95fe3426351d380380db3c2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "58880140"
---
# <a name="create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-using-azure-resource-manager-template"></a>Создание кластера HDInsight с Azure Data Lake Storage 1-го поколения с помощью шаблона Azure Resource Manager
> [!div class="op_single_selector"]
> * [Использование портала](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Использование PowerShell (для хранилища по умолчанию)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Использование PowerShell (для дополнительного хранилища)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Использование Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Узнайте, как с помощью Azure PowerShell настроить кластер HDInsight с Azure Data Lake Storage 1-го поколения в качестве **дополнительного хранилища**.

В поддерживаемых типах кластеров Data Lake Storage 1-го поколения можно использовать в качестве хранилища по умолчанию или дополнительной учетной записи хранения. Если Data Lake Storage 1-го поколения используется как дополнительное хранилище, в этом случае в качестве учетной записи хранения по умолчанию для кластеров по-прежнему используется Azure Storage Blob (WASB). Кроме того, относящиеся к кластеру файлы (журналы и т. д.) записываются в хранилище по умолчанию, а данные, которые необходимо обработать, могут храниться в учетной записи Data Lake Storage 1-го поколения. Использование Data Lake Storage 1-го поколения в качестве дополнительной учетной записи хранения не влияет на производительность или возможность выполнять чтение и запись в хранилище из кластера.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>Использование Data Lake Storage 1-го поколения в качестве хранилища кластера HDInsight

Ниже приведены некоторые важные сведения об использовании HDInsight с Data Lake Storage 1-го поколения.

* Возможность создавать кластеры HDInsight, использующие Data Lake Storage 1-го поколения в качестве хранилища по умолчанию, поддерживается для HDInsight версий 3.5 и 3.6.

* Возможность создавать кластеры HDInsight с доступом к Data Lake Storage 1-го поколения в качестве дополнительного хранилища поддерживается для версий HDInsight 3.2, 3.4, 3.5 и 3.6.

В этой статье мы подготовим кластер Hadoop, в котором Data Lake Storage 1-го поколения будет дополнительным хранилищем. Инструкции по созданию кластера Hadoop с Data Lake Storage 1-го поколения в качестве хранилища по умолчанию см. в статье [Создание кластера HDInsight с Data Lake Storage 1-го поколения с помощью портала Azure](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Перед началом работы с этим учебником необходимо иметь следующее:

* **Подписка Azure**. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 или более поздней версии**. Ознакомьтесь со статьей [Установка и настройка Azure PowerShell](/powershell/azure/overview).
* **Субъект-служба Azure Active Directory**. В этом учебнике приведены инструкции по созданию субъекта-службы в Azure AD. Однако, чтобы создать субъект-службу, необходимо быть администратором Azure AD. Если вы являетесь администратором Azure AD, то можете пропустить это предварительное требование и продолжить работу с учебником.

    **Если вы не являетесь администратором Azure AD**, то вы не сможете выполнить шаги, необходимые для создания субъекта-службы. В этом случае администратор Azure AD должен сначала создать субъект-службу, после чего вы сможете создать кластер HDInsight с Data Lake Storage 1-го поколения. При создании субъекта-службы также необходимо использовать сертификат, как описано в разделе [Create a service principal with certificate](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority) (Создание субъекта-службы с сертификатом).

## <a name="create-an-hdinsight-cluster-with-data-lake-storage-gen1"></a>Создание кластера HDInsight с Data Lake Storage 1-го поколения
Шаблон Resource Manager и предварительные требования для использования шаблона см. в статье [Deploy a HDInsight Linux cluster with new Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage) (Развертывание кластера HDInsight Linux с помощью нового Data Lake Storage 1-го поколения) на GitHub. Следуйте инструкциям по этой ссылке, чтобы создать кластер HDInsight с Data Lake Storage 1-го поколения в качестве дополнительного хранилища.

Инструкции по ссылке выше требуют PowerShell. Прежде чем начать работу с ними, обязательно войдите в учетную запись Azure. На своем компьютере откройте новое окно Azure PowerShell и введите следующий фрагмент кода. Когда вам будет предложено войти, введите учетные данные администратора или владельца подписки.

```
# Log in to your Azure account
Connect-AzAccount

# List all the subscriptions associated to your account
Get-AzSubscription

# Select a subscription
Set-AzContext -SubscriptionId <subscription ID>
```

Шаблон развертывает ресурсы следующих типов:

* [Microsoft.DataLakeStore/accounts](/azure/templates/microsoft.datalakestore/accounts)
* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.HDInsight/clusters](/azure/templates/microsoft.hdinsight/clusters)

## <a name="upload-sample-data-to-data-lake-storage-gen1"></a>Отправка примера данных в Data Lake Storage 1-го поколения
Шаблон Resource Manager создает учетную запись Data Lake Storage 1-го поколения и связывает ее с кластером HDInsight. На этом этапе необходимо отправить пример данных в Data Lake Storage 1-го поколения. Эти данные потребуются позже при выполнении заданий из кластера HDInsight для получения доступа к учетной записи Data Lake Storage 1-го поколения. Инструкции по отправке данных см. в разделе [Отправка файла в учетную запись Data Lake Storage 1-го поколения](data-lake-store-get-started-portal.md#uploaddata). Если у вас нет под рукой подходящих для этих целей данных, передайте папку **Ambulance Data** из [репозитория Git для озера данных Azure](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).

## <a name="set-relevant-acls-on-the-sample-data"></a>Установка соответствующих списков управления доступом для примера данных
Чтобы обеспечить доступ к отправляемым примерам данных для кластера HDInsight, необходимо убедиться, что у приложения Azure AD, используемого для определения удостоверения между кластером HDInsight и Data Lake Storage 1-го поколения, есть доступ к файлу или папке, к которой вы пытаетесь получить доступ. Для этого сделайте следующее.

1. Найдите имя приложения Azure AD, связанного с кластером HDInsight и учетной записью Data Lake Storage 1-го поколения. Чтобы найти его, можно открыть колонку кластера HDInsight, созданного с помощью шаблона Resource Manager, выбрать вкладку **Удостоверение кластера AAD** и просмотреть значение параметра **Service Principal Display Name** (Отображаемое имя субъекта-службы).
2. Теперь предоставьте этому приложению Azure AD доступ к файлу или папке, к которым необходимо получить доступ из кластера HDInsight. Чтобы установить подходящие списки управления доступом для файла или папки в Data Lake Storage 1-го поколения, см. статью [Защита данных, хранимых в Data Lake Storage 1-го поколения](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-storage-gen1"></a>Выполнение тестовых заданий в кластере HDInsight для использования Data Lake Storage 1-го поколения
После настройки кластера HDInsight выполните в нем тестовые задания, чтобы проверить, доступно ли ему Data Lake Storage 1-го поколения. Для этого запустите образец задания Hive, создающего таблицу с данными, которые вы ранее отправили в учетную запись Data Lake Storage 1-го поколения.

В этом разделе вы подключитесь к кластеру HDInsight на платформе Linux по протоколу SSH и выполните пример запроса Hive. При использовании клиента Windows мы рекомендуем использовать служебную программу **PuTTY**, которую можно скачать на странице [https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Дополнительные сведения об использовании PuTTY см. в разделе [Использование SSH с Hadoop на основе Linux в HDInsight из Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. После подключения запустите интерфейс командной строки Hive с помощью следующей команды:

   ```
   hive
   ```
2. Используя интерфейс командной строки, введите следующие инструкции, чтобы создать таблицу с именем **vehicles** с помощью примера данных в Data Lake Storage 1-го поколения:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Должен отобразиться результат, аналогичный приведенному ниже:

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>Доступ к Data Lake Storage 1-го поколения с помощью команд HDFS
Настроив в кластере HDInsight параметры для работы с Data Lake Storage 1-го поколения, используйте для доступа к хранилищу команды оболочки HDFS.

В этом разделе вы подключитесь к кластеру HDInsight на платформе Linux по протоколу SSH и выполните команды HDFS. При использовании клиента Windows мы рекомендуем использовать служебную программу **PuTTY**, которую можно скачать на странице [https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Дополнительные сведения об использовании PuTTY см. в разделе [Использование SSH с Hadoop на основе Linux в HDInsight из Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

После подключения используйте следующую команду файловой системы HDFS для получения списка файлов в учетной записи Data Lake Storage 1-го поколения.

```
hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/
```

Эта команда должна показать файл, который вы ранее отправили в Data Lake Storage 1-го поколения.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder
```

С помощью команды `hdfs dfs -put` вы можете передать несколько файлов в Data Lake Storage 1-го поколения, а затем с помощью команды `hdfs dfs -ls` проверить, успешно ли они передались.


## <a name="next-steps"></a>Дальнейшие действия
* [Копирование данных из больших двоичных объектов хранилища Azure в хранилище озера данных](data-lake-store-copy-data-wasb-distcp.md)
* [Использование Data Lake Storage 1-го поколения с кластерами Azure HDInsight](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
