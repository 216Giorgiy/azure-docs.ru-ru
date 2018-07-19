---
title: Руководство по интеграции Azure Active Directory с Trisotech Digital Enterprise Server | Документация Майкрософт
description: Сведения о том, как настроить единый вход Azure Active Directory в Trisotech Digital Enterprise Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d54d20c-eca1-4fa6-b56a-4c3ed0593db0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: f579f914496427febdf60c3a8d3dc368ef265a9d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045331"
---
# <a name="tutorial-azure-active-directory-integration-with-trisotech-digital-enterprise-server"></a>Руководство по интеграции Azure Active Directory с Trisotech Digital Enterprise Server

В этом руководстве описано, как интегрировать Trisotech Digital Enterprise Server с Azure Active Directory (Azure AD).

Интеграция Trisotech Digital Enterprise Server с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD можно контролировать доступ к Trisotech Digital Enterprise Server.
- Вы можете включить автоматический вход пользователей в Trisotech Digital Enterprise Server (единый вход) с применением учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением Trisotech Digital Enterprise Server, вам потребуется следующее:

- подписка Azure AD;
- подписка Trisotech Digital Enterprise Server с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Trisotech Digital Enterprise Server из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-trisotech-digital-enterprise-server-from-the-gallery"></a>Добавление Trisotech Digital Enterprise Server из коллекции
Чтобы настроить интеграцию Trisotech Digital Enterprise Server с Azure AD, необходимо добавить Trisotech Digital Enterprise Server из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Trisotech Digital Enterprise Server из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **Trisotech Digital Enterprise Server**, выберите **Trisotech Digital Enterprise Server** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Trisotech Digital Enterprise Server в списке результатов](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Trisotech Digital Enterprise Server для тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Trisotech Digital Enterprise Server соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Trisotech Digital Enterprise Server.

Чтобы настроить и проверить единый вход Azure AD в Trisotech Digital Enterprise Server, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Trisotech Digital Enterprise Server](#create-a-trisotech-digital-enterprise-server-test-user)** требуется для того, чтобы в Trisotech Digital Enterprise Server существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Trisotech Digital Enterprise Server.

**Чтобы настроить единый вход Azure AD в Trisotech Digital Enterprise Server, сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **Trisotech Digital Enterprise Server** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_samlbase.png)

3. В разделе **Домены и URL-адреса приложения Trisotech Digital Enterprise Server** выполните следующие действия:

    ![Сведения о домене и URL-адресах приложения Trisotech Digital Enterprise Server](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.trisotech.com`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.trisotech.com`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Обратитесь в [клиентскую службу поддержки Trisotech Digital Enterprise Server](mailto:support@trisotech.com), чтобы получить эти значения.

4. В разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений**. Затем вставьте его в Блокнот. 

    ![Ссылка для скачивания сертификата](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_certificate.png)

5. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_400.png)

6. В другом окне веб-браузера войдите на сайт компании Trisotech Digital Enterprise Server с правами администратора.

7. Щелкните **значок меню**, а затем выберите **Administration** (Администрирование).

    ![Настройка единого входа](./media/trisotechdigitalenterpriseserver-tutorial/user1.png)

8. Выберите пункт **User Provider** (Поставщик пользователей).

    ![Настройка единого входа](./media/trisotechdigitalenterpriseserver-tutorial/user2.png)

9. В разделе **User Provider Configurations** (Конфигурация поставщика пользователей) выполните следующие действия:

    ![Настройка единого входа](./media/trisotechdigitalenterpriseserver-tutorial/user3.png)

    a. В раскрывающемся списке в поле **Authentication Method** (Способ проверки подлинности) выберите **язык разметки заявлений системы безопасности 2 (SAML 2)**.

    b. В текстовое поле **URL-адрес метаданных** вставьте значение **URL-адреса метаданных федерации приложений**, скопированное на портале Azure.

    c. В текстовое поле **Application ID** (Идентификатор приложения) введите URL-адрес в следующем формате: `https://<companyname>.trisotech.com`.

    d. Нажмите кнопку **Сохранить**

    д. Введите доменное имя в текстовое поле **Allowed Domains (empty means everyone)** (Разрешенные домены (если оставить это поле пустым, будут использоваться все домены)). Пользователям, которые соответствуют разрешенным доменам, будут автоматически назначены лицензии.

    f. Нажмите кнопку **Сохранить**

 ### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-trisotech-digital-enterprise-server-test-user"></a>Создание тестового пользователя Trisotech Digital Enterprise Server

Цель этого раздела — создать в приложении Trisotech Digital Enterprise Server пользователя с именем Britta Simon. Приложение Trisotech Digital Enterprise Server поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. При попытке получить доступ к приложению Trisotech Digital Enterprise Server учетная запись пользователя создается автоматически, если она еще не создана.
>[!Note]
>Если необходимо создать пользователя вручную, обратитесь в [службу поддержки Trisotech Digital Enterprise Server](mailto:support@trisotech.com).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к Trisotech Digital Enterprise Server.

![Назначение роли пользователя][200] 

**Чтобы назначить Britta Simon в Trisotech Digital Enterprise Server, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **Trisotech Digital Enterprise Server**.

    ![Ссылка на приложение Trisotech Digital Enterprise Server в списке приложений](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Trisotech Digital Enterprise Server на панели доступа, вы автоматически войдете в приложение Trisotech Digital Enterprise Server.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_01.png
[2]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_02.png
[3]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_03.png
[4]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_04.png

[100]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_100.png

[200]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_200.png
[201]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_201.png
[202]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_202.png
[203]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_203.png

