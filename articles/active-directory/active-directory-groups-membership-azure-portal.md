---
title: "Управление группами, к которым относится ваша группа, в Azure Active Directory | Документы Майкрософт"
description: "В Azure Active Directory группы могут содержать другие группы. Вот как можно управлять членством такого типа."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 44911ec2c278f7af7b8ec4bd971bc97b342a8bf6
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Управление тем, к каким группам относится группа в клиенте Azure Active Directory
В Azure Active Directory группы могут содержать другие группы. Вот как можно управлять членством такого типа.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Как узнать, членом каких групп является моя группа?
1. Войдите в [центр администрирования Azure AD](https://aad.portal.azure.com) с помощью учетной записи глобального администратора каталога.
2. Выберите **Пользователи и группы**.

   ![Открытие образа пользователей и групп](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Выберите **Все группы**.

   ![Выбор образа групп](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Выберите группу.
2. Выберите **Членства в группах**.

   ![Открытие образа членства в группах](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Чтобы добавить группу в качестве участника в другую группу, в колонке **Группа — Членства в группах** выберите команду **Добавить**.
2. Выберите группу в колонке **Выбор группы** и нажмите кнопку **Выбрать**, расположенную в нижней части колонки. Одним действием группу можно добавить только в одну другую группу. В поле **Пользователь** можно ввести часть имени пользователя или устройства, чтобы отфильтровать по нему список отображенных элементов. Подстановочные знаки в поле не допускаются.

   ![Добавление членства в группе](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Чтобы удалить группу из другой группы, участником которой она является, в колонке **Группа — Принадлежность к группам** выберите удаляемую группу.
9. Выберите команду **Удалить** и подтвердите свой выбор при появлении соответствующего запроса.

   ![Команда "Удаление членства"](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Завершив изменение членства группы в других группах, щелкните **Сохранить**.

## <a name="additional-information"></a>Дополнительная информация
В следующих статьях содержатся дополнительные сведения об Azure Active Directory.

* [Просмотр существующих групп](active-directory-groups-view-azure-portal.md)
* [Создание группы и добавление участников](active-directory-groups-create-azure-portal.md)
* [Управление параметрами группы](active-directory-groups-settings-azure-portal.md)
* [Управление участниками группы](active-directory-groups-members-azure-portal.md)
* [Управление динамическими правилами для пользователей в группе](active-directory-groups-dynamic-membership-azure-portal.md)
