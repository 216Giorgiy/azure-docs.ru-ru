---
title: Руководство. Интеграция Azure Active Directory с Manabi Pocket | Документация Майкрософт
description: Сведения о том, как настроить единый вход между Azure Active Directory и Manabi Pocket.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e521099-bf7d-43ab-a0e0-86aa1c9e577e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f5dd7012d280580dca76e50290bc2de4322d55c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198427"
---
# <a name="tutorial-azure-active-directory-integration-with-manabi-pocket"></a>Руководство. Интеграция Azure Active Directory с Manabi Pocket

В этом руководстве описано, как интегрировать Manabi Pocket с Azure Active Directory (Azure AD).

Интеграция Manabi Pocket с Azure AD обеспечивает следующие преимущества:

- C помощью Azure AD вы можете контролировать доступ к Manabi Pocket.
- Вы можете включить автоматический вход пользователей в Manabi Pocket (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Manabi Pocket, вам потребуется:

- подписка Azure AD;
- подписка с поддержкой единого входа Manabi Pocket.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Manabi Pocket из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-manabi-pocket-from-the-gallery"></a>Добавление Manabi Pocket из коллекции
Чтобы настроить интеграцию Manabi Pocket с Azure AD, нужно добавить Manabi Pocket из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Manabi Pocket из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **Manabi Pocket**, выберите **Manabi Pocket** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Manabi Pocket в списке результатов](./media/manabipocket-tutorial/tutorial_manabipocket_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Manabi Pocket с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD нужно знать, какой пользователь в Manabi Pocket соответствует пользователю в Azure AD. Иными словами, нужно установить связь между пользователем Azure AD и соответствующим пользователем в Manabi Pocket.

Чтобы настроить и проверить единый вход Azure AD в Manabi Pocket, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Manabi Pocket](#create-a-manabi-pocket-test-user)** требуется для того, чтобы в Manabi Pocket существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Manabi Pocket.

**Чтобы настроить единый вход Azure AD в Manabi Pocket, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Manabi Pocket** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Диалоговое окно "Единый вход"](./media/manabipocket-tutorial/tutorial_manabipocket_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Manabi Pocket** выполните следующие действия:

    ![Сведения о домене и URL-адресах единого входа для приложения Manabi Pocket](./media/manabipocket-tutorial/tutorial_manabipocket_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес: `https://ed-cl.com/`

    б) В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<SERVER-NAME>.ed-cl.com/<TENANT-ID>/idp/provider`

    > [!NOTE]
    > Значение идентификатора приведено для примера и не является реальным. Измените это значение на фактический идентификатор. Чтобы получить значение идентификатора, обратитесь в [службу поддержки клиентов Manabi Pocket](mailto:info-ed-cl@ntt.com).

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/manabipocket-tutorial/tutorial_manabipocket_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/manabipocket-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **Manabi Pocket**, отправьте скачанный **XML-файл метаданных** в [службу поддержки Manabi Pocket](mailto:info-ed-cl@ntt.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/manabipocket-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/manabipocket-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/manabipocket-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/manabipocket-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    4.3. Нажмите кнопку **Создать**.
 
### <a name="create-a-manabi-pocket-test-user"></a>Создание тестового пользователя Manabi Pocket

В этом разделе описано, как создать пользователя Britta Simon в приложении Manabi Pocket. Чтобы добавить пользователей на платформу Manabi Pocket, обратитесь в [службу поддержки Manabi Pocket](mailto:info-ed-cl@ntt.com). Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Manabi Pocket.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Manabi Pocket, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Manabi Pocket**.

    ![Ссылка на Manabi Pocket в списке "Приложения"](./media/manabipocket-tutorial/tutorial_manabipocket_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Manabi Pocket на панели доступа, вы автоматически войдете в приложение Manabi Pocket.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/manabipocket-tutorial/tutorial_general_01.png
[2]: ./media/manabipocket-tutorial/tutorial_general_02.png
[3]: ./media/manabipocket-tutorial/tutorial_general_03.png
[4]: ./media/manabipocket-tutorial/tutorial_general_04.png

[100]: ./media/manabipocket-tutorial/tutorial_general_100.png

[200]: ./media/manabipocket-tutorial/tutorial_general_200.png
[201]: ./media/manabipocket-tutorial/tutorial_general_201.png
[202]: ./media/manabipocket-tutorial/tutorial_general_202.png
[203]: ./media/manabipocket-tutorial/tutorial_general_203.png
