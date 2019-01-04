---
title: Инструменты Azure HDInsight. Настройка интерактивной среды PySpark для Visual Studio Code
description: Сведения о создании и отправке запросов и скриптов с помощью средств Azure HDInsight для Visual Studio Code.
keywords: VScode,средства Azure HDInsight,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: bf47915ba93a4a3a7dec338395cfe0ce6aa3cdf6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993847"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Настройка интерактивной среды PySpark для Visual Studio Code

Ниже показано, как установить пакеты Python, запустив **интерактивную среду PySpark HDInsight**.

## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>Настройка интерактивной среды PySpark в MacOS и Linux
Если вы используете **python 3.x**, выполните команду **pip3** для указанных ниже шагов.

1. Убедитесь, что **Python** и **pip** установлены.
 
    ![Версия pip для Python](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Установите пакет Jupyter.
    ```
    sudo pip install jupyter
    ```
   В Linux и MacOS может появляться следующее сообщение об ошибке:

   ![Ошибка 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Установите **libkrb5-dev** (только для Linux). Может появиться следующее сообщение об ошибке:

   ![Ошибка 2](./media/set-up-pyspark-interactive-environment/error2.png)
       
   ```Resolve:
   sudo apt-get install libkrb5-dev 
   ```

3. Установите **sparkmagic**.
   ```
   sudo pip install sparkmagic
   ```

4. Убедитесь, что мини-приложение **ipywidgets** установлено правильно. Для этого выполните следующую команду:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Установка ядер программ-оболочек](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Установите ядра программ-оболочек. Выполните команду **pip show sparkmagic**. В выходных данных отобразится путь к установленному пакету **sparkmagic**. 

    ![расположение sparkmagic](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Перейдите в нужное расположение и выполните следующую команду:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec install](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Проверьте состояние установки.

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec list](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Для доступных ядер: 
    - **python2** и **pysparkkernel** соответствуют **python 2.x**. 
    - **python3** и **pyspark3kernel** соответствуют **python 3.x**. 

8. Перезапустите VS Code, а затем снова перейдите к редактору сценариев, который работает на базе **интерактивной среды PySpark HDInsight**.

## <a name="next-steps"></a>Дополнительная информация

### <a name="demo"></a>Демонстрация
* HDInsight для VS Code: [Видео](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Средства и расширения
* [Использование средств Azure HDInsight для Visual Studio Code](hdinsight-for-vscode.md)
* [Создание приложений Apache Spark для кластера HDInsight с помощью Azure Toolkit for IntelliJ](spark/apache-spark-intellij-tool-plugin.md)
* [Удаленная или локальная отладка приложений Spark в кластере HDInsight с помощью Azure Toolkit for IntelliJ через SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Удаленная отладка приложений Spark в HDInsight через VPN с помощью Azure Toolkit for IntelliJ](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Использование средств HDInsight в Azure Toolkit for Eclipse для создания приложений Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Использование инструментов HDInsight для IntelliJ с песочницей Hortonworks](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Использование записных книжек Zeppelin с кластером Apache Spark в Azure HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Ядра для записной книжки Jupyter в кластерах Spark в Azure HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Использование внешних пакетов с записными книжками Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Визуализация данных Apache Hive с Microsoft Power BI с использованием ODBC в Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Выполнение запросов Apache Hive в Azure HDInsight с помощью Apache Zeppelin](hdinsight-connect-hive-zeppelin.md)
