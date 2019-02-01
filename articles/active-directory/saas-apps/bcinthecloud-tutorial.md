---
title: Руководство. Интеграция Azure Active Directory с BC in the Cloud | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в BC in the Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 7dc40d2c-6349-40cb-b304-b098bd03a66c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2017
ms.author: jeedes
ms.openlocfilehash: 5cb622c7641a1a0d3764dbb63c2e7301a6faf4e6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149107"
---
# <a name="tutorial-azure-active-directory-integration-with-bc-in-the-cloud"></a>Руководство. Интеграция Azure Active Directory с BC in the Cloud

В этом руководстве описано, как интегрировать BC in the Cloud с Azure Active Directory (Azure AD).

Интеграция Azure AD с BC in the Cloud обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к BC in the Cloud.
- Вы можете включить автоматический вход пользователей в BC in the Cloud (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением BC in the Cloud, вам потребуется:

- подписка Azure AD;
- подписка BC in the Cloud с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление BC in the Cloud из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-bc-in-the-cloud-from-the-gallery"></a>Добавление BC in the Cloud из коллекции
Чтобы настроить интеграцию BC in the Cloud с Azure AD, необходимо добавить BC in the Cloud из коллекции в список управляемых приложений SaaS.

**Чтобы добавить BC in the Cloud из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Добавить**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **BC in the Cloud**.

    ![Создание тестового пользователя Azure AD](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_search.png)

1. На панели результатов выберите **BC in the Cloud** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в BC in the Cloud с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в BC in the Cloud соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в BC in the Cloud.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в BC in the Cloud.

Чтобы настроить и проверить единый вход Azure AD в BC in the Cloud, выполните действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя BC in the Cloud](#creating-a-bc-in-the-cloud-test-user)** нужно для того, чтобы в BC in the Cloud также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении BC in the Cloud.

**Чтобы настроить единый вход Azure AD в BC in the Cloud, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **BC in the Cloud** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_samlbase.png)

1. В разделе **Домены и URL-адреса приложения BC in the Cloud** выполните следующие действия.

    ![Настройка единого входа](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://app.bcinthecloud.com/router/loginSaml/<customerid>`

    b. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://app.bcinthecloud.com`.

    > [!NOTE] 
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Для получения этого значения обратитесь к [группе поддержки BC in the Cloud](https://www.bcinthecloud.com/supportcenter/). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/bcinthecloud-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **BC in the Cloud**, отправьте скачанный **XML-файл метаданных** [группе поддержки BC in the Cloud](https://www.bcinthecloud.com/supportcenter/).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/bcinthecloud-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/bcinthecloud-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/bcinthecloud-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/bcinthecloud-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-bc-in-the-cloud-test-user"></a>Создание тестового пользователя BC in the Cloud

В этом разделе описано, как создать пользователя Britta Simon в приложении BC in the Cloud. Обратитесь к  [группе поддержки BC in the Cloud](https://www.bcinthecloud.com/supportcenter/)  для добавления пользователей в приложение BC in the Cloud. Перед использованием единого входа необходимо создать и активировать пользователей. 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к BC in the Cloud.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в BC in the Cloud, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **BC in the Cloud**.

    ![Настройка единого входа](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

 Щелкнув элемент "BC in the Cloud" на панели доступа, вы автоматически войдете в приложение BC in the Cloud. Дополнительные сведения о панели доступа см. в статье  [Что такое портал MyApps?](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bcinthecloud-tutorial/tutorial_general_01.png
[2]: ./media/bcinthecloud-tutorial/tutorial_general_02.png
[3]: ./media/bcinthecloud-tutorial/tutorial_general_03.png
[4]: ./media/bcinthecloud-tutorial/tutorial_general_04.png

[100]: ./media/bcinthecloud-tutorial/tutorial_general_100.png

[200]: ./media/bcinthecloud-tutorial/tutorial_general_200.png
[201]: ./media/bcinthecloud-tutorial/tutorial_general_201.png
[202]: ./media/bcinthecloud-tutorial/tutorial_general_202.png
[203]: ./media/bcinthecloud-tutorial/tutorial_general_203.png

