---
title: Руководство по Интеграция Azure Active Directory с InsideView | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении InsideView.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 845ab85ae10a9d57bf3e263d49532675f60cd84f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517850"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Руководство по Интеграция Azure Active Directory с InsideView

В этом руководстве описано, как интегрировать InsideView с Azure Active Directory (Azure AD).

Интеграция InsideView с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к InsideView.
- Вы можете включить автоматический вход пользователей в InsideView (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с InsideView, вам нужно:

- подписка Azure AD;
- InsideView единого входа — подписка с поддержкой

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление InsideView из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-insideview-from-the-gallery"></a>Добавление InsideView из коллекции
Чтобы настроить интеграцию InsideView с Azure AD, нужно добавить InsideView из коллекции в список управляемых приложений SaaS.

**Чтобы добавить InsideView из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **InsideView**.

    ![Создание тестового пользователя Azure AD](./media/insideview-tutorial/tutorial_insideview_search.png)

1. На панели результатов выберите **InsideView** и выберите **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в InsideView с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в InsideView соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в InsideView.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в InsideView.

Чтобы настроить и проверить единый вход Azure AD в InsideView, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя InsideView](#creating-an-insideview-test-user)**  — требуется для создания пользователя Britta Simon в InsideView, связанного с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении InsideView.

**Чтобы настроить единый вход Azure AD в InsideView, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **InsideView** выберите **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/insideview-tutorial/tutorial_insideview_samlbase.png)

1. В разделе **Домены и URL-адреса приложения InsideView** выполните следующие действия:

    ![Настройка единого входа](./media/insideview-tutorial/tutorial_insideview_url.png)
    
    В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://my.insideview.com/iv/<STS Name>/login.iv`.

    > [!NOTE] 
    > Это значение приведено для справки. Вместо него нужно указать фактический URL-адрес ответа. Обратитесь к [поддержки insideview](mailto:support@insideview.com) для получения этого значения.
 
1. В разделе **Сертификат подписи SAML** щелкните **Certificate (Raw)** (Сертификат (необработанный)), а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/insideview-tutorial/tutorial_insideview_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/insideview-tutorial/tutorial_general_400.png)

1. В разделе **Настройка InsideView** выберите **Настроить InsideView**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/insideview-tutorial/tutorial_insideview_configure.png) 

1. В другом окне веб-браузера войдите на свой корпоративный веб-сайт InsideView в качестве администратора.

1. На панели инструментов в верхней части экрана щелкните **Admin** (Администрирование), **SingleSignOn Settings** (Параметры единого входа) и выберите **Add SAML** (Добавить SAML).
   
   ![Параметры единого входа SAML](./media/insideview-tutorial/ic794135.png "Параметры единого входа SAML")

1. В разделе **Добавить SAML** сделайте следующее:

    ![Добавление SAML](./media/insideview-tutorial/ic794136.png "Добавление SAML")
   
    a. В текстовом поле **Имя службы токенов безопасности** введите имя конфигурации.

    2. В текстовое поле  **SamlP/WS-Fed Unsolicited EndPoint** (Незапрашиваемая конечная точка WS-Fed/SamlP) вставьте значение **URL-адреса службы единого входа SAML**, скопированное на портале Azure.
    
    c. Откройте сертификат в кодировке Base-64, скачанный на портале Azure, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **STS Certificate** (Сертификат STS).

    d. В **Crm User Id Mapping** (Сопоставление идентификаторов пользователей Crm) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    д. В **Crm Email Mapping** (Сопоставление адресов электронной почты CRM) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    Е. В **Crm First Name Mapping** (Сопоставление имен CRM) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    ж. В **Crm lastName Mapping** (Сопоставление фамилий CRM) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. Выберите команду **Сохранить**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/insideview-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/insideview-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/insideview-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/insideview-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-an-insideview-test-user"></a>Создание тестового пользователя InsideView

Чтобы пользователи Azure AD могли выполнять вход в InsideView, им нужна определенная информация об InsideView. В случае с InsideView подготовка выполняется вручную.

Чтобы создать контакты и пользователей в InsideView, обратитесь в [службу поддержки InsideView](mailto:support@insideview.com).

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетных записей пользователя InsideView или API, предоставляемые InsideView для подготовки учетных записей пользователя AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к InsideView, чтобы он мог использовать единый вход Azure.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в InsideView, выполните следующие указания:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **InsideView**.

    ![Настройка единого входа](./media/insideview-tutorial/tutorial_insideview_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент InsideView на панели доступа, вы автоматически войдете в приложение InsideView.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/insideview-tutorial/tutorial_general_01.png
[2]: ./media/insideview-tutorial/tutorial_general_02.png
[3]: ./media/insideview-tutorial/tutorial_general_03.png
[4]: ./media/insideview-tutorial/tutorial_general_04.png

[100]: ./media/insideview-tutorial/tutorial_general_100.png

[200]: ./media/insideview-tutorial/tutorial_general_200.png
[201]: ./media/insideview-tutorial/tutorial_general_201.png
[202]: ./media/insideview-tutorial/tutorial_general_202.png
[203]: ./media/insideview-tutorial/tutorial_general_203.png
