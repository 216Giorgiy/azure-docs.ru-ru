---
title: Руководство. Интеграция Azure Active Directory с Thoughtworks Mingle | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Thoughtworks Mingle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: fa46db38511f54b0ddae2b9897c6c821abc42dc9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180795"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Руководство. Интеграция Azure Active Directory с Thoughtworks Mingle

В этом руководстве описано, как интегрировать Thoughtworks Mingle с Azure Active Directory (Azure AD).

Интеграция Azure AD с Thoughtworks Mingle обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Thoughtworks Mingle.
- Вы можете включить автоматический вход пользователей в Thoughtworks Mingle (единый вход) с применением учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Thoughtworks Mingle, вам потребуется:

- подписка Azure AD;
- подписка Thoughtworks Mingle с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Thoughtworks Mingle из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Добавление Thoughtworks Mingle из коллекции
Чтобы настроить интеграцию Thoughtworks Mingle с Azure AD, необходимо добавить Thoughtworks Mingle из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Thoughtworks Mingle из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **Thoughtworks Mingle**, выберите **Thoughtworks Mingle** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![ThoughtWorks Mingle в списке результатов](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в Thoughtworks Mingle с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Thoughtworks Mingle соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Thoughtworks Mingle.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Thoughtworks Mingle.

Чтобы настроить и проверить единый вход Azure AD в Thoughtworks Mingle, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)** требуется для того, чтобы в Thoughtworks Mingle существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе мы включим на портале Azure единый вход Azure AD и настроим его в приложении Thoughtworks Mingle.

**Чтобы настроить единый вход Azure AD в Thoughtworks Mingle, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Thoughtworks Mingle** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Thoughtworks Mingle** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Thoughtworks Mingle](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов Thoughtworks Mingle](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

1. Войдите на корпоративный веб-сайт **Thoughtworks Mingle** в качестве администратора.

1. Откройте вкладку **Admin** (Администратор), а затем щелкните **SSO Config** (Конфигурация единого входа).
   
    ![Вкладка Admin](./media/thoughtworks-mingle-tutorial/ic785157.png "Настройка единого входа")

1. В разделе **Конфигурация единого входа** выполните следующие действия.
   
    ![Настройка единого входа](./media/thoughtworks-mingle-tutorial/ic785158.png "Настройка единого входа")
    
    a. Чтобы отправить файл метаданных, нажмите кнопку **Выбрать файл**. 

    b. Нажмите кнопку **Сохранить изменения**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Кнопка "Добавить"](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Диалоговое окно "Пользователь"](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Создание тестового пользователя Thoughtworks Mingle

Чтобы пользователи AAD могли войти систему, их необходимо подготовить для приложения Thoughtworks Mingle, используя их имена пользователей Azure Active Directory. В случае с Thoughtworks Mingle подготовка выполняется вручную.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Войдите на корпоративный веб-сайт Thoughtworks Mingle в качестве администратора.

1. Щелкните **Профиль**.
   
    ![Ваш первый проект](./media/thoughtworks-mingle-tutorial/ic785160.png "Ваш первый проект")

1. Откройте вкладку **Admin** (Администратор), а затем щелкните **Users** (Пользователи).
   
    ![Пользователи](./media/thoughtworks-mingle-tutorial/ic785161.png "Пользователи")

1. Щелкните **Новый пользователь**.
   
    ![Новый пользователь](./media/thoughtworks-mingle-tutorial/ic785162.png "Новый пользователь")

1. На странице диалогового окна **Новый пользователь** выполните следующие действия.
   
    ![Диалоговое окно нового пользователя](./media/thoughtworks-mingle-tutorial/ic785163.png "Новый пользователь")  
 
    a. Введите в текстовые поля **Sign-in name** (Учетное имя), **Display name** (Отображаемое имя), **Choose password** (Выберите пароль) и **Confirm password** (Подтвердите пароль) соответствующие данные действующей учетной записи Azure AD, которую вы хотите подготовить. 

    b. В поле **User type** (Тип пользователя) выберите значение **Full user** (С полным доступом).

    c. Щелкните **Создать этот профиль**.

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетных записей пользователя Thoughtworks Mingle или API-интерфейсы, предоставляемые Thoughtworks Mingle для подготовки учетных записей пользователей AAD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Thoughtworks Mingle, чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Thoughtworks Mingle, выполните указанные ниже действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Thoughtworks Mingle**.

    ![Ссылка на Thoughtworks Mingle в списке "Приложения"](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Thoughtworks Mingle на панели доступа, вы автоматически войдете в приложение Thoughtworks Mingle.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

