---
title: Настройка политик Apache Kafka в HDInsight с Корпоративным пакетом безопасности — Azure
description: Сведения о настройке политик Apache Ranger для Kafka в Azure HDInsight с корпоративным пакетом безопасности.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: 0d9ad11ab9a53cf5de51dd3f262dc16054be5d85
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438614"
---
# <a name="tutorial-configure-apache-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Руководство. Настройка политик Apache Kafka в HDInsight с Корпоративным пакетом безопасности (предварительная версия)

Сведения о настройке политик Apache Ranger для кластеров Apache Kafka с Корпоративным пакетом безопасности (ESP). Кластеры ESP подключены к домену, благодаря чему пользователи могут проходить аутентификацию с учетными данными домена. В этом руководстве вы создадите две политики Ranger для ограничения доступа к разделам `sales*` и `marketingspend`.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание пользователей домена
> * Создание политик Ranger
> * Создание разделов в кластере Kafka
> * Создание политик Ranger

## <a name="before-you-begin"></a>Перед началом работы

* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/).

* Войдите на [портале Azure](https://portal.azure.com/).

* Создайте [кластер Kafka в HDInsight с корпоративным пакетом безопасности](apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-apache-ranger-admin-ui"></a>Подключение к пользовательскому интерфейсу администратора Apache Ranger

1. В браузере подключитесь к интерфейсу администратора Ranger с помощью URL-адреса `https://<ClusterName>.azurehdinsight.net/Ranger/`. Не забудьте изменить `<ClusterName>` на имя вашего кластера Kafka.

    > [!NOTE]  
    > Учетные данные Ranger не совпадают с учетными данными кластера Hadoop. Чтобы браузеры не использовали кэшированные учетные данные Hadoop, подключитесь к пользовательскому интерфейсу администратора Ranger в новом окне браузера в режиме InPrivate.

2. Зарегистрируйтесь, используя свои учетные данные администратора Azure Active Directory (AD). Учетные данные администратора Azure AD отличаются от учетных данных кластера HDInsight или учетных данных SSH узла Linux HDInsight.

   ![Пользовательский интерфейс администратора Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Создание пользователей домена

В разделе [Создание кластера HDInsight с корпоративным пакетом безопасности](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-domain-joined-hdinsight-cluster), чтобы узнать, как создать пользователей домена **sales_user** и **marketing_user**. В рабочем сценарии пользователи домена берутся из вашего клиента Active Directory.

## <a name="create-ranger-policy"></a>Создание политики Ranger 

Создайте политику Ranger для пользователей **sales_user** и **marketing_user**.

1. Откройте **пользовательский интерфейс администратора Ranger**.

2. Нажмите кнопку **\<ClusterName>_kafka** в разделе **Kafka**. Может быть указана одна предварительно настроенная политика.

3. Щелкните **Добавить новую политику**, а затем введите следующие значения.

   |**Параметр**  |**Рекомендуемое значение**  |
   |---------|---------|
   |Имя политики  |  политика hdi sales*   |
   |Раздел   |  sales* |
   |Выберите пользователя  |  sales_user1 |
   |Разрешения  | публикация, использование, создание |

   Имя раздела может содержать следующие подстановочные знаки:

   * * обозначает ноль или более вхождений символов.
   * ? означает один символ

   ![Политика создания пользовательского интерфейса администратора Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)   

   >[!NOTE]   
   >Подождите несколько минут, пока Ranger синхронизируется с Azure AD, если в поле **Выберите пользователя** автоматически не подставится пользователь домена.

4. Щелкните **Добавить**, чтобы сохранить политику.

5. Щелкните **Добавить новую политику**, а затем введите следующие значения.

   |**Параметр**  |**Рекомендуемое значение**  |
   |---------|---------|
   |Имя политики  |  маркетинговая политика hdi   |
   |Раздел   |  marketingspend |
   |Выберите пользователя  |  marketing_user1 |
   |Разрешения  | публикация, использование, создание |

   ![Политика создания пользовательского интерфейса администратора Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. Щелкните **Добавить**, чтобы сохранить политику.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>Создание разделов в кластере Kafka с ESP

Чтобы создать два раздела, **salesevents** и **marketingspend**, выполните следующие действия:

1. Чтобы открыть SSH-подключение к кластеру, выполните следующую команду:

   ```bash
   ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Замените `SSHUSER` именем пользователя SSH для кластера, а `CLUSTERNAME` — именем кластера. При появлении запроса введите пароль для учетной записи пользователя SSH. Дополнительные сведения об использовании `scp` с HDInsight см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix).

   В рабочем сценарии пользователи домена, настроенные при создании кластера, могут войти в кластер по протоколу SSH.

2. Чтобы сохранить имя кластера в переменной и установить служебную программу синтаксического анализа JSON (`jq`), используйте следующие команды. При появлении запроса введите имя кластера Kafka.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Чтобы получить узлы Apache Zookeeper и брокера Kafka, используйте приведенные ниже команды. При появлении запроса введите пароль для учетной записи администратора кластера.

   ```bash
   export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```
> [!Note]  
> Прежде чем продолжить, вам может потребоваться настроить среду разработки, если вы еще не сделали это. Требуются такие компоненты, как Java JDK, Apache Maven и SSH-клиент с scp. См. [инструкции по установке](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer).
1. Скачайте [примеры присоединенного к домену производителя и потребителя Apache Kafka](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer).

1. Выполните шаги 2 и 3 в разделе **Создание и развертывание примера** в [руководстве по использованию API производителя и потребителя Apache Kafka](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-producer-consumer-api#build-and-deploy-the-example).

1. Выполните следующие команды:

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

   >[!NOTE]   
   >Только владелец процесса службы Kafka, такой как root, может осуществлять запись в z-узлы Zookeeper `/config/topics`. Политики Ranger не реализуются при создании темы непривилегированным пользователем. Это обусловлено тем, что скрипт `kafka-topics.sh` связывается напрямую с Zookeeper для создания темы. Записи добавляются в узлы Zookeeper, в то время как наблюдатели на стороне брокера отслеживают и создают соответствующие темы. Авторизацию невозможно выполнить с помощью подключаемого модуля Ranger, а команда выше выполняется с помощью `sudo` через брокера Kafka.


## <a name="test-the-ranger-policies"></a>Тестирование политик Ranger

На основе настроенных политик Ranger **sales_user** может создавать/использовать тему **salesevents** (но не тему **marketingspend**). И наоборот: **marketing_user** может создавать/использовать тему **marketingspend**, но не тему **salesevents**.

1. Откройте новое подключение SSH к кластеру. Выполните следующую команду, чтобы войти от имени пользователя **sales_user1**.

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Выполните следующую команду.

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Используйте брокер и имена Zookeeper из предыдущего раздела, чтобы настроить следующие переменные среды.

   ```bash
   export KAFKABROKERS=<brokerlist>:9092 
   ```

   Пример: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

   ```bash
   export KAFKAZKHOSTS=<zklist>:2181
   ```

   Пример: `export KAFKAZKHOSTS=zk1-khdicl.contoso.com:2181,zk2-khdicl.contoso.com:2181`

4. Убедитесь, что **sales_user1** может создать тему **salesevents**.
   
   Выполните следующую команду, чтобы запустить консоль, создающую тему **salesevents**.

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic salesevents --security-protocol SASL_PLAINTEXT
   ```

   Введите несколько сообщений на консоли. Нажмите клавишу **Ctrl + C**, чтобы выйти из программы, создающей консоль.

5. Выполните следующую команду, чтобы использовать ресурсы из темы **salesevents**.

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic salesevents --security-protocol PLAINTEXTSASL --from-beginning
   ```
 
6. Убедитесь, что введенные на предыдущем шаге сообщения отображаются, а пользователь **sales_user1** не может создавать тему **marketingspend**.

   В том же окне SSH, что и выше, выполните следующую команду, чтобы создать тему **marketingspend**.

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic marketingspend --security-protocol SASL_PLAINTEXT
   ```

   Происходит ошибка авторизации, ее можно игнорировать. 

7. Обратите внимание, что пользователь **marketing_user1** не может использовать ресурсы из темы **salesevents**.

   Повторите шаги 1–3 выше, но на этот раз от имени пользователя **marketing_user1**.

   Выполните следующую команду, чтобы использовать ресурсы из темы **salesevents**.

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic marketingspend --security-protocol PLAINTEXTSASL --from-beginning
   ```

   Предыдущие сообщения не будут отображаться.

8. Просмотрите события доступа к ресурсам аудита из интерфейса Ranger.

   ![Аудит политики пользовательского интерфейса Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Дополнительная информация

* [Создание собственных ключей для Apache Kafka](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Общие сведения об обеспечении безопасности Apache Hadoop с помощью Корпоративного пакета безопасности](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
