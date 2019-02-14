---
title: Руководство по Интеграция Azure Active Directory с Getabstract | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Getabstract.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2b63d048-b529-4fad-9e90-f244323409dd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3725089c8b2e7dc71e652ddcbd20a757d9bbf726
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56169758"
---
# <a name="tutorial-azure-active-directory-integration-with-getabstract"></a>Руководство по Интеграция Azure Active Directory с Getabstract

В этом руководстве описано, как интегрировать Getabstract с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Getabstract обеспечивает следующие преимущества:

- C помощью Azure AD вы можете управлять доступом к Getabstract.
- Вы можете включить автоматический вход пользователей в Getabstract (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Getabstract, вам потребуется:

- подписка Azure AD;
- подписка Getabstract с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Getabstract из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-getabstract-from-the-gallery"></a>Добавление Getabstract из коллекции
Чтобы настроить интеграцию Getabstract с Azure AD, необходимо добавить Getabstract из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Getabstract из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **Getabstract**, выберите **Getabstract** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Getabstract в списке результатов](./media/getabstract-tutorial/tutorial_getabstract_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описано, как настроить и проверить единый вход Azure AD в Getabstract с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Getabstract соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Getabstract.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Getabstract.

Чтобы настроить и проверить единый вход Azure AD в Getabstract, выполните следующие стандартные действия:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Getabstract](#create-a-getabstract-test-user)** требуется для того, чтобы в Getabstract существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Getabstract.

**Чтобы настроить единый вход Azure AD в Getabstract, сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **Getabstract** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/getabstract-tutorial/tutorial_getabstract_samlbase.png)

1. Чтобы настроить приложение в режиме, **инициированном поставщиком удостоверений**, в разделе **Домены и URL-адреса приложения Getabstract** сделайте следующее:

    ![Сведения о домене и URL-адресах единого входа для приложения Getabstract](./media/getabstract-tutorial/tutorial_getabstract_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес:

    На этапе подготовки к разработке: `https://int.getabstract.com`.

    На этапе разработки: `https://www.getabstract.com`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес:
    
    На этапе подготовки к разработке: `https://int.getabstract.com/ACS.do`.
    
    На этапе разработки: `https://www.getabstract.com/ACS.do`.

1. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах единого входа для приложения Getabstract](./media/getabstract-tutorial/tutorial_getabstract_url1.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    
    На этапе подготовки к разработке: `https://int.getabstract.com/portal/<org_username>`.
    
    На этапе разработки: `https://www.getabstract.com/portal/<org_username>`.

    > [!NOTE] 
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов Getabstract](https://www.getabstract.com/en/contact).

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/getabstract-tutorial/tutorial_getabstract_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/getabstract-tutorial/tutorial_general_400.png)
    
1. Чтобы настроить единый вход на стороне **Getabstract**, отправьте в [службу поддержки Getabstract](https://www.getabstract.com/en/contact) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/getabstract-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/getabstract-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/getabstract-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/getabstract-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    4.3. Нажмите кнопку **Создать**.
 
### <a name="create-a-getabstract-test-user"></a>Создание тестового пользователя Getabstract

Цель этого раздела — создать пользователя с именем Britta Simon в Getabstract. Приложение Getabstract поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. При попытке получить доступ к приложению Getabstract создается учетная запись пользователя (если она еще не создана).
>[!Note]
>Чтобы создать пользователя вручную, обратитесь к [группе поддержки Getabstract](https://www.getabstract.com/en/contact).


### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Getabstract.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Getabstract, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Getabstract**.

    ![Ссылка на Getabstract в списке приложений](./media/getabstract-tutorial/tutorial_getabstract_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Getabstract на панели доступа, вы автоматически войдете в приложение Getabstract.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/getabstract-tutorial/tutorial_general_01.png
[2]: ./media/getabstract-tutorial/tutorial_general_02.png
[3]: ./media/getabstract-tutorial/tutorial_general_03.png
[4]: ./media/getabstract-tutorial/tutorial_general_04.png

[100]: ./media/getabstract-tutorial/tutorial_general_100.png

[200]: ./media/getabstract-tutorial/tutorial_general_200.png
[201]: ./media/getabstract-tutorial/tutorial_general_201.png
[202]: ./media/getabstract-tutorial/tutorial_general_202.png
[203]: ./media/getabstract-tutorial/tutorial_general_203.png

