---
title: "Руководство по интеграции Azure Active Directory c Palo Alto Networks (GlobalProtect) | Документация Майкрософт"
description: "Узнайте, как настроить единый вход Azure Active Directory в Palo Alto Networks (GlobalProtect)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 03bef6f2-3ea2-4eaa-a828-79c5f1346ce5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: 88322bc34183d6ee0b68cab7c59379e0f65ebecf
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2017
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---globalprotect"></a>Руководство по интеграции Azure Active Directory c Palo Alto Networks (GlobalProtect)

В этом руководстве описано, как интегрировать Palo Alto Networks (GlobalProtect) с Azure Active Directory (Azure AD).

Интеграция Azure AD с Palo Alto Networks (GlobalProtect) обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать предоставление доступа к Palo Alto Networks (GlobalProtect).
- Вы можете включить автоматический вход пользователей в Palo Alto Networks (GlobalProtect) (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Palo Alto Networks (GlobalProtect), вам потребуется:

- подписка Azure AD;
- подписка Palo Alto Networks (GlobalProtect) с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление Palo Alto Networks (GlobalProtect) из коллекции.
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-palo-alto-networks---globalprotect-from-the-gallery"></a>Добавление Palo Alto Networks (GlobalProtect) из коллекции
Чтобы настроить интеграцию Palo Alto Networks (GlobalProtect) с Azure AD, необходимо добавить Palo Alto Networks (GlobalProtect) из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Palo Alto Networks (GlobalProtect) из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **Palo Alto Networks (GlobalProtect)** и выберите **Palo Alto Networks (GlobalProtect)** на панели результатов, а затем нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Palo Alto Networks (GlobalProtect) в списке результатов](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Palo Alto Networks (GlobalProtect) с использованием тестового пользователя Britta Simon.

Чтобы активировать единый вход, Azure AD необходима информация о том, какой пользователь в Palo Alto Networks (GlobalProtect) соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем Palo Alto Networks (GlobalProtect).

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения поля **имени пользователя** в Palo Alto Networks (GlobalProtect).

Чтобы настроить и проверить единый вход Azure AD в Palo Alto Networks (GlobalProtect), вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Palo Alto Networks (GlobalProtect)](#create-a-palo-alto-networks---globalprotect-test-user)** требуется, чтобы в Palo Alto Networks (GlobalProtect) существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в Palo Alto Networks (GlobalProtect).

**Чтобы настроить единый вход Azure AD с Palo Alto Networks (GlobalProtect), сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **Palo Alto Networks (GlobalProtect)** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_samlbase.png)

3. В разделе **Домены и URL-адреса приложения Palo Alto Networks (GlobalProtect)** сделайте следующее:

    ![Сведения о домене и URL-адресах единого входа для Palo Alto Networks (GlobalProtect)](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_url.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<Customer Firewall URL>`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<Customer Firewall URL>/SAML20/SP`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Для получения этих значений обратитесь в [службу поддержки клиентов Palo Alto Networks (GlobalProtect)](https://support.paloaltonetworks.com/support). 
 
4. Приложение Palo Alto Networks (GlobalProtect) ожидает утверждения SAML в определенном формате. Настройте следующие утверждения для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На следующем снимке экрана приведен пример.
    
    ![Настройка единого входа](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_attribute.png)
    
5. В разделе **Атрибуты пользователя** диалогового окна **Единый вход** настройте атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия:
    
    | Имя атрибута | Значение атрибута |
    | --- | --- |    
    | Имя пользователя | user.userprincipalname |

    а. Щелкните **Добавить атрибут**, чтобы открыть диалоговое окно **Добавление атрибута**.

    ![Настройка единого входа](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_attribute_04.png)

    ![Настройка единого входа](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_attribute_05.png)
    
    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.
    
    c. В списке **Значение** выберите значение атрибута, отображаемое для этой строки. Мы сопоставили значение со значением user.userprincipalname в качестве примера, но вы можете сопоставить его со своим соответствующим значением. 
    
    d. Нажмите кнопку **ОК**.


6. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_certificate.png) 

7. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_400.png)

8. В другом окне браузера откройте сайт Palo Alto с правами администратора.

9. Щелкните **Device** (Устройство).

    ![Настройка единого входа в Palo Alto](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin1.png)

10. Чтобы импортировать файл метаданных, в левой области навигации выберите **SAML Identity Provider** (Поставщик удостоверений SAML) и нажмите кнопку Import (Импорт).

    ![Настройка единого входа в Palo Alto](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin2.png)

11. Сделайте следующее в окне Import (Импорт).

    ![Настройка единого входа в Palo Alto](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin3.png)

    а. Укажите имя в текстовом поле **Profile Name** (Имя профиля), например Azure AD Global Protect.
    
    b. В разделе **Identity Provider Metadata** (Метаданные поставщика удостоверений) щелкните **Browse** (Обзор) и выберите файл metadata.xml, загруженный ранее с портала Azure.
    
    c. Щелкните **ОК**

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/active-directory-saas-paloaltoglobalprotect-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/active-directory-saas-paloaltoglobalprotect-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/active-directory-saas-paloaltoglobalprotect-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/active-directory-saas-paloaltoglobalprotect-tutorial/create_aaduser_04.png)

    а. В поле **Имя** введите **BrittaSimon**.

    b. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    г) Щелкните **Создать**.
 
### <a name="create-a-palo-alto-networks---globalprotect-test-user"></a>Создание тестового пользователя в Palo Alto Networks (GlobalProtect)

Palo Alto Networks (GlobalProtect) поддерживает JIT-подготовку пользователей, поэтому после успешной проверки подлинности в системе будет автоматически создан пользователь, если он еще не существует. В этом разделе не нужно выполнять никаких действий. 

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Palo Alto Networks (GlobalProtect), чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon для приложения Palo Alto Networks (GlobalProtect), сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. Выберите **Palo Alto Networks (GlobalProtect)** из списка приложений.

    ![Ссылка на Palo Alto Networks (GlobalProtect) в списке приложений](./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "Palo Alto Networks (GlobalProtect)" на панели доступа, вы автоматически войдете в приложение Palo Alto Networks (GlobalProtect).
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoglobalprotect-tutorial/tutorial_general_203.png

