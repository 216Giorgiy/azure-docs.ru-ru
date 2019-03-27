---
title: Настройка кластеров HDInsight с помощью начальной загрузки в Azure
description: Узнайте, как настраивать кластеры HDInsight с помощью начальной загрузки.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 66c3345c1387e5f1535bbe2bc9a8eafdbb7423d2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447805"
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Настройка кластеров HDInsight с помощью начальной загрузки

В некоторых случаях требуется изменить файлы конфигурации, к которым относятся:

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml
* server.properties (конфигурация kafka-broker).

Существуют три способа использования начальной загрузки.

* Использование Azure PowerShell
* Использование пакета SDK для .NET
* Использование шаблона Azure Resource Manager

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Сведения об установке дополнительных компонентов в кластере HDInsight во время создания см. в следующих разделах:

* [Настройка кластеров HDInsight с помощью действия скрипта (Linux)](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="use-azure-powershell"></a>Использование Azure PowerShell
Следующий код PowerShell настраивает конфигурацию [Apache Hive](https://hive.apache.org/).

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValues `
        -HiveSite $hiveConfigValues 

New-AzHDInsightCluster `
    -ResourceGroupName $existingResourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -Config $config 
```

Полный рабочий сценарий PowerShell можно найти в разделе [Приложение. Пример PowerShell](#appendix-powershell-sample).

**Чтобы проверить изменения, выполните следующие действия:**

1. Выполните вход на [портал Azure](https://portal.azure.com).
2. В меню слева щелкните **Кластеры HDInsight**. Если вы не видите этот пункт, сначала щелкните **Все службы**.
3. Щелкните кластер, созданный с помощью сценария PowerShell.
4. Щелкните **Панель мониторинга** в верхней части колонки, чтобы открыть пользовательский интерфейс Ambari.
5. Щелкните **Hive** в меню слева.
6. Щелкните **HiveServer2** в разделе **Сводка**.
7. Выберите вкладку **Configs** (Конфигурации).
8. Щелкните **Hive** в меню слева.
9. Выберите вкладку **Advanced** (Дополнительно).
10. Прокрутите вниз и разверните раздел **Advanced hive-site**(Расширенный сайт Hive).
11. Найдите в этом разделе **hive.metastore.client.socket.timeout** .

Вот еще несколько примеров изменения других файлов конфигурации:

```xml
# hdfs-site.xml configuration
$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

# core-site.xml configuration
$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

# mapred-site.xml configuration
$MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

# oozie-site.xml configuration
$OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
```
Дополнительные сведения см. в статье Азима Уддина (Azim Uddin) [Customizing HDInsight Cluster creation](https://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx) (Дополнительная настройка при подготовке кластера HDInsight).

## <a name="use-net-sdk"></a>Использование пакета SDK для .NET
Ознакомьтесь с разделом [Создание кластеров под управлением Linux в HDInsight с помощью пакета SDK для .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Использование шаблона Resource Manager
В шаблоне Resource Manager можно использовать начальную загрузку:

```json
"configurations": {
    �
    "hive-site": {
        "hive.metastore.client.connect.retry.delay": "5",
        "hive.execution.engine": "mr",
        "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
    }
}
```

![Настройка шаблона Azure Resource Manager для начальной загрузки кластера с помощью HDInsight Hadoop](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a>См. также
* статью [Установка кластеров в HDInsight с использованием Hadoop, Spark, Kafka и других технологий][hdinsight-provision-cluster], в которой даются указания по созданию кластера HDInsight с использованием других настраиваемых параметров.
* [Разработка скриптов действия сценария для HDInsight][hdinsight-write-script]
* [Краткое руководство. Создание кластера Apache Spark в HDInsight с помощью шаблона][hdinsight-install-spark].
* [Установка и использование Apache Giraph в кластерах HDInsight под управлением Windows](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions-linux.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Этапы создания кластера"

## <a name="appendix-powershell-sample"></a>Приложение Пример для PowerShell
Этот сценарий PowerShell создает кластер HDInsight и настраивает параметр Hive:

```powershell
####################################
# Set these variables
####################################
#region - used for creating Azure service names
$nameToken = "<ENTER AN ALIAS>" 
#endregion

#region - cluster user accounts
$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

$sshUserName = "sshuser" #HDInsight ssh user name
$sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
#endregion

####################################
# Service names and varialbes
####################################
#region - service names
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

$resourceGroupName = $namePrefix + "rg"
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName

$location = "East US 2"
#endregion

# Treat all errors as terminating
$ErrorActionPreference = "Stop"

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzContext}
catch{Connect-AzAccount}
#endregion

#region - Create an HDInsight cluster
####################################
# Create dependent components
####################################
Write-Host "Creating a resource group ..." -ForegroundColor Green
New-AzResourceGroup `
    -Name  $resourceGroupName `
    -Location $location

Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -Type Standard_GRS

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey
New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageContext #use the cluster name as the container name

####################################
# Create a configuration object
####################################
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValues `
        -HiveSite $hiveConfigValues 

####################################
# Create an HDInsight cluster
####################################
$httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

$sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
$sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -Location $location `
    -ClusterSizeInNodes 1 `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -Config $config

####################################
# Verify the cluster
####################################
Get-AzHDInsightCluster -ClusterName $hdinsightClusterName

#endregion
```
