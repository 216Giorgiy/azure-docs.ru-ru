---
title: Руководство. Интеграция Azure Active Directory с Trakopolis | Документация Майкрософт
description: Узнайте, как настроить единый вход для Azure Active Directory и Trakopolis.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 73d67c3e-4b4b-4d3b-aa58-6699ea1ccea3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1cd7bf90483bca2b2e6eb6185b48e02a1cf65bc
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190421"
---
# <a name="tutorial-azure-active-directory-integration-with-trakopolis"></a>Руководство. Интеграция Azure Active Directory с Trakopolis

В этом учебнике описано, как интегрировать приложение Trakopolis с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Trakopolis обеспечивает следующие преимущества.

- Вы можете контролировать доступ к Trakopolis с помощью Azure AD.
- Вы можете включить автоматический вход пользователей в Trakopolis с учетной записью Azure AD (единый вход).
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Trakopolis, вам потребуется:

- подписка Azure AD;
- подписка Trakopolis с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Trakopolis из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-trakopolis-from-the-gallery"></a>Добавление Trakopolis из коллекции
Чтобы настроить интеграцию Trakopolis с Azure AD, необходимо добавить Trakopolis из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Trakopolis из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Trakopolis**.

    ![Создание тестового пользователя Azure AD](./media/trakopolis-tutorial/tutorial_trakopolis_search.png)

1. На панели результатов выберите **Trakopolis** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/trakopolis-tutorial/tutorial_trakopolis_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Trakopolis с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в Trakopolis соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Trakopolis.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Trakopolis.

Чтобы настроить и проверить единый вход Azure AD в Trakopolis, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Trakopolis](#creating-a-trakopolis-test-user)** требуется для того, чтобы в Trakopolis существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Trakopolis.

**Чтобы настроить единый вход Azure AD в Trakopolis, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Trakopolis** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/trakopolis-tutorial/tutorial_trakopolis_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Trakopolis** сделайте следующее.

    ![Настройка единого входа](./media/trakopolis-tutorial/tutorial_trakopolis_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<company name>.trakopolis.com/`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<company name>.trakopolis.com`

    > [!NOTE] 
    > Эти значения приведены для примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов Trakopolis](mailto:support@cantelematics.com). 

1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/trakopolis-tutorial/tutorial_trakopolis_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/trakopolis-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация Trakopolis** щелкните **Настроить Trakopolis**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/trakopolis-tutorial/tutorial_trakopolis_configure.png) 

1. Чтобы настроить единый вход на стороне **Trakopolis**, нужно отправить скачанный **XML-файл метаданных, URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** [группе поддержки Trakopolis](mailto:support@cantelematics.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/trakopolis-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/trakopolis-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/trakopolis-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/trakopolis-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    б) В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    4.3. Нажмите кнопку **Создать**.
 
### <a name="creating-a-trakopolis-test-user"></a>Создание тестового пользователя Trakopolis

В этом разделе описано, как создать пользователя Britta Simon в Trakopolis. Чтобы добавить пользователей на платформу Trakopolis, обратитесь к [группе поддержки Trakopolis](mailto:support@cantelematics.com). Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Trakopolis.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Trakopolis, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Trakopolis**.

    ![Настройка единого входа](./media/trakopolis-tutorial/tutorial_trakopolis_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  
Щелкнув элемент Trakopolis на панели доступа, вы автоматически войдете в приложение Trakopolis.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/trakopolis-tutorial/tutorial_general_01.png
[2]: ./media/trakopolis-tutorial/tutorial_general_02.png
[3]: ./media/trakopolis-tutorial/tutorial_general_03.png
[4]: ./media/trakopolis-tutorial/tutorial_general_04.png

[100]: ./media/trakopolis-tutorial/tutorial_general_100.png

[200]: ./media/trakopolis-tutorial/tutorial_general_200.png
[201]: ./media/trakopolis-tutorial/tutorial_general_201.png
[202]: ./media/trakopolis-tutorial/tutorial_general_202.png
[203]: ./media/trakopolis-tutorial/tutorial_general_203.png

