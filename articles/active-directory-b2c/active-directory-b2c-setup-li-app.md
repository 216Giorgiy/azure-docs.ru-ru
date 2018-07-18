---
title: Настройка LinkedIn в Azure Active Directory B2C | Документация Майкрософт
description: Регистрация и вход пользователей с помощью учетных записей LinkedIn в приложениях, защищенных с помощью Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7588711bd1c2a02e2e9a100d2ba182f43e7df488
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446103"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: регистрация и вход пользователей с помощью учетных записей LinkedIn
## <a name="create-a-linkedin-application"></a>Создание приложения LinkedIn
Чтобы использовать LinkedIn в качестве поставщика удостоверений в Azure Active Directory (Azure AD) B2C, необходимо создать приложение LinkedIn и задать в нем правильные параметры. Для этого потребуется учетная запись LinkedIn. Если у вас нет этой учетной записи, ее можно получить на сайте [https://www.linkedin.com/](https://www.linkedin.com/).

1. Посетите [сайт разработчиков LinkedIn](https://www.developer.linkedin.com/) и войдите с помощью своих учетных данных LinkedIn.
2. Щелкните **Мои приложения** в верхней строке меню и нажмите кнопку **Создать приложение**.
   
    ![LinkedIn — создание приложения](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. В форме **Создание нового приложения** укажите соответствующие сведения (**Название компании**, **Имя**, **Описание**, **URL-адрес логотипа приложения**, **Применение приложения**, **URL-адрес веб-сайта**, **Адрес электронной почты компании** и **Рабочий телефон**).
4. Примите **условия использования LinkedIn API** и нажмите кнопку **Отправить**.
   
    ![LinkedIn — регистрация приложения](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Скопируйте значения **Идентификатор клиента** и **Секрет клиента**. Их можно найти в разделе **Ключи аутентификации**. Оба значения необходимы для настройки LinkedIn в качестве поставщика удостоверений для вашего клиента.
   
   > [!NOTE]
   > **Секрет клиента** — это важные учетные данные безопасности.
   > 
   > 
6. Введите `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` в поле **Разрешённые URL-адреса перенаправления** (в разделе **OAuth 2.0**). Замените **{tenant}** именем своего клиента (например, contoso.onmicrosoft.com). Нажмите кнопку **Добавить**, затем нажмите кнопку **Обновить**. Значение **{клиент}** должно быть в нижнем регистре.
   
    ![LinkedIn — настройка приложения](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Настройка LinkedIn в качестве поставщика удостоверений для клиента
1. Выполните следующие действия, чтобы [перейти к колонке функций B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) на портале Azure.
2. В колонке функций B2C щелкните **Поставщики удостоверений**.
3. Нажмите **+Добавить** в верхней части колонки.
4. Укажите понятное **имя** конфигурации поставщика удостоверений. Например, введите "LI".
5. Нажмите **Тип поставщика удостоверений**, выберите **LinkedIn** и нажмите кнопку **ОК**.
6. Нажмите **Настроить этот поставщик удостоверений** и введите идентификатор клиента и секрет клиента ранее созданного приложения LinkedIn.
7. Нажмите кнопку **ОК**, а затем — **Создать**, чтобы сохранить конфигурацию LinkedIn.

