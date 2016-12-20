---
title: "Регистрация данных из Data Lake Store в каталоге данных Azure | Документация Майкрософт"
description: "Регистрация данных из хранилища озера данных в каталоге данных Azure"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: c881b378a96b9d3eca7018bc32154a265ec524ea
ms.openlocfilehash: 893892759b0cac38c0baa5ed3d56addb0dd75065


---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Регистрация данных из хранилища озера данных в каталоге данных Azure
В этой статье вы узнаете, как интегрировать хранилище озера данных Azure с каталогом данных Azure, чтобы в организации можно было обнаруживать данные с помощью интеграции с каталогом данных. Дополнительные сведения о каталогизации данных см. в статье [Каталог данных Azure](../data-catalog/data-catalog-what-is-data-catalog.md). Чтобы понять, в каких сценариях можно использовать каталог данных, см. статью [Типичные сценарии каталога данных Azure](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Предварительные требования
Перед началом работы с этим учебником необходимо иметь следующее:

* **Подписка Azure**. Ознакомьтесь с [бесплатной пробной версией Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Настройте свою подписку Azure** для использования общедоступной предварительной версии Data Lake Store. Ознакомьтесь с [инструкциями](data-lake-store-get-started-portal.md).
* **Учетная запись хранилища озера данных Azure**. Следуйте инструкциям в разделе [Приступая к работе с хранилищем озера данных Azure на портале Azure](data-lake-store-get-started-portal.md). Давайте в целях этого учебника создадим учетную запись хранилища данных озера и назовем ее **datacatalogstore**.

    После создания учетной записи передайте в нее пример набора данных. В этом учебнике мы передадим CSV-файлы в папку **AmbulanceData** в [репозитории Git озера данных Azure](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Чтобы передать данные в контейнер больших двоичных объектов, можно использовать различные клиенты, например [обозреватель хранилищ Azure](http://storageexplorer.com/).
* **Каталог данных Azure**. В организации уже должен быть создан каталог данных Azure. Для каждой организации допускается только один каталог.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Регистрация хранилища озера данных в качестве источника для каталога данных

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Перейдите на страницу `https://azure.microsoft.com/services/data-catalog`и щелкните **Начало работы**.
2. Войдите на портал каталога данных Azure и щелкните **Опубликовать данные**.

    ![Регистрация источника данных](./media/data-lake-store-with-data-catalog/register-data-source.png "Register a data source")
3. На следующей странице щелкните **Запустить приложение**. На ваш компьютер будет скачан файл манифеста приложения. Дважды щелкните этот файл манифеста, чтобы запустить приложение.
4. На странице "Приветствие" щелкните **Войти**и введите учетные данные.

    ![Экран приветствия](./media/data-lake-store-with-data-catalog/welcome.screen.png "Welcome screen")
5. На странице "Выбор источника данных" выберите **Azure Data Lake**, а затем нажмите кнопку **Далее**.

    ![Выберите источник данных](./media/data-lake-store-with-data-catalog/select-source.png "Select data source")
6. На следующей странице укажите имя учетной записи хранилища озера данных, которую необходимо зарегистрировать в каталоге данных. Оставьте значения по умолчанию для остальных параметров и щелкните **Подключиться**.

    ![Подключение к источнику данных](./media/data-lake-store-with-data-catalog/connect-to-source.png "Connect to data source")
7. Следующую страницу можно разделить на следующие области.

    а. Поле **Иерархия серверов** представляет структуру папки учетной записи хранилища озера данных. **$Root** представляет корень учетной записи Data Lake Store, а **AmbulanceData** — папку, созданную в корне учетной записи Data Lake Store.

    b. В поле **Доступные объекты** перечислены файлы и папки, расположенные в папке **AmbulanceData**.

    c. **Объекты для регистрации** перечислены файлы и папки, которые вы хотите зарегистрировать в каталоге данных Azure.

    ![Просмотр структуры данных](./media/data-lake-store-with-data-catalog/view-data-structure.png "View data structure")
8. В рамках этого учебника необходимо зарегистрировать все файлы в каталоге. Для этого нажмите кнопку (![Перемещение объектов](./media/data-lake-store-with-data-catalog/move-objects.png "Move objects")), чтобы переместить все файлы в поле **Объекты для регистрации** .

    Так как данные будут зарегистрированы в каталоге данных на уровне всей организации, рекомендуется добавить какие-либо метаданные, которые позже можно будет использовать для быстрого поиска данных. Скажем, можно добавить электронный адрес владельца данных (например, того, кто передает данные) или добавить тег для идентификации данных. На снимке экрана ниже показан тег, добавляемый к данным.

    ![Просмотр структуры данных](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "View data structure")

    Щелкните **Зарегистрировать**.
9. На следующем снимке экрана показано, что данные успешно зарегистрированы в каталоге данных.

    ![Регистрация завершена](./media/data-lake-store-with-data-catalog/registration-complete.png "View data structure")
10. Щелкните **Просмотреть портал** , чтобы вернуться на портал каталога данных и убедиться, что теперь вы можете обращаться к зарегистрированным данным на портале. Для поиска данных можно использовать тег, который вы добавили при регистрации данных.

     ![Поиск данных в каталоге](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Search data in catalog")
11. Теперь можно выполнять такие операции, как добавление аннотаций и документации к данным. Дополнительные сведения см. по следующим ссылкам.

    * [Создание заметок к источникам данных](../data-catalog/data-catalog-how-to-annotate.md)
    * [Создание документации по источникам данных](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Дополнительные материалы
* [Создание заметок к источникам данных](../data-catalog/data-catalog-how-to-annotate.md)
* [Создание документации по источникам данных](../data-catalog/data-catalog-how-to-documentation.md)
* [Интеграция хранилища озера данных с другими службами Azure](data-lake-store-integrate-with-other-services.md)



<!--HONumber=Nov16_HO3-->


