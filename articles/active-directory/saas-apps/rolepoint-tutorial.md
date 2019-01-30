---
title: Руководство. Интеграция Azure Active Directory с RolePoint | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и RolePoint.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: d16e2ebf6f2a170a65d9e471f0c4a19a3a31ded2
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827845"
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a>Руководство. Интеграция Azure Active Directory с RolePoint

В этом руководстве описано, как интегрировать RolePoint с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением RolePoint дает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к RolePoint.
- Вы можете включить автоматический вход пользователей в RolePoint (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением RolePoint, вам потребуется:

- подписка Azure AD;
- подписка RolePoint с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление RolePoint из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-rolepoint-from-the-gallery"></a>Добавление RolePoint из коллекции
Чтобы настроить интеграцию RolePoint с Azure AD, необходимо добавить RolePoint из коллекции в список управляемых приложений SaaS.

**Чтобы добавить RolePoint из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **RolePoint**.

    ![Создание тестового пользователя Azure AD](./media/rolepoint-tutorial/tutorial_rolepoint_search.png)

1. На панели результатов выберите **RolePoint** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/rolepoint-tutorial/tutorial_rolepoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе мы выполним настройку и проверку единого входа Azure AD в RolePoint с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в RolePoint соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в RolePoint.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в RolePoint.

Чтобы настроить и проверить единый вход Azure AD в RolePoint, выполните следующие стандартные действия.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя RolePoint](#creating-a-rolepoint-test-user)** — требуется для создания в RolePoint копии пользователя Britta Simon, связанной с соответствующим представлением пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении RolePoint.

**Чтобы настроить единый вход Azure AD в RolePoint, сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **RolePoint** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/rolepoint-tutorial/tutorial_rolepoint_samlbase.png)

1. В разделе **Домены и URL-адреса приложения RolePoint** сделайте следующее:

    ![Настройка единого входа](./media/rolepoint-tutorial/tutorial_rolepoint_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<subdomain>.rolepoint.com/login`
    
    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://app.rolepoint.com/<instancename>`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Мы рекомендуем использовать уникальное значение строки в идентификаторе. Чтобы получить это значение, обратитесь в [службу поддержки RolePoint](mailto:info@rolepoint.com). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/rolepoint-tutorial/tutorial_rolepoint_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/rolepoint-tutorial/tutorial_general_400.png)


1. Чтобы настроить единый вход на стороне **RolePoint**, отправьте скачанный **XML-файл метаданных** в [службу поддержки RolePoint](mailto:info@rolepoint.com).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/rolepoint-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/rolepoint-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/rolepoint-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/rolepoint-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-rolepoint-test-user"></a>Создание тестового пользователя RolePoint

В этом разделе описано, как создать пользователя Britta Simon в приложении RolePoint. Обратитесь к [службу поддержки RolePoint](mailto:info@rolepoint.com), чтобы добавить пользователей на платформу RolePoint.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к RolePoint.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в RolePoint, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **RolePoint**.

    ![Настройка единого входа](./media/rolepoint-tutorial/tutorial_rolepoint_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент RolePoint на панели доступа, вы автоматически войдете в приложение RolePoint. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rolepoint-tutorial/tutorial_general_01.png
[2]: ./media/rolepoint-tutorial/tutorial_general_02.png
[3]: ./media/rolepoint-tutorial/tutorial_general_03.png
[4]: ./media/rolepoint-tutorial/tutorial_general_04.png

[100]: ./media/rolepoint-tutorial/tutorial_general_100.png

[200]: ./media/rolepoint-tutorial/tutorial_general_200.png
[201]: ./media/rolepoint-tutorial/tutorial_general_201.png
[202]: ./media/rolepoint-tutorial/tutorial_general_202.png
[203]: ./media/rolepoint-tutorial/tutorial_general_203.png

