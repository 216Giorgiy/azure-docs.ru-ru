---
title: Руководство. Интеграция Azure Active Directory с moconavi | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 3009cb42ac477b18d45ab5968d6f5793ce1cd36c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165903"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Руководство. Интеграция Azure Active Directory с moconavi

В этом руководстве описано, как интегрировать moconavi с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением moconavi обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к moconavi.
- Вы можете включить автоматический вход пользователей в moconavi (единый вход) с использованием учетных записей Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с moconavi, вам потребуется:

- подписка Azure AD;
- подписка moconavi с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде.
Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление moconavi из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-moconavi-from-the-gallery"></a>Добавление moconavi из коллекции
Чтобы настроить интеграцию moconavi с Azure AD, необходимо добавить moconavi из коллекции в список управляемых приложений SaaS.

**Чтобы добавить moconavi из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **moconavi**, выберите **moconavi** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Приложение moconavi в списке результатов](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в moconavi с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в moconavi соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в moconavi.

Чтобы настроить и проверить единый вход Azure AD в moconavi, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя moconavi](#create-a-moconavi-test-user)** требуется для того, чтобы в moconavi существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить такой вход в приложении moconavi.

**Чтобы настроить единый вход Azure AD в moconavi, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **moconavi** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Диалоговое окно "Единый вход"](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. В разделе **Домены и URL-адреса приложения moconavi** выполните следующие действия.

    ![Сведения о домене и URL-адресах для единого входа для приложения moconavi](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<yourserverurl>/moconavi-saml2`

    C. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<yourserverurl>/moconavi-saml2/saml/SSO`.

    > [!NOTE]
    > Эти значения приведены в качестве примера. Укажите вместо них фактические значения URL-адреса для входа, идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов moconavi](mailto:support@recomot.co.jp).

4. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/moconavi-tutorial/tutorial_general_400.png)

6. Чтобы настроить единый вход на стороне **moconavi**, отправьте [группе поддержки moconavi](mailto:support@recomot.co.jp) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/moconavi-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/moconavi-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/moconavi-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/moconavi-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="create-a-moconavi-test-user"></a>Создание тестового пользователя moconavi

В этом разделе описано, как создать пользователя Britta Simon в приложении moconavi. Обратитесь к  [группе поддержки moconavi](mailto:support@recomot.co.jp) для добавления пользователей на платформу moconavi. Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к moconavi.

![Назначение роли пользователя][200]

**Чтобы назначить пользователя Britta Simon в moconavi, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201]

2. Из списка приложений выберите **moconavi**.

    ![Ссылка на moconavi в списке "Приложения"](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

1. Установите moconavi из Microsoft Store.

2. Запустите moconavi.

3. Нажмите кнопку **Connect setting** (Настроить подключение).

    ![Проверка единого входа](./media/moconavi-tutorial/testing1.png)

4. Введите `https://mcs-admin.moconavi.biz/gateway` в текстовое поле **Connect to URL** (Подключение по URL-адресу) и нажмите кнопку **Done** (Готово).

    ![Проверка единого входа](./media/moconavi-tutorial/testing2.png)

5. Выполните действия, как показано на снимке экрана ниже.

    ![Проверка единого входа](./media/moconavi-tutorial/testing3.png)

    a. Введите **ключ аутентификации** `azureAD` в текстовое поле **Input Authentication Key** (Ввод ключа аутентификации).

    b. Введите **идентификатор пользователя** `your ad account` в текстовое поле **Input User ID** (Ввод идентификатора пользователя).

    c. Нажмите кнопку **LOGIN** (Войти).

6. В текстовое поле **Password** (Пароль) введите свой пароль Azure AD и нажмите кнопку **Login** (Войти).

    ![Проверка единого входа](./media/moconavi-tutorial/testing4.png)

7. Если появится меню, значит аутентификация Azure AD выполнена успешно.

    ![Проверка единого входа](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/moconavi-tutorial/tutorial_general_01.png
[2]: ./media/moconavi-tutorial/tutorial_general_02.png
[3]: ./media/moconavi-tutorial/tutorial_general_03.png
[4]: ./media/moconavi-tutorial/tutorial_general_04.png

[100]: ./media/moconavi-tutorial/tutorial_general_100.png

[200]: ./media/moconavi-tutorial/tutorial_general_200.png
[201]: ./media/moconavi-tutorial/tutorial_general_201.png
[202]: ./media/moconavi-tutorial/tutorial_general_202.png
[203]: ./media/moconavi-tutorial/tutorial_general_203.png

