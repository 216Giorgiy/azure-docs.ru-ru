---
title: Руководство. Интеграция Azure Active Directory с Halogen Software | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Halogen Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 108667cbaaf52c9c85dffff51c3f9d012f54122b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153561"
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Руководство. Интеграция Azure Active Directory с Halogen Software

В этом руководстве описано, как интегрировать Halogen Software с Azure Active Directory (Azure AD).

Интеграция Halogen Software с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Halogen Software.
- Вы можете включить автоматический вход пользователей в Halogen Software (единый вход) под учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Halogen Software, вам потребуется следующее:

- подписка Azure AD;
- подписка Halogen Software с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Halogen Software из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-halogen-software-from-the-gallery"></a>Добавление Halogen Software из коллекции.

Чтобы настроить интеграцию Halogen Software с Azure AD, вам потребуется добавить Halogen Software из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Halogen Software из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Halogen Software**.

    ![Создание тестового пользователя Azure AD](./media/halogen-software-tutorial/tutorial_halogensoftware_search.png)

1. В области результатов выберите **Halogen Software** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Halogen Software для тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Halogen Software соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователям Azure AD и соответствующим пользователем в Halogen Software.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Halogen Software.

Чтобы настроить и проверить единый вход в Azure AD в Halogen Software, вам потребуется выполнить действия в следующих блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Halogen Software](#creating-a-halogen-software-test-user)** требуется для создания дублирующего Britta Simon пользователя в Halogen Software, связанного с представлением этого пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Halogen Software.

**Чтобы настроить единый вход в Azure AD с помощью Halogen Software, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Halogen Software** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

1. В разделе **Домены и URL-адреса Halogen Software** выполните следующие действия:

    ![Настройка единого входа](./media/halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://global.hgncloud.com/<companyname>`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Halogen Software](https://support.halogensoftware.com/). 
 


1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/halogen-software-tutorial/tutorial_general_400.png)

1. В отдельном окне браузера войдите в приложение **Halogen Software** под учетной записью администратора.

1. Откройте вкладку **Параметры** . 
   
    ![Что такое Azure AD Connect?][12]

1. На панели навигации слева щелкните **Настройка SAML**. 
   
    ![Что такое Azure AD Connect?][13]

1. На странице **Настройка SAML** сделайте следующее: 

    ![Что такое Azure AD Connect?][14]

     a. В качестве значения поля **Уникальный идентификатор** выберите **NameID**.

     b. В качестве значения поля **Unique Identifier Maps To** (Уникальный идентификатор сопоставляется с) выберите **Имя пользователя**.
  
     c. Для отправки скачанного файла метаданных нажмите кнопку **Обзор**, чтобы выбрать файл, а затем щелкните **Отправить файл**.
 
     d. Чтобы проверить конфигурацию, нажмите кнопку **Запустить тест**. 
    
    >[!NOTE]
    >Подождите, пока не появится сообщение "*Проверка SAML завершена. Закройте это окно*". Закройте окно браузера. Флажок **Включить SAML** установлен, только если проверка завершена. 
     
     д. Выберите **Включить SAML**.
    
     Е. Нажмите кнопку **Сохранить изменения**. 

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).


### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/halogen-software-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/halogen-software-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/halogen-software-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/halogen-software-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-halogen-software-test-user"></a>Создание тестового пользователя Halogen Software

Цель этого раздела — создать пользователя с именем Britta Simon в Halogen Software.

**Чтобы создать пользователя с именем Britta Simon в Halogen Software, выполните следующие действия:**

1. Войдите в приложение **Halogen Software** под учетной записью администратора.

1. Щелкните вкладку **Центр пользователей**, затем щелкните **Создать пользователя**.
   
    ![Что такое Azure AD Connect?][300]  

1. На странице диалогового окна **Новый пользователь** выполните следующие действия.
   
    ![Что такое Azure AD Connect?][301]

    a. В текстовом поле **Имя** введите имя, например **Britta**.
    
    b. В текстовом поле **Фамилия** введите фамилию, например **Simon**. 

    c. В текстовом поле **Имя пользователя** введите **Britta Simon**, имя пользователя на портале Azure.

    d. В текстовом поле **Пароль** введите пароль для пользователя Britta.
    
    д. Выберите команду **Сохранить**.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon и предоставить этому пользователю доступ к Halogen Software.

![Назначение пользователя][200] 

**Чтобы назначить Britta Simon в Halogen Software, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Halogen Software**.

    ![Настройка единого входа](./media/halogen-software-tutorial/tutorial_halogensoftware_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Нажав элемент Halogen Software на панели доступа, вы автоматически войдете в приложение Halogen Software.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/halogen-software-tutorial/tutorial_halogen_301.png
