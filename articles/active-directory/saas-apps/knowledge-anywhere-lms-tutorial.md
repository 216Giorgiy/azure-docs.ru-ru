---
title: Руководство. Интеграция Azure Active Directory с Knowledge Anywhere LMS | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Knowledge Anywhere LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5cfa07b1-a792-4f0a-8c6f-1a13142193d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.openlocfilehash: f39952c74006964155fd23920c85506cac13a878
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57892730"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledge-anywhere-lms"></a>Руководство. интеграции Azure Active Directory с Knowledge Anywhere LMS

В этом руководстве описано, как интегрировать приложение Knowledge Anywhere LMS с Azure Active Directory (Azure AD).
Интеграция Knowledge Anywhere LMS с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Knowledge Anywhere LMS.
* Вы можете включить автоматический вход пользователей в Knowledge Anywhere LMS (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Knowledge Anywhere LMS, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Knowledge Anywhere LMS с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Knowledge Anywhere LMS поддерживает единый вход, инициируемый **поставщиком услуг и поставщиком удостоверений**.
* Knowledge Anywhere LMS поддерживает **JIT**-подготовку пользователей.

## <a name="adding-knowledge-anywhere-lms-from-the-gallery"></a>Добавление Knowledge Anywhere LMS из коллекции

Чтобы настроить интеграцию Knowledge Anywhere LMS с Azure AD, нужно добавить Knowledge Anywhere LMS из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Knowledge Anywhere LMS из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Knowledge Anywhere LMS**, выберите **Knowledge Anywhere LMS** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Knowledge Anywhere LMS в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Knowledge Anywhere LMS с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Knowledge Anywhere LMS.

Чтобы настроить и проверить единый вход Azure AD в Knowledge Anywhere LMS, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Knowledge Anywhere LMS](#configure-knowledge-anywhere-lms-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Knowledge Anywhere LMS](#create-knowledge-anywhere-lms-test-user)** требуется, чтобы в Knowledge Anywhere LMS существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Knowledge Anywhere LMS, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Knowledge Anywhere LMS** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Knowledge Anywhere LMS](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<CLIENTNAME>.knowledgeanywhere.com/`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<CLIENTNAME>.knowledgeanywhere.com/SSO/SAML/Response.aspx?<IDPNAME>`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените эти значения фактическими URL-адресом ответа и идентификатором, как описано позже в этом руководстве.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Knowledge Anywhere LMS](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<CLIENTNAME>.knowledgeanywhere.com/`.

    > [!NOTE]
    > Значение URL-адреса входа приведено для примера. Вместо него необходимо указать фактический URL-адрес для входа. Для получения данного значения обратитесь в [службу поддержки клиентов Knowledge Anywhere LMS](https://knowany.zendesk.com/hc/en-us/articles/360000469034-SAML-2-0-Single-Sign-On-SSO-Set-Up-Guide). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. Скопируйте требуемый URL-адрес из раздела **Настройка Knowledge Anywhere LMS**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-knowledge-anywhere-lms-single-sign-on"></a>Настройка единого входа Knowledge Anywhere LMS

1. В новом окне браузера откройте портал администрирования Knowledge Anywhere LMS.

2. Выберите вкладку **Сайт**.

    ![Конфигурация Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure1.png)

3. Выберите вкладку **SAML Settings** (Параметры SAML).

    ![Конфигурация Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure2.png)

4. Щелкните **Добавить**.

    ![Конфигурация Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure3.png)

5. На странице **Add/Update SAML Settings** (Добавить/обновить параметры SAML) выполните следующие действия.

    ![Конфигурация Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure4.png)

    a. Введите имя поставщика удостоверений в соответствии с вашей организацией. Например: `Azure`.

    b. В текстовое поле **IDP Entity ID** (Идентификатор сущности поставщика удостоверений) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    c. В текстовое поле **IDP URL** (URL-адрес IDP) вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    d. Откройте в блокноте скачанный с портала Azure файл сертификата, скопируйте содержимое сертификата и вставьте его в текстовое поле **Сертификат**.

    д. В текстовое поле **URL-адрес выхода** вставьте значение **URL-адреса выхода**, скопированное на портале Azure.

    Е. Выберите **Main Site** (Основной сайт) из раскрывающегося списка для параметра **Домен**.

    ж. Скопируйте значение **SP Entity ID** (Идентификатор сущности поставщика услуг) и вставьте его в текстовое поле **Идентификатор** в разделе **Базовая конфигурация SAML** на портале Azure.

    h. Скопируйте **SP Response(ACS) URL** (URL-адрес отклика (ACS) поставщика услуг) и вставьте его в текстовое поле **URL-адрес ответа** в разделе **Базовая конфигурация SAML** на портале Azure.

    i. Выберите команду **Сохранить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon\@домен_вашей_компании.доменная_зона**.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к Knowledge Anywhere LMS.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Knowledge Anywhere LMS**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Knowledge Anywhere LMS**.

    ![Ссылка на Knowledge Anywhere LMS в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-knowledge-anywhere-lms-test-user"></a>Создание тестового пользователя Knowledge Anywhere LMS

В этом разделе вы создадите в Knowledge Anywhere LMS пользователя Britta Simon. Приложение Knowledge Anywhere LMS поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Knowledge Anywhere LMS, он создается после проверки подлинности.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Knowledge Anywhere LMS на панели доступа, вы автоматически войдете в приложение Knowledge Anywhere LMS, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)