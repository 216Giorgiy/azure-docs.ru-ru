---
title: "Добавление решений по управлению кластерами HDInsight в Log Analytics | Документация Майкрософт"
description: "Узнайте, как использовать Azure Log Analytics, чтобы создавать пользовательские представления для кластеров HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: f8a073fbf40feaef97053bb1165d63bbca3e3feb
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/24/2018
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Добавление решений по управлению кластерами HDInsight в Log Analytics

HDInsight предоставляет решения по управлению для кластеров, которые можно добавлять в Azure Log Analytics. [Решения по управлению](../log-analytics/log-analytics-add-solutions.md) добавляют функциональные возможности в [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), тем самым предоставляя дополнительные средства анализа и работы с данными в Log Analytics. Эти решения собирают важные метрики производительности из кластеров HDInsight и предоставляют средства для поиска метрик. Кроме того, они позволяют использовать визуализации и панели мониторинга для большинства типов кластеров, которые поддерживаются в HDInsight. На основе этих метрик можно создавать пользовательские правила мониторинга и оповещения. 

Из этой статьи вы узнаете, как добавлять решения по управлению для кластеров в рабочую область Operations Management Suite.

## <a name="prerequisites"></a>предварительным требованиям

* Для использования Log Analytics необходимо настроить кластер HDInsight. Инструкции см. в статье [Use Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Использование Azure Log Analytics для мониторинга кластеров HDInsight (предварительная версия)).

## <a name="add-cluster-specific-management-solutions"></a>Добавление решений по управлению кластерами

В этом разделе мы добавим решение по управлению кластерами HBase в существующую рабочую область Operations Management Suite.

1. Откройте кластер HDInsigt на портале Azure, выберите **Мониторинг**, затем нажмите кнопку **Открыть панель мониторинга OMS**.

    ![Открытие панели мониторинга Operations Management Suite](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS dashboard")

1. На панели мониторинга OMS щелкните **Коллекции решений** или значок **конструктора представлений** в области слева.

    ![Добавление решения по управлению в Operations Management Suite](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "Add management solution in Operations Management Suite")

2. В коллекции решений выберите одну из плиток:

    - Мониторинг HDInsight Hadoop
    - Мониторинг HDInsight HBase (предварительная версия)
    - Мониторинг Kafka HDInsight
    - Мониторинг HDInsight Storm
    - Мониторинг HDInsight Spark

3. На следующем экране нажмите кнопку **Добавить**.  На следующем рисунке показана кнопка "Добавить" для мониторинга HBase.

     ![Добавление решения по управлению HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "Добавление решения по управлению HBase")

4. На панели мониторинга OMS отобразится плитка решения по управлению HBase. Если кластер, связанный с Operations Management Suite (что требуется сделать в целях этой статьи), является кластером HBase, на плитке отобразятся имя кластера и число связанных узлов.

    ![Добавленное решение по управлению HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "Добавленное решение по управлению HBase")

## <a name="next-steps"></a>Дополнительная информация

* [Запрос в Azure Log Analytics для мониторинга кластеров HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>См. также

* [Работа с Operations Management Suite Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Работа с правилами генерации оповещений в Log Analytics](../log-analytics/log-analytics-alerts-creating.md)
