---
title: Руководство по Интеграция Azure Active Directory с Veritas Enterprise Vault.cloud SSO | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Veritas Enterprise Vault.cloud SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 59012bf32a4e1f0532b4d42e510d431180c35730
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59264188"
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Руководство по Интеграция Azure Active Directory с Veritas Enterprise Vault.cloud SSO

В этом руководстве описано, как интегрировать Veritas Enterprise Vault.cloud SSO с Azure Active Directory (Azure AD).
Интеграция Veritas Enterprise Vault.cloud SSO с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD можно контролировать доступ к Veritas Enterprise Vault.cloud SSO.
* Вы можете включить автоматический вход пользователей в Veritas Enterprise Vault.cloud SSO (единый вход) с учетной записью Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Veritas Enterprise Vault.cloud SSO, вам потребуется следующее:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Veritas Enterprise Vault.cloud SSO с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Veritas Enterprise Vault.cloud SSO поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Добавление Veritas Enterprise Vault.cloud SSO из коллекции

Чтобы настроить интеграцию Veritas Enterprise Vault.cloud SSO c Azure AD, необходимо добавить Veritas Enterprise Vault.cloud SSO из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Veritas Enterprise Vault.cloud SSO из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Veritas Enterprise Vault.cloud SSO**, выберите **Veritas Enterprise Vault.cloud SSO** на панели результатов, а затем нажмите кнопку **Добавить**, чтобы добавить приложение.

     ![Veritas Enterprise Vault.cloud SSO в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описывается настройка и проверка единого входа Azure AD в Veritas Enterprise Vault.cloud SSO с использованием тестового пользователя **Britta Simon**.
Чтобы настроить единый вход, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Veritas Enterprise Vault.cloud SSO.

Чтобы настроить и проверить единый вход Azure AD в Veritas Enterprise Vault.cloud SSO, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Veritas Enterprise Vault.cloud SSO](#configure-veritas-enterprise-vaultcloud-sso-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Veritas Enterprise Vault.cloud SSO](#create-veritas-enterprise-vaultcloud-sso-test-user)** требуется для создания пользователя Britta Simon в Veritas Enterprise Vault.cloud SSO, связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Veritas Enterprise Vault.cloud SSO, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Veritas Enterprise Vault.cloud SSO** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Veritas Enterprise Vault.cloud SSO](common/sp-identifier-reply.png)

    a. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`.

    b. В поле **Идентификатор** введите URL-адрес соответствующего центра обработки данных:

    | Центр обработки данных| URL-адрес |
    |----------|----|
    | Северная Америка| `https://auth.lax.archivecloud.net` |
    | Европа | `https://auth.ams.archivecloud.net` |
    | Азиатско-Тихоокеанский регион| `https://auth.syd.archivecloud.net`|

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес соответствующего центра обработки данных:

    | Центр обработки данных| URL-адрес |
    |----------|----|
    | Северная Америка| `https://auth.lax.archivecloud.net` |
    | Европа | `https://auth.ams.archivecloud.net` |
    | Азиатско-Тихоокеанский регион| `https://auth.syd.archivecloud.net`|

    > [!NOTE]
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Для получения этого значения обратитесь в [службу поддержки клиентов Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемые URL-адреса вы можете скопировать из раздела **Настройка Veritas Enterprise Vault.cloud SSO**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-veritas-enterprise-vaultcloud-sso-single-sign-on"></a>Настройка единого входа Veritas Enterprise Vault.cloud SSO

Чтобы настроить единый вход на стороне **Veritas Enterprise Vault.cloud SSO**, нужно передать скачанный **сертификат в кодировке Base64** и соответствующие URL-адреса, скопированные на портале Azure, в [службу поддержки Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите brittasimon@yourcompanydomain.extension. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к Veritas Enterprise Vault.cloud SSO.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Veritas Enterprise Vault.cloud SSO**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Veritas Enterprise Vault.cloud SSO**.

    ![Veritas Enterprise Vault.cloud SSO в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-veritas-enterprise-vaultcloud-sso-test-user"></a>Создание тестового пользователя Veritas Enterprise Vault.cloud SSO

В этом разделе описано, как создать пользователя Britta Simon в Veritas Enterprise Vault.cloud SSO. Обратитесь в  [службу поддержки Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html), чтобы добавить пользователей на платформу Veritas Enterprise Vault.cloud SSO. Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Veritas Enterprise Vault.cloud SSO на Панели доступа, вы автоматически войдете в приложение Veritas Enterprise Vault.cloud SSO, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

