---
title: Руководство. Интеграция Azure Active Directory с GaggleAMP | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении GaggleAMP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce28f3667689134a2de177ed5c0dfae810dc1889
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173578"
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Руководство по Интеграция Azure Active Directory с GaggleAMP

В этом руководстве описано, как интегрировать GaggleAMP с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением GaggleAMP обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к приложению GaggleAMP.
- Вы можете включить автоматический вход пользователей в GaggleAMP (единый вход) под учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с GaggleAMP, вам потребуется:

- подписка Azure AD;
- подписка на GaggleAMP с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление GaggleAMP из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-gaggleamp-from-the-gallery"></a>Добавление GaggleAMP из коллекции
Чтобы настроить интеграцию GaggleAMP с Azure AD, необходимо добавить GaggleAMP из коллекции в список управляемых приложений SaaS.

**Чтобы добавить GaggleAMP из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **GaggleAMP**.

    ![Создание тестового пользователя Azure AD](./media/gaggleamp-tutorial/tutorial_gaggleamp_search.png)

1. На панели результатов выберите **GaggleAMP** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в приложение GaggleAMP для тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в GaggleAMP соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в GaggleAMP.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в GaggleAMP.

Чтобы настроить и проверить единый вход Azure AD в GaggleAMP, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя GaggleAMP](#creating-a-gaggleamp-test-user)** требуется для создания пользователя Britta Simon в GaggleAMP, связанного с представлением этого пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении GaggleAMP.

**Чтобы настроить единый вход Azure AD в GaggleAMP, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **GaggleAMP** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

1. Если вы хотите настроить приложение в режиме, инициированном поставщиком удостоверений, в разделе**Домены и URL-адреса приложения GaggleAMP** выполните следующие действия:

    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     В текстовом поле **Идентификатор** введите URL-адрес `https://accounts.gaggleamp.com/auth/saml/callback`.

1. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_gaggleamp_url1.png)

     В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://gaggleamp.com/i/<customerid>`

    > [!NOTE]
    > Значение URL-адреса входа приведено для примера. Вместо него необходимо указать фактический URL-адрес для входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов GaggleAMP](mailto:sales@gaggleamp.com).
 
1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_general_400.png)

1. В разделе **Настройка GaggleAMP** щелкните **Настроить GaggleAMP**, чтобы открыть окно **Настройка единого входа**. Скопируйте **идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

1. В другом окне браузера перейдите на страницу единого входа SAML, созданную для вас группой поддержки Gaggle (например, *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).

1. На странице **Единый вход SAML** выполните следующие действия.  
   
    ![Единый вход в GaggleAMP](./media/gaggleamp-tutorial/tutorial_gaggleamp_06.png)

    a. Выберите **Other** (Другое) из раскрывающегося списка **SAML Identity Provider** (Поставщик удостоверений SAML).
    
    б) В текстовое поле **Издатель поставщика удостоверений** вставьте значение **URL-адреса издателя**, скопированное на портале Azure.
    
    c. В текстовое поле **URL-адрес единого входа поставщика удостоверений** вставьте значение **URL-адреса службы единого входа**, скопированное на портале Azure.
    
    4.3. Откройте скачанный файл **сертификата в кодировке Base64** в Блокноте, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **Certificate X.509** (Сертификат X.509).
    
    д. Выберите команду **Сохранить**.

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/gaggleamp-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/gaggleamp-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/gaggleamp-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/gaggleamp-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    б) В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    4.3. Нажмите кнопку **Создать**.
 
### <a name="creating-a-gaggleamp-test-user"></a>Создание тестового пользователя GaggleAMP

Цель этого раздела — создать в приложении GaggleAMP пользователя с именем Britta Simon. Приложение GaggleAMP поддерживает JIT-подготовку. Эта функция включена по умолчанию.

В этом разделе никакие действия с вашей стороны не требуются. Новый пользователь будет создан при попытке получить доступ к GaggleAMP (если он еще не создан). 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon, предоставив этому пользователю доступ к GaggleAMP.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в GaggleAMP, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **GaggleAMP**.

    ![Настройка единого входа](./media/gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент GaggleAMP на панели доступа, вы автоматически войдете в приложение GaggleAMP.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/gaggleamp-tutorial/tutorial_general_203.png
