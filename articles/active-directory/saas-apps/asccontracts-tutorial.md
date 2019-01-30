---
title: Руководство. Интеграция Azure Active Directory с ASC Contracts | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении ASC Contracts.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f7f54202-1581-4e55-a97e-02633ff9382d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: jeedes
ms.openlocfilehash: 5737bd76e7809035f21103f45a20fa8306061f30
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823306"
---
# <a name="tutorial-azure-active-directory-integration-with-asc-contracts"></a>Руководство. Интеграция Azure Active Directory с ASC Contracts

В этом руководстве описано, как интегрировать ASC Contracts с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением ASC Contracts обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к ASC Contracts.
- Вы можете включить автоматический вход пользователей в ASC Contracts (единый вход) с использованием учетных записей Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с ASC Contracts, вам потребуется:

- подписка Azure AD;
- подписка ASC Contracts с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление ASC Contracts из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-asc-contracts-from-the-gallery"></a>Добавление ASC Contracts из коллекции
Чтобы настроить интеграцию ASC Contracts с Azure AD, необходимо добавить ASC Contracts из коллекции в список управляемых приложений SaaS.

**Чтобы добавить ASC Contracts из коллекции, выполните следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **ASC Contracts**.

    ![Создание тестового пользователя Azure AD](./media/asccontracts-tutorial/tutorial_asccontracts_search.png)

1. На панели результатов выберите **ASC Contracts** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/asccontracts-tutorial/tutorial_asccontracts_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в ASC Contracts с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в ASC Contracts соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в ASC Contracts.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в ASC Contracts.

Чтобы настроить и проверить единый вход Azure AD в ASC Contracts, требуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя ASC Contracts](#creating-an-asc-contracts-test-user)** требуется для создания в ASC Contracts пользователя Britta Simon, связанного с соответствующим пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на новом портале Azure и настроить его в приложении ASC Contracts.

**Чтобы настроить единый вход Azure AD в ASC Contracts, выполните следующее.**

1. На портале Azure на странице интеграции с приложением **ASC Contracts** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/asccontracts-tutorial/tutorial_asccontracts_samlbase.png)

1. В разделе **Домены и URL-адреса приложения ASC Contracts** сделайте следующее.

    ![Настройка единого входа](./media/asccontracts-tutorial/tutorial_asccontracts_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<subdomain>.asccontracts.com/shibboleth`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<subdomain>.asccontracts.com/shibboleth.sso/login`.

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Обратитесь к группе поддержки ASC Networks Inc. (ASC) по номеру **(613) 599-61-78**, чтобы получить эти значения.

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/asccontracts-tutorial/tutorial_asccontracts_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/asccontracts-tutorial/tutorial_general_400.png)

1. Для настройки единого входа на стороне **ASC Contracts** обратитесь в службу поддержки ASC Networks Inc. (ASC) по номеру **(613) 599-61-78** и предоставьте скачанный **XML-файл метаданных**. Это позволит службе поддержки правильно настроить подключение единого входа SAML для приложения на обоих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/asccontracts-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/asccontracts-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/asccontracts-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/asccontracts-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-an-asc-contracts-test-user"></a>Создание тестового пользователя ASC Contracts

Обратитесь в службу поддержки ASC Networks Inc. (ASC) по номеру **(613) 599-61-78**, чтобы добавить пользователей на платформу ASC Contracts.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к ASC Contracts.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению ASC Contracts, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **ASC Contracts**.

    ![Настройка единого входа](./media/asccontracts-tutorial/tutorial_asccontracts_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент ASC Contracts на панели доступа, вы автоматически войдете в приложение ASC Contracts. Дополнительные сведения о панели доступа см. в статье [Что такое портал MyApps?](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/asccontracts-tutorial/tutorial_general_01.png
[2]: ./media/asccontracts-tutorial/tutorial_general_02.png
[3]: ./media/asccontracts-tutorial/tutorial_general_03.png
[4]: ./media/asccontracts-tutorial/tutorial_general_04.png

[100]: ./media/asccontracts-tutorial/tutorial_general_100.png

[200]: ./media/asccontracts-tutorial/tutorial_general_200.png
[201]: ./media/asccontracts-tutorial/tutorial_general_201.png
[202]: ./media/asccontracts-tutorial/tutorial_general_202.png
[203]: ./media/asccontracts-tutorial/tutorial_general_203.png
