---
title: Руководство. Интеграция Azure Active Directory c IQNavigator VMS | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в IQNavigator VMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee936d174aa3e221bbeb0823ba1503c7cb64a9d6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2019
ms.locfileid: "56185168"
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a>Руководство по Интеграция Azure Active Directory c IQNavigator VMS

В этом руководстве описано, как интегрировать IQNavigator VMS с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением IQNavigator VMS обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к IQNavigator VMS.
- Вы можете включить автоматический вход пользователей в IQNavigator VMS (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с IQNavigator VMS, вам потребуется:

- подписка Azure AD;
- подписка IQNavigator VMS с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление IQNavigator VMS из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-iqnavigator-vms-from-the-gallery"></a>Добавление IQNavigator VMS из коллекции
Чтобы настроить интеграцию IQNavigator VMS с Azure AD, необходимо добавить IQNavigator VMS из коллекции в список управляемых приложений SaaS.

**Чтобы добавить IQNavigator VMS из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **IQNavigator VMS**.

    ![Создание тестового пользователя Azure AD](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

1. На панели результатов выберите **IQNavigator VMS** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в IQNavigator VMS с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в IQNavigator VMS соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в IQNavigator VMS.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в IQNavigator VMS.

Чтобы настроить и проверить единый вход Azure AD в IQNavigator VMS, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя IQNavigator VMS](#creating-a-iqnavigator-vms-test-user)** требуется для того, чтобы в IQNavigator VMS существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении IQNavigator VMS.

**Чтобы настроить единый вход Azure AD в IQNavigator VMS, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **IQNavigator VMS** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

1. В разделе **Домены и URL-адреса приложения IQNavigator VMS** сделайте следующее.

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес: `iqn.com`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`.

1. Установите флажок **Показывать дополнительные параметры URL-адреса** и выполните следующие действия:

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    В текстовое поле **Состояние ретранслятора** введите URL-адрес в следующем формате: `https://<subdomain>.iqnavigator.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените эти значения фактическими URL-адресом ответа и значением состояния ретранслятора. Чтобы получить эти значения, обратитесь к [группе поддержки IQNavigator VMS](https://www.beeline.com/iqn-product-support/).

1. В разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать  **URL-адрес метаданных федерации приложений**, и вставьте его в Блокнот.
    
    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_metadataurl.png)

1. Приложение IQNavigator ожидает в утверждении идентификатора имени значение уникального идентификатора пользователя. Клиент может сопоставить правильное значение для утверждения идентификатора имени. В данном случае для примера мы сопоставили user.UserPrincipalName. Однако вам нужно сопоставить правильное значение, соответствующее параметрам вашей организации.

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация IQNavigator VMS** щелкните **Настроить IQNavigator VMS**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png)

1. Чтобы настроить единый вход на стороне **IQNavigator VMS**, нужно отправить **URL-адрес метаданных федерации приложений**, **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** в [группу поддержки IQNavigator VMS](https://www.beeline.com/iqn-product-support/). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.

    ![Создание тестового пользователя Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_03.png)

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    б) В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    4.3. Нажмите кнопку **Создать**.

### <a name="creating-a-iqnavigator-vms-test-user"></a>Создание тестового пользователя IQNavigator VMS

Цель этого раздела — создать пользователя с именем Britta Simon в IQNavigator VMS. Обратитесь к [группе поддержки IQNavigator VMS](https://www.beeline.com/iqn-product-support/), чтобы добавить пользователей в учетную запись IQNavigator VMS.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к IQNavigator VMS.

![Назначение пользователя][200]

**Чтобы назначить пользователя Britta Simon в IQNavigator VMS, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201]

1. Из списка приложений выберите **IQNavigator VMS**.

    ![Настройка единого входа](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png)

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "IQNavigator VMS" на панели доступа, вы автоматически войдете в приложение IQNavigator VMS.
См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/iqnavigatorvms-tutorial/tutorial_general_203.png

