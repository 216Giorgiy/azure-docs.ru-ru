---
title: Руководство по Интеграция Azure Active Directory с Jitbit Helpdesk | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Jitbit Helpdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 851b28d10bdf0b2df67e1c0782a683e790b711bc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59266500"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Руководство по Интеграция Azure Active Directory с Jitbit Helpdesk

В этом руководстве описано, как интегрировать Jitbit Helpdesk с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Jitbit Helpdesk обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Jitbit Helpdesk.
* Вы можете включить автоматический вход пользователей в Jitbit Helpdesk (единый вход) с использованием учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Jitbit Helpdesk, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка с поддержкой единого входа Jitbit Helpdesk.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Jitbit Helpdesk поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Добавление Jitbit Helpdesk из коллекции.

Чтобы настроить интеграцию Jitbit Helpdesk с Azure AD, необходимо добавить Jitbit Helpdesk из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Jitbit Helpdesk из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Jitbit Helpdesk**, выберите **Jitbit Helpdesk** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Jitbit Helpdesk в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описаны настройка и проверка единого входа Azure AD в Jitbit Helpdesk с использованием тестового пользователя **Britta Simon**.
Чтобы единый вход функционировал, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Jitbit Helpdesk.

Чтобы настроить и проверить единый вход Azure AD в Jitbit Helpdesk, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Jitbit Helpdesk](#configure-jitbit-helpdesk-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Jitbit Helpdesk](#create-jitbit-helpdesk-test-user)** требуется для создания в Jitbit Helpdesk пользователя Britta Simon, связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Jitbit Helpdesk, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Jitbit Helpdesk** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Jitbit Helpdesk](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    | |
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Для получения этого значения обратитесь в [службу поддержки клиентов Jitbit Helpdesk](https://www.jitbit.com/support/).

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес следующим образом: `https://www.jitbit.com/web-helpdesk/`

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка Jitbit Helpdesk**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-jitbit-helpdesk-single-sign-on"></a>Настройка единого входа в Jitbit Helpdesk

1. В другом окне браузера войдите на свой корпоративный веб-сайт Jitbit Helpdesk в качестве администратора.

1. На панели инструментов в верхней части страницы щелкните **Администрирование**.

    ![Администрирование](./media/jitbit-helpdesk-tutorial/ic777681.png "Администрирование")

1. Щелкните **Общие параметры**.

    ![Пользователи, организации и разрешения](./media/jitbit-helpdesk-tutorial/ic777680.png "Пользователи, организации и разрешения")

1. В разделе конфигурации **Параметры проверки подлинности** сделайте следующее:

    ![Параметры аутентификации](./media/jitbit-helpdesk-tutorial/ic777683.png "Параметры аутентификации")

    a. Установите флажок **Enable SAML 2.0 single sign on** (Включить единый вход SAML 2.0) для выполнения единого входа с помощью **OneLogin**.

    b. В текстовое поле **URL-адрес конечной точки** вставьте **URL-адрес входа**, скопированный на портале Azure.

    c. Откройте сертификат в кодировке **Base-64** в Блокноте, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **Сертификат X.509**.

    d. Нажмите кнопку **Сохранить изменения**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension**.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Jitbit Helpdesk.

1. На портале Azure перейдите в колонку **Корпоративные приложения** и последовательно выберите **Все приложения**, **Jitbit Helpdesk**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Jitbit Helpdesk**.

    ![Ссылка на Jitbit Helpdesk в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-jitbit-helpdesk-test-user"></a>Создание тестового пользователя Jitbit Helpdesk

Чтобы пользователи Azure AD могли выполнять вход в Jitbit Helpdesk, они должны быть подготовлены для Jitbit Helpdesk. В случае с Jitbit Helpdesk подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Выполните вход в клиент **Jitbit Helpdesk**.

1. В верхнем меню щелкните **Администрирование**.

    ![Администрирование](./media/jitbit-helpdesk-tutorial/ic777681.png "Администрирование")

1. Нажмите кнопку **Пользователи, компании и разрешения**.

    ![Пользователи, организации и разрешения](./media/jitbit-helpdesk-tutorial/ic777682.png "Пользователи, организации и разрешения")

1. Нажмите кнопку **Добавить пользователя**.

    ![Добавление пользователя](./media/jitbit-helpdesk-tutorial/ic777685.png "Добавление пользователя")

1. В разделе Create (Создание) введите данные учетной записи Azure AD, которую необходимо подготовить, в следующие текстовые поля:

    ![Создание](./media/jitbit-helpdesk-tutorial/ic777686.png "Создание")

   a. В текстовом поле **Имя пользователя** введите имя, например **BrittaSimon**.

   b. В текстовое поле **Email** (Адрес электронной почты) введите адрес электронной почты пользователя, например **BrittaSimon@contoso.com**.

   c. В текстовое поле **First Name** (Имя) введите имя пользователя, например **Britta**.

   d. В текстовое поле **Last Name** (Фамилия) введите фамилию, например, **Simon**.

   д. Нажмите кнопку **Создать**.

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя Jitbit Helpdesk или API, предоставляемые Jitbit Helpdesk для подготовки учетных записей пользователя Azure AD.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Jitbit Helpdesk на панели доступа, вы автоматически войдете в приложение Jitbit Helpdesk, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
