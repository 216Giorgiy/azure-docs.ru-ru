---
title: Руководство. Интеграция Azure Active Directory с Hosted Graphite | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Hosted Graphite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 8aefa0580e6a9a1e446dd4861a5627ea2a4d36ce
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811882"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Руководство. Интеграция Azure Active Directory с Hosted Graphite

В этом руководстве описано, как интегрировать Hosted Graphite с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Hosted Graphite обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Hosted Graphite.
- Вы можете включить автоматический вход пользователей в Hosted Graphite (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Hosted Graphite, вам потребуется:

- подписка Azure AD;
- подписка Hosted Graphite с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Hosted Graphite из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-hosted-graphite-from-the-gallery"></a>Добавление Hosted Graphite из коллекции
Чтобы настроить интеграцию Hosted Graphite с Azure AD, необходимо добавить Hosted Graphite из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Hosted Graphite из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Hosted Graphite**.

    ![Создание тестового пользователя Azure AD](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

1. В области результатов выберите **Hosted Graphite** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в приложение Hosted Graphite для тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Hosted Graphite соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Hosted Graphite.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Hosted Graphite.

Чтобы настроить и проверить единый вход Azure AD в Hosted Graphite, вам потребуется выполнить действия в указанных далее стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Hosted Graphite](#creating-a-hosted-graphite-test-user)** требуется для создания пользователя Britta Simon в Hosted Graphite, связанного с представлением этого пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Hosted Graphite.

**Чтобы настроить единый вход Azure AD в Hosted Graphite, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Hosted Graphite** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

1. Если вы хотите настроить приложение в **режиме, инициированном поставщиком удостоверений**, то в разделе **Домены и URL-адреса Hosted Graphite** сделайте следующее:

    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://www.hostedgraphite.com/metadata/<user id>`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://www.hostedgraphite.com/complete/saml/<user id>`.

1. Если вы хотите настроить приложение в **режиме, инициированном поставщиком услуг**, то в разделе **Домены и URL-адреса Hosted Graphite** сделайте следующее:
   
    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    a. Щелкните параметр **Показать дополнительные параметры URL-адресов**.

    b. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://www.hostedgraphite.com/login/saml/<user id>/`.   

    > [!NOTE] 
    > Обратите внимание, что значения, указанные выше, используются в качестве примера. Необходимо указать фактические значения идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, на стороне приложения перейдите в раздел "Доступ" -> "Настройка SAML" или обратитесь в [службу поддержки Hosted Graphite](mailto:help@hostedgraphite.com).
    >
 
1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_general_400.png)

1. В разделе **Настройка Hosted Graphite** щелкните **Настроить Hosted Graphite**, чтобы открыть окно **Настройка единого входа**. Скопируйте **идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

1. Войдите в клиент Hosted Graphite с правами администратора.

1. На боковой панели выберите **SAML Setup page** (Страница настройки SAML), щелкнув **Access -> SAML Setup** (Доступ -> Настройка SAML).
   
    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

1. Проверьте, что эти URL-адреса соответствуют конфигурации в разделе **Домены и URL-адреса Hosted Graphite** на портале Azure.
   
    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

1. В текстовые поля **Идентификатор сущности или издателя** и **URL-адрес единого входа** вставьте значения **Идентификатора сущности SAML** и **URL-адреса службы единого входа SAML**, скопированные на портале Azure. 
   
    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

1. Для параметра **Default User Role** (Роль пользователя по умолчанию) выберите значение **Read-only** (Только для чтения).
    
    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

1. Откройте в блокноте сертификат в кодировке Base-64, скачанный с портала Azure, скопируйте его в буфер обмена и вставьте в текстовое поле **Сертификат X.509**.
    
    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

1. Нажмите кнопку **Сохранить** .

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/hostedgraphite-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/hostedgraphite-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/hostedgraphite-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/hostedgraphite-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-hosted-graphite-test-user"></a>Создание тестового пользователя Hosted Graphite

Цель этого раздела — создать пользователя с именем Britta Simon в Hosted Graphite. Приложение Hosted Graphite поддерживает JIT-подготовку. Эта функция включена по умолчанию.

В этом разделе никакие действия с вашей стороны не требуются. Пользователь будет создан при попытке получить доступ к Hosted Graphite (если он еще не создан).

>[!NOTE]
>Чтобы создать пользователя вручную, обратитесь в службу поддержки Hosted Graphite, отправив сообщение по адресу <mailto:help@hostedgraphite.com>. 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon, предоставив этому пользователю доступ к Hosted Graphite.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Hosted Graphite, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Hosted Graphite**.

    ![Настройка единого входа](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Hosted Graphite на панели доступа, вы автоматически войдете в приложение Hosted Graphite.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/hostedgraphite-tutorial/tutorial_general_203.png

