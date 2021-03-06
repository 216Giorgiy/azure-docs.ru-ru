---
title: Руководство по Интеграция Azure Active Directory с PagerDuty | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении PagerDuty.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: ded5854c5e669ab1982641169f13a9cb400d5d6d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59270121"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Руководство по Интеграция Azure Active Directory с PagerDuty

В этом руководстве описано, как интегрировать PagerDuty с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением PagerDuty обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к PagerDuty.
* Вы можете включить автоматический вход пользователей в PagerDuty (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением PagerDuty, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка PagerDuty с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* PagerDuty поддерживает единый вход, инициированный **поставщиком услуг**

## <a name="adding-pagerduty-from-the-gallery"></a>Добавление PagerDuty из коллекции

Чтобы настроить интеграцию PagerDuty с Azure AD, необходимо добавить PagerDuty из коллекции в список управляемых приложений SaaS.

**Чтобы добавить PagerDuty из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **PagerDuty**, выберите **PagerDuty** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![PagerDuty в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в PagerDuty с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в PagerDuty.

Чтобы настроить и проверить единый вход Azure AD в PagerDuty, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в PagerDuty](#configure-pagerduty-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя PagerDuty](#create-pagerduty-test-user)** требуется для того, чтобы в приложении PagerDuty существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в PagerDuty, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **PagerDuty** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения PagerDuty](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<tenant-name>.pagerduty.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<tenant-name>.pagerduty.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить их, обратитесь в [службу поддержки клиентов PagerDuty](https://www.pagerduty.com/support/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка PagerDuty**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-pagerduty-single-sign-on"></a>Настройка единого входа PagerDuty

1. В другом окне браузера войдите на свой сайт PagerDuty компании в качестве администратора.

2. В меню в верхней части страницы щелкните **Параметры учетной записи**.

    ![Параметры учетной записи](./media/pagerduty-tutorial/ic778535.png "параметры учетной записи")

3. Щелкните **Single Sign-on**(Единый вход).

    ![Единый вход](./media/pagerduty-tutorial/ic778536.png "Единый вход")

4. На странице **Включить единый вход** выполните следующие действия.

    ![Разрешить единый вход](./media/pagerduty-tutorial/ic778537.png "Разрешить единый вход")

    a. Откройте в Блокноте сертификат в кодировке Base-64, скачанный с портала Azure, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **X.509 Certificate** (Сертификат X.509).
  
    b. В текстовое поле **URL-адрес для входа** вставьте **URL-адрес входа**, скопированный на портале Azure.
  
    c. В текстовое поле **URL-адрес выхода** вставьте **URL-адрес выхода**, скопированный на портале Azure.

    d. Установите флажок **Allow username/password login** (Разрешить вход по имени пользователя и паролю).

    д. Установите флажок **Require EXACT authentication context comparison** (Требовать точное сравнение контекста аутентификации).

    Е. Нажмите кнопку **Сохранить изменения**.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к PagerDuty.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **PagerDuty**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **PagerDuty**.

    ![Ссылка на PagerDuty в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-pagerduty-test-user"></a>Создание тестового пользователя PagerDuty

Чтобы пользователи Azure AD могли выполнять вход в PagerDuty, они должны быть подготовлены для PagerDuty.  
В случае с PagerDuty подготовка выполняется вручную.

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетных записей пользователя PagerDuty или API, предоставляемые PagerDuty для подготовки учетных записей Azure Active Directory.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Выполните вход в клиент **PagerDuty** .

2. В меню в верхней части страницы щелкните **Пользователи**.

3. Щелкните **Добавить пользователей**.
   
    ![Добавление пользователей](./media/pagerduty-tutorial/ic778539.png "добавление пользователей")

4.  В диалоговом окне **Invite your team** (Пригласить команду) выполните следующие действия.
   
    ![Пригласить команду](./media/pagerduty-tutorial/ic778540.png "Пригласить команду")

    a. В текстовое поле **First and Last Name** (Имя и фамилия) введите сведения о пользователе, например **Britta Simon**. 
   
    b. Введите адрес **электронной почты** пользователя, например **brittasimon\@contoso.com**.
   
    c. Нажмите **Add** (Добавить), затем нажмите **Send Invites** (Отправить приглашения).
   
    >[!NOTE]
    >Все добавленные пользователи получат приглашение создать учетную запись PagerDuty.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку PagerDuty на Панели доступа, вы автоматически войдете в приложение PagerDuty, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

