---
title: Руководство. Интеграция Azure Active Directory со Skilljar | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Skilljar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c572f556-98a3-48e6-8e4c-e634b7a2ba70
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 76490f5c4470da95f85bddbde1a2abb70f15d126
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817059"
---
# <a name="tutorial-azure-active-directory-integration-with-skilljar"></a>Руководство. Интеграция Azure Active Directory со Skilljar

В этом руководстве описано, как интегрировать Skilljar с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Skilljar обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Skilljar.
- Вы можете включить автоматический вход пользователей в Skilljar (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Skilljar, вам потребуется:

- подписка Azure AD;
- подписка Skilljar с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Skilljar из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-skilljar-from-the-gallery"></a>Добавление Skilljar из коллекции
Чтобы настроить интеграцию Skilljar с Azure AD, необходимо добавить Skilljar из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Skilljar из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Skilljar**.

    ![Создание тестового пользователя Azure AD](./media/skilljar-tutorial/tutorial_skilljar_search.png)

1. На панели результатов выберите **Skilljar** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/skilljar-tutorial/tutorial_skilljar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Skilljar с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в Skilljar соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Skilljar.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Skilljar.

Чтобы настроить и проверить единый вход Azure AD в Skilljar, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Skilljar](#creating-a-skilljar-test-user)** требуется для того, чтобы в Skilljar существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Skilljar.

**Чтобы настроить единый вход Azure AD в Skilljar, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Skilljar** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/skilljar-tutorial/tutorial_skilljar_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Skilljar** выполните следующие действия.

    ![Настройка единого входа](./media/skilljar-tutorial/tutorial_skilljar_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.skilljar.com/`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.skilljar.com/`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов Skilljar](https://support.skilljar.com/hc/). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/skilljar-tutorial/tutorial_skilljar_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/skilljar-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **Skilljar**, необходимо отправить скачанный **XML-файл метаданных** и значение **Формат идентификатора имени** (urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress) [группе поддержки Skilljar](https://support.skilljar.com/hc/). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/skilljar-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/skilljar-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/skilljar-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/skilljar-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-skilljar-test-user"></a>Создание тестового пользователя Skilljar

Цель этого раздела — создать пользователя с именем Britta Simon в Skilljar. Приложение Skilljar поддерживает JIT-подготовку. Эта функция включена по умолчанию.

В этом разделе никакие действия с вашей стороны не требуются. Пользователь будет создан при попытке получить доступ к Skilljar (если он еще не создан). 

>[!NOTE]
>Если вам нужно вручную создать пользователя, необходимо обратиться к [группе поддержки Skilljar](https://support.skilljar.com/hc/). 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Skilljar.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению Skilljar, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Skilljar**.

    ![Настройка единого входа](./media/skilljar-tutorial/tutorial_skilljar_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  

Щелкнув плитку Skilljar на панели доступа, вы автоматически войдете в приложение Skilljar.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skilljar-tutorial/tutorial_general_01.png
[2]: ./media/skilljar-tutorial/tutorial_general_02.png
[3]: ./media/skilljar-tutorial/tutorial_general_03.png
[4]: ./media/skilljar-tutorial/tutorial_general_04.png

[100]: ./media/skilljar-tutorial/tutorial_general_100.png

[200]: ./media/skilljar-tutorial/tutorial_general_200.png
[201]: ./media/skilljar-tutorial/tutorial_general_201.png
[202]: ./media/skilljar-tutorial/tutorial_general_202.png
[203]: ./media/skilljar-tutorial/tutorial_general_203.png

