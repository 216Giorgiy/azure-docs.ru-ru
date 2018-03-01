---
title: "Удаление назначения доступа к корпоративному приложению для пользователя или группы в Azure Active Directory | Документы Майкрософт"
description: "Узнайте, как удалить назначение доступа к корпоративному приложению для пользователя или группы в Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 084ffcbe473290a8734b1c8b8847b517dac4f6b6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Удаление назначения доступа к корпоративному приложению для пользователя или группы в Azure Active Directory
Вы можете легко удалить назначение доступа к корпоративному приложению для пользователя или группы в Azure Active Directory (Azure AD). Необходимо иметь соответствующие разрешения для управления корпоративным приложением, а также права глобального администратора для доступа к каталогу.

> [!NOTE]
> В приложениях Майкрософт (например, в приложениях Office 365) удалить пользователей в корпоративном приложении можно с помощью PowerShell.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Как удалить назначение доступа к корпоративному приложению для пользователя или группы на портале Azure?
1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора каталога.
2. Выберите **Другие службы**, введите **Azure Active Directory** в текстовое поле, а затем нажмите клавишу **ВВОД**.
3. На странице **Azure Active Directory — *имя_каталога*** (то есть на странице Azure AD для каталога, которым вы управляете) выберите **Корпоративные приложения**.

    ![Открытие колонки "Корпоративные приложения"](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. На странице **Корпоративные приложения** выберите **Все приложения**. Отобразится список приложений, которыми можно управлять.
5. На странице **Корпоративные приложения — Все приложения** выберите приложение.
6. На странице ***имя_приложения*** (то есть на странице с именем выбранного приложения в заголовке) выберите **Пользователи и группы**.

    ![Выбор пользователей или групп](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. На странице ***имя_приложения*** **— User & Group Assignment** (Назначение групп и пользователей) выберите одного из нескольких пользователей или групп, а затем щелкните **Удалить**. При появлении запроса подтвердите свое решение.

    ![Выбор команды "Удалить"](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Как удалить назначение доступа к корпоративному приложению для пользователя или группы с помощью PowerShell?
1. Откройте окно Windows PowerShell с повышенными привилегиями.

    >[!NOTE] 
    > Вам потребуется установить модуль Azure AD (с помощью команды `Install-Module -Name AzureAD`). Если будет предложено установить модуль NuGet или новый модуль PowerShell Azure для Active Directory версии 2, введите Y и нажмите клавишу ВВОД.

2. Выполните команду `Connect-AzureAD` и войдите в систему с помощью учетных данных глобального администратора.
3. Чтобы назначить пользователя и его роль в приложении, используйте следующий скрипт:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ``` 
## <a name="next-steps"></a>Дополнительная информация

- [Просмотр всех моих групп](active-directory-groups-view-azure-portal.md)
- [Назначение корпоративному приложению пользователя или группы](active-directory-coreapps-assign-user-azure-portal.md)
- [Отключение входа пользователя в корпоративное приложение](active-directory-coreapps-disable-app-azure-portal.md)
- [Изменение имени или логотипа корпоративного приложения](active-directory-coreapps-change-app-logo-user-azure-portal.md)
