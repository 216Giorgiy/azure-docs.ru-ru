---
title: Краткое руководство. Использование API Cassandra с Python в Azure Cosmos DB
description: В этом руководстве показано, как использовать API Apache Cassandra Azure Cosmos DB для создания приложения профиля с помощью Python.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: python
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 5d5715e21b0560f4433e557f52b7fcf204e2046b
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56588064"
---
# <a name="quickstart-build-a-cassandra-app-with-python-sdk-and-azure-cosmos-db"></a>Краткое руководство. Создание приложения Cassandra с помощью пакета SDK для Python и Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [Java](create-cassandra-java.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

В этом руководстве показано, как использовать Python и [API Cassandra](cassandra-introduction.md) Azure Cosmos DB для создания приложения профиля путем клонирования примера с сайта GitHub. Кроме того, здесь показано, как создать учетную запись Azure Cosmos DB на веб-портале Azure.

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. Вы можете быстро создавать и запрашивать документы, таблицы, пары "ключ — значение" и базы данных графов, используя возможности глобального распределения и горизонтального масштабирования Azure Cosmos DB.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Кроме того, можно воспользоваться [бесплатной пробной версией Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) без подписки Azure, оплаты и каких-либо обязательств.

Кроме того, вам потребуется:
* [Python](https://www.python.org/downloads/) версии 2.7.14
* [Git](https://git-scm.com/)
* [Драйвер Python для Apache Cassandra](https://github.com/datastax/python-driver)

## <a name="create-a-database-account"></a>Создание учетной записи базы данных

Прежде чем создавать базу данных документов, необходимо создать в Azure Cosmos DB учетную запись Cassandra.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Клонирование примера приложения

Теперь необходимо клонировать приложение API Cassandra из GitHub, задать строку подключения и запустить приложение. Вы узнаете, как можно упростить работу с данными программным способом. 

1. Откройте окно командной строки. Создайте папку с именем `git-samples`. Затем закройте командную строку.

    ```bash
    md "C:\git-samples"
    ```

2. Откройте окно терминала git, например git bash, и выполните команду `cd`, чтобы перейти в новую папку для установки примера приложения.

    ```bash
    cd "C:\git-samples"
    ```

3. Выполните команду ниже, чтобы клонировать репозиторий с примером. Эта команда создает копию примера приложения на локальном компьютере.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Просмотр кода

Этот шаг не является обязательным. Если вы хотите узнать, как создавать ресурсы базы данных в коде, изучите приведенные ниже фрагменты кода. Фрагменты кода взяты из файла pyquickstart.py. Если вас это не интересует, можете сразу переходить к разделу [Обновление строки подключения](#update-your-connection-string). 

* Для указания имени пользователя и пароля используется страница строки подключения на портале Azure. `path\to\cert` указывает путь к сертификату X509. 

   ```python
    ssl_opts = {
            'ca_certs': 'path\to\cert',
            'ssl_version': ssl.PROTOCOL_TLSv1_2
            }
    auth_provider = PlainTextAuthProvider( username=cfg.config['username'], password=cfg.config['password'])
    cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider, ssl_options=ssl_opts)
    session = cluster.connect()
   
   ```

* `cluster` инициализируется со сведениями contactPoint. contactPoint извлекается с портала Azure.

    ```python
   cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider)
    ```

* `cluster` подключается к API Cassandra Azure Cosmos DB.

    ```python
    session = cluster.connect()
    ```

* Создается пространство ключей.

    ```python
   session.execute('CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }')
    ```

* Создается таблица.

   ```
   session.execute('CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)');
   ```

* Вставляются сущности ключа и значения.

    ```Python
    insert_data = session.prepare("INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)")
    session.execute(insert_data, [1,'Lybkov','Seattle'])
    session.execute(insert_data, [2,'Doniv','Dubai'])
    session.execute(insert_data, [3,'Keviv','Chennai'])
    session.execute(insert_data, [4,'Ehtevs','Pune'])
    session.execute(insert_data, [5,'Dnivog','Belgaum'])
    ....
    
    ```

* Запрос на получение всех значений ключа.

    ```Python
    rows = session.execute('SELECT * FROM uprofile.user')
    ```  
    
* Запрос на получение значения ключа.

    ```Python
    
    rows = session.execute('SELECT * FROM uprofile.user where user_id=1')
    ```  

## <a name="update-your-connection-string"></a>Обновление строки подключения

Теперь вернитесь на портал Azure, чтобы получить данные строки подключения. Скопируйте эти данные в приложение. Строка подключения обеспечивает обмен данными между вашим приложением и размещенной базой данных.

1. На [портале Azure](https://portal.azure.com/) выберите **Строка подключения**. 

    Вы можете использовать кнопку ![Кнопка "Скопировать"](./media/create-cassandra-python/copy.png) в правой части экрана, чтобы скопировать значение параметра Contact point (Контакт).

    ![Просмотр и копирование имени пользователя, пароля и контакта в колонку строки подключения на портале Azure](./media/create-cassandra-python/keys.png)

2. Откройте файл `config.py` . 

3. Вставьте полученное на портале значение параметра Contact point (Контакт) над элементом `<FILLME>` в строке 10.

    Теперь строка 10 должна выглядеть примерно так: 

    `'contactPoint': 'cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350'`

4. Скопируйте значение параметра "Пользователь" на портале и вставьте его над элементом `<FILLME>` в строке 6.

    Теперь строка 6 должна выглядеть примерно так: 

    `'username': 'cosmos-db-quickstart',`
    
5. Скопируйте значение параметра "Пароль" с портала и вставьте его над элементом `<FILLME>` в строке 8.

    Теперь строка 8 должна выглядеть примерно так:

    `'password' = '2Ggkr662ifxz2Mg==`';`

6. Сохраните файл config.py.
    
## <a name="use-the-x509-certificate"></a>Использование сертификата X509

1. Скачайте сертификат Baltimore CyberTrust Root на локальный компьютер из [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt). Переименуйте файл, указав расширение файла `.cer`.

   Сертификат имеет серийный номер `02:00:00:b9` и отпечаток SHA1 `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.

2. Откройте `pyquickstart.py` и измените `path\to\cert` таким образом, чтобы они указывали на новый сертификат.

3. Сохраните `pyquickstart.py`.

## <a name="run-the-python-app"></a>Запуск приложения Python

1. Выполните команду cd в окне терминала git, чтобы переключиться на папку `azure-cosmos-db-cassandra-python-getting-started`. 

2. Чтобы установить необходимые модули, выполните следующие команды:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Запустите веб-приложение с помощью следующей команды:

    ```
    python pyquickstart.py
    ```

3. Проверьте результаты из командной строки.

    Нажмите клавиши CTRL+C, чтобы остановить выполнение программы и закрыть окно консоли. 

    ![Просмотр и проверка выходных данных](./media/create-cassandra-python/output.png)
    
4. На портале Azure откройте **обозреватель данных**, чтобы запросить, изменить и обработать новые данные. 

    ![Просмотр данных в обозревателе данных](./media/create-cassandra-python/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Просмотр соглашений об уровне обслуживания на портале Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве описано, как создать учетную запись Azure Cosmos DB и контейнер с помощью обозревателя данных, а также как запустить приложение. Теперь можно импортировать дополнительные данные в учетную запись Azure Cosmos DB. 

> [!div class="nextstepaction"]
> [Импорт данных Cassandra в Azure Cosmos DB](cassandra-import-data.md)

