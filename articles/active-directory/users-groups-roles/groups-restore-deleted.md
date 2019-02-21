---
title: Восстановление удаленной группы Office 365 в Azure Active Directory | Документация Майкрософт
description: Описывает, как в Azure Active Directory восстановить удаленную группу, просмотреть доступные для восстановления группы или удалить группу без возможности восстановления
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d2e756676ee1abde88f75a1629640239f3162ea
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430647"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Восстановление удаленной группы Office 365 в Azure Active Directory

Когда вы удаляете группу Office 365 в Azure Active Directory (Azure AD), эта группа в течение 30 дней с момента удаления сохраняется, но не отображается. Это сделано для того, чтобы группу и ее содержимое можно было при необходимости восстановить. Эта функция действует в Azure AD исключительно для групп Office 365. Она недоступна для групп безопасности или групп рассылки.

> [!NOTE]
> Не используйте `Remove-MsolGroup`, иначе группа будет удалена без возможности восстановления. Для удаления группы O365 всегда используйте `Remove-AzureADMSGroup`.

Для восстановления группы нужны разрешения любого из следующих видов.

Роль | Разрешения
--------- | ---------
Администратор компании, партнер с поддержкой 2 уровня; администратор службы InTune | Восстановление любой удаленной группы Office 365
Администратор учетных записей, партнер с поддержкой 1 уровня | Восстановление любой удаленной группы Office 365, кроме тех, которым назначена роль администратора компании
Пользователь | Восстановление любой удаленной группы Office 365, которая принадлежала им


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a>Просмотр удаленных групп Office 365, которые можно восстановить

Следующие командлеты можно использовать для просмотра удаленных групп, чтобы убедиться, что нужные группы еще не удалены окончательно. Эти командлеты являются частью [модуля Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/). Дополнительные сведения об этом модуле можно найти в статье [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) (Модуль PowerShell версии 2 для Azure Active Directory).

1.  Запустите следующий командлет, чтобы отобразить в клиенте все удаленные группы Office 365, которые доступны для восстановления.
   
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Если вам известен идентификатор объекта для конкретной группы (его можно получить из командлета, описанного в шаге 1), вы можете с выполнить следующий командлет, чтобы убедиться, что эта группа еще не удалена без возможности восстановления.
  
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```

## <a name="how-to-restore-your-deleted-office-365-group"></a>Как восстановить удаленную группу Office 365
Когда вы убедитесь, что группа еще доступна для восстановления, вы можете восстановить эту удаленную группу, выполнив одно из следующих действий. Если группа содержит документы, узлы SP или другие постоянные объекты, полное восстановление группы и ее содержимого может занять до 24 часов.

1.  Запустите следующий командлет, чтобы восстановить группу и ее содержимое.
 
 ```
 Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
 ``` 

Кроме того, следующий командлет позволяет окончательно удалить группу без возможности восстановления.
 ```
 Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
 ```

## <a name="how-do-you-know-this-worked"></a>Как убедиться, что действие выполнено?

Чтобы убедиться, что группа Office 365 успешно восстановлена, запустите командлет `Get-AzureADGroup –ObjectId <objectId>` для отображения сведений о группе. После выполнения запроса на восстановление происходят следующие изменения.

- Группа появится в Exchange на панели навигации слева.
- В планировщике отобразится план для этой группы.
- Станут доступны все сайты SharePoint и их содержимое.
- Группа станет доступна из любой конечной точки Exchange и из других рабочих нагрузок Office 365, которые поддерживают группы Office 365.

## <a name="next-steps"></a>Дополнительная информация
В следующих статьях содержатся дополнительные сведения о группах Azure Active Directory.

* [Просмотр существующих групп](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Управление параметрами группы](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Управление участниками группы](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Управление членством в группе](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Управление динамическими правилами для пользователей в группе](groups-dynamic-membership.md)
