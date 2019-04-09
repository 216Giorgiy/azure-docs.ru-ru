---
title: Руководство по Настройка Tableau Online для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической подготовки и отзыва учетных записей пользователей в Tableau Online.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: f732eebd410a6b52a21a46925a29bf4676f7c8cb
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057495"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Руководство по Настройка Tableau Online для автоматической подготовки пользователей

В этом руководстве описаны шаги, которые необходимо выполнить в Tableau Online и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической подготовки и отзыва пользователей и групп в Tableau Online.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Технические условия

Сценарий, описанный в этом руководстве, предполагает, что у вас уже имеется:

*   клиент Azure AD;
*   [клиент Tableau Online](https://www.tableau.com/);
*   учетная запись пользователя Tableau Online с разрешениями администратора.

> [!NOTE]
> Интеграция подготовки Azure AD зависит от [Rest API Tableau Online](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm), доступного для разработчиков Tableau Online.

## <a name="adding-tableau-online-from-the-gallery"></a>Добавление Tableau Online из коллекции.
Перед настройкой Tableau Online для автоматической подготовки пользователей в Azure AD необходимо добавить Tableau Online из коллекции приложений Azure AD в список управляемых приложений SaaS.

**Чтобы добавить Tableau Online из коллекции приложений Azure AD, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Tableau Online**выберите **Tableau Online** на панели результатов и нажмите **добавить** кнопку, чтобы добавить это приложение.

    ![Tableau Online в списке результатов](common/search-new-app.png)

## <a name="assigning-users-to-tableau-online"></a>Назначение пользователей в приложении Tableau Online

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки синхронизированы будут только те пользователи и группы, которые были "назначены" приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей необходимо решить, какие пользователи или группы в Azure AD будут иметь доступ к Tableau Online. После этого пользователей и (или) группы можно будет назначить приложению Tableau Online, следуя приведенным ниже указаниям.

*   [Назначение пользователя или группы корпоративному приложению](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Важные советы по назначению пользователей в приложении Tableau Online

*   Рекомендуется назначить одного пользователя Azure AD в Tableau Online для тестирования конфигурации автоматической подготовки пользователей. Дополнительные пользователи и/или группы можно назначить позднее.

*   При назначении пользователя в Tableau Online в диалоговом окне назначения необходимо выбрать действительную роль для конкретного приложения (если доступно). Пользователи с ролью **Доступ по умолчанию** исключаются из подготовки.

## <a name="configuring-automatic-user-provisioning-to-tableau-online"></a>Настройка автоматической подготовки пользователей в Tableau Online

В этом разделе описано, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и групп в Tableau Online на основе их назначений в Azure AD.

> [!TIP]
> Для Tableau Online также можно включить единый вход на основе SAML. Для этого следуйте инструкциям, приведенным в [руководстве по единому входу в Tableau Online](tableauonline-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Чтобы настроить автоматическую подготовку пользователей в Tableau Online, сделайте следующее.

1. Войдите в [портала Azure](https://portal.azure.com) и выберите **корпоративные приложения**выберите **все приложения**, а затем выберите **Tableau Online**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Tableau Online**.

    ![Ссылка на Tableau Online в списке приложений](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. В разделе **Учетные данные администратора** введите **домен**, **имя пользователя-администратора**, **пароль администратора** и **URL-адрес содержимого** учетной записи Tableau Online.

   * В поле **Домен** укажите поддомен на основе шага 6.

   * В поле **Имя администратора** укажите имя учетной записи администратора в клиенте Clarizen. Пример: admin@contoso.com.

   * В поле **Пароль администратора** введите пароль учетной записи, соответствующей имени пользователя-администратора.

   * В поле **URL-адрес содержимого** укажите поддомен на основе шага 6.

6. После входа в учетную запись администратора Tableau Online значения для **домена** и **URL-адреса содержимого** можно извлечь из URL-адреса страницы администратора.

    * Скопировать **домен** для учетной записи Tableau Online можно из этой части URL-адреса:

        ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * Скопировать **URL-адрес содержимого** для учетной записи Tableau Online можно из этого раздела и во время ее настройки определить значение. В этом примере используется значение "contoso":

        ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > Ваш **домен** может отличаться от приведенного здесь.

7. После заполнения полей, указанных на шаге 5, щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к Tableau Online. Если установить подключение не удалось, убедитесь, что у учетной записи Tableau Online есть разрешения администратора, и повторите попытку.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Send an email notification when a failure occurs** (Отправить уведомление по электронной почте при сбое).

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. Выберите команду **Сохранить**.

10. В разделе **Сопоставления** выберите **Синхронизировать пользователей Azure Active Directory с Tableau Online**.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. В разделе **Сопоставление атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в Tableau Online. Атрибуты, выбранные как свойства **Matching**, используются для сопоставления учетных записей пользователей в Tableau Online для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. В разделе **Сопоставления** выберите **Синхронизировать группы Azure Active Directory с Tableau Online**.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. В разделе **Сопоставление атрибутов** просмотрите атрибуты группы, которые синхронизированы из Azure AD в Tableau Online. Атрибуты, выбранные как свойства **Matching**, используются для сопоставления учетных записей пользователей в Tableau Online для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Чтобы включить службу подготовки Azure AD для Tableau Online, измените значение параметра **Состояние подготовки** на **Включено** в разделе **Параметры**.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Определите пользователей или группы для подготовки в Tableau Online, выбрав нужные значения в поле **Область** раздела **Параметры**.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Подготовка Tableau Online](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

После этого начнется начальная синхронизация пользователей и (или) групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов о подготовке, в которых зафиксированы все действия, выполняемые службой подготовки Azure AD с приложением Tableau Online.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Узнайте, как просматривать журналы и получать отчеты о действиях по подготовке](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
