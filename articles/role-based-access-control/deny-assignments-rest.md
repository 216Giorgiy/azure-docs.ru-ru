---
title: Вывод списка запретов назначений для ресурсов Azure с помощью REST API — Azure | Документация Майкрософт
description: Сведения о создании списка запретов назначений для пользователей, групп и приложений с помощью управления доступом на основе ролей (RBAC) для ресурсов Azure и REST API.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 29b8e0953109238b724cc8df9f456706f71a041e
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341629"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>Вывод списка запретов назначений для ресурсов Azure с помощью REST API

Сейчас запреты назначений доступны **только для чтения** и настраиваются только в Azure. Несмотря на то что вы не можете создать свой собственный запрет назначений, можно составить список запретов назначений, так как они могут повлиять на эффективные разрешения. В этой статье описывается, как создать список запретов назначений с помощью RBAC и REST API.

## <a name="list-a-single-deny-assignment"></a>Вывод списка определенного запрета назначения

1. Можете начать со следующего запроса:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. Внутри URI замените *{scope}* областью, для которой требуется создать список запретов назначений.

    | Область | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Подписка |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ресурс |

1. Замените *{deny-assignment-id}* идентификатором запрета назначения, которое необходимо получить.

## <a name="list-multiple-deny-assignments"></a>Вывод списка нескольких запретов назначений

1. Начните работу с любого из следующих запросов.

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    С использованием необязательных параметров:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Внутри URI замените *{scope}* областью, для которой требуется создать список запретов назначений.

    | Область | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Подписка |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ресурс |

1. Замените *{filter}* условием, по которому требуется отфильтровать список запретов назначений.

    | Фильтр | ОПИСАНИЕ |
    | --- | --- |
    | (без фильтра) | Вывод списка всех запретов назначений, находящихся выше и ниже заданной области. |
    | `$filter=atScope()` | Вывод списка запретов назначений только в указанной области и областях выше нее. Не включаются запреты назначений во внутренних областях. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Вывод списка всех запретов назначений с указанным названием. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Вывод списка запретов назначений в корневой области (/)

1. Повысьте уровень доступа, как описано в статье [Повышение прав доступа глобального администратора в Azure Active Directory](elevate-access-global-admin.md).

1. Используйте следующий запрос:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Замените *{filter}* условием, по которому требуется отфильтровать список запретов назначений. Необходимо указать фильтр.

    | Фильтр | ОПИСАНИЕ |
    | --- | --- |
    | `$filter=atScope()` | Вывод списка запретов назначений только в корневой области. Не включаются запреты назначений во внутренних областях. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Вывод списка всех запретов назначений с указанным названием. |

1. Удалите повышенный уровень доступа.

## <a name="next-steps"></a>Дополнительная информация

- [Запрет назначений для ресурсов Azure](deny-assignments.md)
- [Повышение прав доступа глобального администратора в Azure Active Directory](elevate-access-global-admin.md)
- [Справочник по REST API Azure](/rest/api/azure/)
