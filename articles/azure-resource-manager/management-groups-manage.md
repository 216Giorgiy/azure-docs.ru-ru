---
title: Изменение, удаление групп управления и управление ими в Azure | Документация Майкрософт
description: Узнайте, как сохранять и обновлять иерархию групп управления.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: rithorn
ms.openlocfilehash: 0a13627232904f4b14cdb5cbf5c3ca927d9ea167
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754410"
---
# <a name="manage-your-resources-with-management-groups"></a>Управление ресурсами с помощью групп управления 
Группы управления — это контейнеры, которые помогают управлять доступом, политикой и соответствием требованиям в нескольких подписках. Вы можете создавать и удалять такие контейнеры и управлять ими, чтобы создать иерархии, которые можно использовать с [политикой Azure](../azure-policy/azure-policy-introduction.md) и [элементами управления доступом на основе ролей Azure](../role-based-access-control/overview.md). Дополнительные сведения о группах управления см. в статье [Упорядочение ресурсов с помощью групп управления Azure](management-groups-overview.md).

Функция групп управления предоставляется в общедоступной предварительной версии. Чтобы начать использование групп управления, войдите на [портал Azure](https://portal.azure.com) или воспользуйтесь [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available) либо [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) для управления группами управления.

Чтобы внести изменения в группу управления, необходимо иметь роль владельца или участника в этой группе. Чтобы получить сведения об имеющихся разрешениях, выберите группу управления, а затем выберите **IAM**. Дополнительные сведения о ролях RBAC см. в статье [Начало работы с управлением доступом на основе ролей на портале Azure](../role-based-access-control/overview.md).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Изменение имени группы управления 
Имя группы управления можно изменить с помощью портала, PowerShell или Azure CLI.

### <a name="change-the-name-in-the-portal"></a>Изменение имени на портале

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** > **Группы управления**.  
3. Выберите группу управления, которую требуется переименовать. 
4. Выберите параметр **Переименовать группу** в верхней части страницы. 

    ![Переименование группы](media/management-groups/detail_action_small.png)
5. Когда откроется меню, введите новое имя.

    ![Переименование группы](media/management-groups/rename_context.png) 
4. Щелкните **Сохранить**. 

### <a name="change-the-name-in-powershell"></a>Изменение имени в PowerShell

Чтобы обновить отображаемое имя, воспользуйтесь командой **Update-AzureRmManagementGroup**. Например, чтобы изменить имя группы управления с "ИТ-отдел Contoso" на "Группа Contoso", выполните следующую команду: 

```azurepowershell-inetractive
C:\> Update-AzureRmManagementGroup -GroupName ContosoIt -DisplayName "Contoso Group"  
``` 

### <a name="change-the-name-in-azure-cli"></a>Изменение имени в Azure CLI

Для Azure CLI используется команда обновления. 

```azurecli-interactive
az account management-group update --name Contoso --display-name "Contoso Group" 
```

---
 
## <a name="delete-a-management-group"></a>Удаление группы управления
Чтобы удалить группу управления, необходимо выполнить следующие требования:
1. В группе управления нет дочерних групп управления или подписок. 
    - Сведения о том, как переместить подписку из группы управления, см. в [этом разделе](#Move-subscriptions-in-the-hierarchy). 
    - Сведения о перемещении группы управления в другую группу управления см. в [этом разделе](#Move-management-groups-in-the-hierarchy). 
2. В вашей роли владельца или участника в группе управления есть разрешения на запись. Чтобы получить сведения об имеющихся разрешениях, выберите группу управления, а затем выберите **IAM**. Дополнительные сведения о ролях RBAC см. в статье [Начало работы с управлением доступом на основе ролей на портале Azure](../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Удаление на портале

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** > **Группы управления**.  
3. Выберите группу управления, которую требуется удалить. 
    
    ![Удаление группы](media/management-groups/delete.png)
4. Нажмите кнопку **Удалить**. 
    - Если значок неактивен, наведите указатель мыши на значок, чтобы узнать причину. 
5. Откроется окно, в котором нужно подтвердить удаление группы управления. 

    ![Удаление группы](media/management-groups/delete_confirm.png) 
6. Выберите **Да**. 


### <a name="delete-in-powershell"></a>Удаление в PowerShell

В PowerShell для удаления группы управления используется команда **Remove-AzureRmManagementGroup**. 

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName Contoso
```

### <a name="delete-in-azure-cli"></a>Удаление в Azure CLI
В Azure CLI используется команда az account management-group delete. 

```azurecli-interactive
az account management-group delete --name Contoso
```
---

## <a name="view-management-groups"></a>Просмотр групп управления
Вы можете просматривать все группы управления, в которых у вас есть прямые или наследуемые роли RBAC.  

### <a name="view-in-the-portal"></a>Просмотр на портале
1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** > **Группы управления**. 
3. Загружается страница иерархии групп управления, где можно изучить все группы управления и подписки, к которым у вас есть доступ. При выборе имени группы открывается нижний уровень в иерархии. Навигация работает так же, как и в обозревателе файлов. 
    ![Основная страница](media/management-groups/main.png)
4. Чтобы просмотреть сведения о группе управления, перейдите по ссылке **(подробности)** рядом с заголовком группы управления. Если ссылка недоступна, у вас нет разрешения на просмотр этой группы управления.  

### <a name="view-in-powershell"></a>Просмотр в PowerShell
Для получения всех групп используется команда Get-AzureRmManagementGroup.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```
Для получения сведений об одной группе управления используйте параметр -GroupName.

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName Contoso
```

### <a name="view-in-azure-cli"></a>Просмотр в Azure CLI
Для извлечения всех групп используется команда вывода списка.  

```azurecli-interactive
az account management-group list
```
Для получения сведений об одной группе управления используется команда отображения.

```azurecli-interactive
az account management-group show --name Contoso
```
---

## <a name="move-subscriptions-in-the-hierarchy"></a>Перемещение подписок в иерархии
Одна из причин для создания группы управления — объединение подписок. Дочерним элементом группы управления могут быть только подписки и группы управления. При перемещении в группу управления подписка наследует все права доступа и политики пользователя из родительской группы управления. 

Чтобы перенести подписку, необходимо иметь определенные разрешения: 
- Роль "Владелец" в дочерней подписке.
- Роль "Владелец" или "Участник" в новой родительской группе управления. 
- Роль "Владелец" или "Участник" в старой родительской группе управления.
Чтобы получить сведения об имеющихся разрешениях, выберите группу управления, а затем выберите **IAM**. Дополнительные сведения о ролях RBAC см. в статье [Начало работы с управлением доступом на основе ролей на портале Azure](../role-based-access-control/overview.md). 

### <a name="move-subscriptions-in-the-portal"></a>Перемещение подписок на портале

**Добавление имеющейся подписки в группу управления**
1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** > **Группы управления**. 
3. Выберите группу управления, которую вы планируете сделать родительской.      
5. В верхней части страницы выберите пункт **Добавить существующий**. 
6. В открывшемся меню выберите **Тип ресурса** для элемента, который вы пытаетесь переместить. В этом примере это значение **Подписка**.
7. Выберите подписку в списке с правильным идентификатором. 

    ![Дочерние элементы](media/management-groups/add_context_2.png)
8. Щелкните "Сохранить".

**Удаление подписки из группы управления**
1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** > **Группы управления**. 
3. Выберите группу управления, которая является родительской.  
4. Выберите многоточие в конце строки подписки в списке, которую требуется переместить.

    ![Move](media/management-groups/move_small.png)
5. Выберите **Переместить**.
6. В открывшемся меню выберите **родительскую группу управления**.

    ![Move](media/management-groups/move_small_context.png)
7. Нажмите кнопку **Сохранить**.

### <a name="move-subscriptions-in-powershell"></a>Перемещение подписок в PowerShell
Чтобы переместить подписку в PowerShell, используйте команду Add-AzureRmManagementGroupSubscription.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

Чтобы удалить связь между подпиской и группой управления, используйте команду Remove-AzureRmManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

### <a name="move-subscriptions-in-azure-cli"></a>Перемещение подписок в Azure CLI
Чтобы переместить подписку в CLI, используйте команду добавления. 

```azurecli-interactive
az account management-group subscription add --name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

Чтобы удалить подписку из группы управления, используйте команду удаления.  

```azurecli-interactive
az account management-group subscription remove --name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

---

## <a name="move-management-groups-in-the-hierarchy"></a>Перемещение групп управления в иерархии  
При перемещении родительской группы управления вместе с ней перемещаются все дочерние ресурсы, включая группы управления, подписки, группы ресурсов и ресурсы.   

### <a name="move-management-groups-in-the-portal"></a>Перемещение групп управления на портале

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **Все службы** > **Группы управления**. 
3. Выберите группу управления, которую вы планируете сделать родительской.      
5. В верхней части страницы выберите пункт **Добавить существующий**.
6. В открывшемся меню выберите **Тип ресурса** для элемента, который вы пытаетесь переместить. В этом примере это значение **Группа управления**.
7. Выберите группу управления с правильным идентификатором и именем.

    ![Перемещение](media/management-groups/add_context.png)
8. Нажмите кнопку **Сохранить**.

### <a name="move-management-groups-in-powershell"></a>Перемещение групп управления в PowerShell
Чтобы переместить группу управления в другой группе, используйте команду Update-AzureRmManagementGroup в PowerShell.  

```powershell
Update-AzureRmManagementGroup -GroupName Contoso  -ParentName ContosoIT
```  
### <a name="move-management-groups-in-azure-cli"></a>Перемещение групп управления в Azure CLI
Чтобы переместить группу управления с помощью Azure CLI, используйте команду обновления. 

```azurecli-interactive
az account management-group update --name Contoso --parent "Contoso Tenant" 
``` 

---

## <a name="next-steps"></a>Дополнительная информация 
Дополнительные сведения о группах управления: 
- [Упорядочение ресурсов с помощью групп управления Azure](management-groups-overview.md)
- [Создание групп управления для организации ресурсов Azure](management-groups-create.md)
- [Страница для установки модуля Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Просмотр спецификации REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Установка расширения Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
