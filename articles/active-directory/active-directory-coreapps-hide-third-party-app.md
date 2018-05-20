---
title: Скрытие приложения в пользовательском интерфейсе Azure Active Directory | Документация Майкрософт
description: Как скрыть приложение от пользователей на панелях доступа Azure Active Directory или в средствах запуска Office 365.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 564b724ab3971e0566fb2b1dc3a75c2eeac3d391
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Скрытие приложения в пользовательском интерфейсе Azure Active Directory

Если у вас есть приложение, которое не требуется отображать на панелях доступа пользователей и в средствах запуска Office 365, плитку приложения можно скрыть.  В средстве запуска приложений пользователя можно скрыть приложения двумя следующими способами.

- Скрытие стороннего приложения на панелях доступа пользователей и в средствах запуска приложений Office 365.
- Скрытие всех приложений Office 365 на панелях доступа пользователей.

Когда вы скроете приложение, у пользователей останутся разрешения для этого приложения, но оно не будет отображаться в их средствах запуска. Необходимо иметь соответствующие разрешения для управления корпоративным приложением, а также права глобального администратора для доступа к каталогу.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Скрытие приложения в интерфейсе пользователя
Используйте описанные ниже шаги в зависимости от конкретной ситуации, чтобы скрыть приложения на панели доступа.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Как скрыть стороннее приложение на панели доступа пользователя и в средствах запуска приложений O365?
Чтобы скрыть приложение на панели доступа пользователя и в средствах запуска приложений Office 365, выполните описанные ниже действия.

1.  Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора каталога.
2.  Выберите **Все службы**, в текстовом поле введите **Azure Active Directory**, а затем нажмите клавишу **ВВОД**.
3.  На экране **Azure Active Directory — *имя_каталога*** (то есть на экране Azure AD для каталога, которым вы управляете) выберите **Корпоративные приложения**.
![Корпоративные приложения](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  На экране **Корпоративные приложения** выберите **Все приложения**. Отобразится список приложений, которыми можно управлять.
5.  На экране **Корпоративные приложения — Все приложения** выберите приложение.</br>
![Корпоративные приложения](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  На экране ***имя_приложения*** (то есть на экране с именем выбранного приложения в заголовке) выберите "Свойства".
7.  На экране ***имя_приложения* — Свойства** для вопроса **Видно пользователям?** выберите ответ **Да**.
![Корпоративные приложения](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Щелкните **Сохранить** .

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Как скрыть приложения Office 365 на панели доступа пользователя?

Чтобы скрыть все приложения Office 365 на панели доступа, выполните следующие действия. Эти приложения по-прежнему будут отображаться на портале Office 365.

1.  Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора каталога.
2.  Выберите **Все службы**, в текстовом поле введите **Azure Active Directory**, а затем нажмите клавишу **ВВОД**.
3.  На экране**Azure Active Directory - *имя_каталога*** (то есть на экране Azure AD для каталога, которым вы управляете) выберите **Параметры пользователей**.
4.  На экране **Параметры пользователей** в разделе **Корпоративные приложения** выберите **Да** для параметра **Users can only see Office 365 apps in the Office 365 portal** (Пользователи могут видеть приложения Office 365 только на портале Office 365).

![Приложения Enterprise](media/active-directory-coreapps-hide-third-party-app/apps4.png)

## <a name="next-steps"></a>Дополнительная информация
* [Просмотр всех моих групп](active-directory-groups-view-azure-portal.md)
* [Назначение корпоративному приложению пользователя или группы](active-directory-coreapps-assign-user-azure-portal.md)
* [Удаление назначения пользователя или группы из корпоративного приложения](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Изменение имени или логотипа корпоративного приложения](active-directory-coreapps-change-app-logo-user-azure-portal.md)

