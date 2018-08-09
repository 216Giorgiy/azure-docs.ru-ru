---
title: Руководство по интеграции Azure Active Directory с SciQuest Spend Director | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и SciQuest Spend Director.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 0ddc8a42f4e0454061fa645b8c5d465e9e8dd9bf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445175"
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Учебник. Интеграция Azure Active Directory с SciQuest Spend Director

В этом руководстве описано, как интегрировать SciQuest Spend Director с Azure Active Directory (Azure AD).

Интеграция SciQuest Spend Director с Azure AD обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к SciQuest Spend Director.
- Вы можете включить автоматический вход пользователей в SciQuest Spend Director (единый вход) под учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с SciQuest Spend Director, вам потребуется следующее:

- подписка Azure AD;
- подписка SciQuest Spend Director с включенным единым входом.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление SciQuest Spend Director из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Добавление SciQuest Spend Director из коллекции
Чтобы настроить интеграцию SciQuest Spend Director с Azure AD, вам потребуется добавить SciQuest Spend Director из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SciQuest Spend Director из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **SciQuest Spend Director**, выберите **SciQuest Spend Director** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![SciQuest Spend Director в списке результатов](./media/sciquest-spend-director-tutorial/tutorial_sciquestspenddirector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описано, как настроить и проверить единый вход Azure AD в SciQuest Spend Director с использованием тестового пользователя Britta Simon.

При настройке единого входа Azure AD необходимо знать, какой пользователь в SciQuest Spend Director соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователям Azure AD и соответствующим пользователем в SciQuest Spend Director.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в SciQuest Spend Director.

Чтобы настроить и проверить единый вход в Azure AD в SciQuest Spend Director, вам потребуется выполнить действия в следующих блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя SciQuest Spend Director](#create-a-sciquest-spend-director-test-user)** требуется для того, чтобы в SciQuest Spend Director существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении SciQuest Spend Director.

**Чтобы настроить единый вход в Azure AD с помощью SciQuest Spend Director, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **SciQuest Spend Director** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/sciquest-spend-director-tutorial/tutorial_sciquestspenddirector_samlbase.png)

1. В разделе **Домены и URL-адреса приложения SciQuest Spend Director** выполните следующие действия:

    ![Сведения о домене и URL-адресах единого входа приложения SciQuest Spend Director](./media/sciquest-spend-director-tutorial/tutorial_sciquestspenddirector_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.sciquest.com/apps/Router/SAMLAuth/<instancename>`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.sciquest.com`

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<companyname>.sciquest.com/apps/Router/ExternalAuth/Login/<instancename>`.

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Укажите вместо них фактические значения URL-адреса для входа, идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов SciQuest Spend Director](https://www.jaggaer.com/contact-us/). 

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/sciquest-spend-director-tutorial/tutorial_sciquestspenddirector_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/sciquest-spend-director-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **SciQuest Spend Director**, отправьте скачанный **XML-файл метаданных** в [службу поддержки SciQuest Spend Director](https://www.jaggaer.com/contact-us/).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/sciquest-spend-director-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/sciquest-spend-director-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/sciquest-spend-director-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/sciquest-spend-director-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-sciquest-spend-director-test-user"></a>Создание тестового пользователя SciQuest Spend Director

Цель этого раздела — создать пользователя с именем Britta Simon в SciQuest Spend Director.

Для этого обратитесь в [службу поддержки SciQuest Spend Director](https://www.jaggaer.com/contact-us/) и предоставьте им сведения о тестовой учетной записи.

Вы также можете использовать JIT-подготовку — функцию единого входа, поддерживаемую SciQuest Spend Director.  
Если функция JIT-подготовки включена, SciQuest Spend Director автоматически создает пользователей при попытке единого входа, если они еще не созданы. Эта функция устраняет необходимость вручную создавать дублирующих пользователей для единого входа.

Чтобы включить JIT-подготовку, обратитесь в [службу поддержки SciQuest Spend Director](https://www.jaggaer.com/contact-us/).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к SciQuest Spend Director, чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить Britta Simon в SciQuest Spend Director, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **SciQuest Spend Director**.

    ![Ссылка SciQuest Spend Director в списке приложений](./media/sciquest-spend-director-tutorial/tutorial_sciquestspenddirector_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Нажав элемент SciQuest Spend Director на панели доступа, вы автоматически войдете в приложение SciQuest Spend Director.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/sciquest-spend-director-tutorial/tutorial_general_04.png

[100]: ./media/sciquest-spend-director-tutorial/tutorial_general_100.png

[200]: ./media/sciquest-spend-director-tutorial/tutorial_general_200.png
[201]: ./media/sciquest-spend-director-tutorial/tutorial_general_201.png
[202]: ./media/sciquest-spend-director-tutorial/tutorial_general_202.png
[203]: ./media/sciquest-spend-director-tutorial/tutorial_general_203.png

