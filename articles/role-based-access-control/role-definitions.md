---
title: Общие сведения об определениях ролей в Azure RBAC | Документация Майкрософт
description: Сведения об определениях ролей при управлении доступом на основе ролей (RBAC) и о том, как определить пользовательские роли для точного управления доступом к ресурсам в Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 9bb7808f2b483fe9cd7d22c6df3fe80d4a98f1f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266862"
---
# <a name="understand-role-definitions"></a>Определения ролей

Чтобы составить представление о роли или создать собственную [пользовательскую роль](custom-roles.md), полезно узнать, как определяются роли. В этой статье подробно описаны определения ролей и приведены примеры.

## <a name="role-definition-structure"></a>Структура определения роли

*Определение роли* представляет собой коллекцию разрешений. Иногда оно называется просто *ролью*. В определении роли перечисляются операции, которые можно выполнить, например чтение, запись и удаление. В ней также перечисляются операции, которые нельзя выполнить, или операции, которые относятся к базовым данным. Определение роли имеет следующую структуру:

```
assignableScopes []
description
id
name
permissions []
  actions []
  dataActions []
  notActions []
  notDataActions []
roleName
roleType
type
```

Операции указываются с помощью строк в следующем формате:

- `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

Часть `{action}` строки операции указывает тип операций, которые можно выполнять с ресурсом того или иного типа. Например, в `{action}` могут содержаться следующие подстроки:

| Подстрока действия    | ОПИСАНИЕ         |
| ------------------- | ------------------- |
| `*` | Подстановочный знак предоставляет доступ ко всем операциям, которые соответствуют строке. |
| `read` | Разрешает операции чтения (GET). |
| `write` | Разрешает операции записи (PUT, POST и PATCH). |
| `delete` | Разрешает операции удаления (DELETE). |

Здесь приведено определение роли [Участник](built-in-roles.md#contributor) в формате JSON. Операция подстановочного знака (`*`) в разделе `actions` указывает, что субъект, которому назначена эта роль, может выполнять все действия или, иными словами, осуществлять полное управление. Сюда входят и действия, определяемые в будущем (по мере добавления новых типов ресурсов в Azure). Операции, указанные в разделе `notActions`, вычитаются из раздела `actions`. При использовании роли [Участник](built-in-roles.md#contributor) `notActions` удаляет для этой роли возможность управлять доступом к ресурсам, а также назначать доступ к ресурсам.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-and-data-operations-preview"></a>Операции управления и операции с данными (предварительная версия)

Управление доступом на основе ролей для операций управления указывается в разделах `actions` и `notActions` определения роли. Ниже перечислены некоторые примеры операций управления в Azure:

- управление доступом к учетной записи хранения;
- создание, обновление или удаление контейнера больших двоичных объектов;
- удаление группы ресурсов со всеми ее ресурсами.

Управление доступом не наследуется для данных. Такое разделение предотвращает неограниченный доступ ролей с подстановочными знаками (`*`) к вашим данным. Например, если у пользователя в подписке есть роль [Читатель](built-in-roles.md#reader), он может просматривать учетную запись хранения, но по умолчанию не имеет возможности просматривать базовые данные.

Ранее управление доступом на основе ролей не использовалось для операций с данными. Авторизация для операций с данными отличается в зависимости от поставщиков ресурсов. Та же модель авторизации управления доступом на основе ролей, используемая для операций управления, была расширена для операций с данными (в настоящее время в предварительной версии).

В структуру определения роли для поддержки операций с данными добавлены новые разделы данных. Операции с данными указаны в разделах `dataActions` и `notDataActions`. За счет добавления этих разделов данных, поддерживается разделение управления и данных. Это предотвращает случайный доступ текущих назначений ролей с подстановочными знаками (`*`) к данным. Ниже приведены некоторые операции с данными, которые можно указать в разделах `dataActions` и `notDataActions`:

- Считывание списка больших двоичных объектов в контейнере.
- Запись большого двоичного объекта хранилища в контейнер.
- Удаление сообщения из очереди.

Ниже указано определение роли [Модуль чтения данных больших двоичных объектов хранилища (предварительная версия)](built-in-roles.md#storage-blob-data-reader-preview), которое включает операции в разделах `actions` и `dataActions`. Эта роль позволяет считывать контейнер больших двоичных объектов, а также базовые данные большого двоичного объекта.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Allows for read access to Azure Storage blob containers and data.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/read"
        ],
        "additionalProperties": {},
        "dataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Storage Blob Data Reader (Preview)",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

В разделы `dataActions` и `notDataActions` можно добавить только операции с данными. Поставщики ресурсов определяют операции, которые являются операциями с данными, задавая для свойства `isDataAction` значение `true`. Список операций, где свойство `isDataAction` имеет значение `true`, см. в статье [Операции поставщиков ресурсов Azure Resource Manager](resource-provider-operations.md). В определениях ролей, в которых нет операций с данными, разделы `dataActions` и `notDataActions` не обязательны.

Авторизацию всех вызовов API для операций управления выполняет Azure Resource Manager. Авторизацию вызовов API для операций с данными выполняет поставщик ресурсов или Azure Resource Manager.

### <a name="data-operations-example"></a>Пример операций с данными

Чтобы лучше понять, как работают операции управления и операции с данными, рассмотрим конкретный пример. Алисе назначена роль [Владелец](built-in-roles.md#owner) на уровне подписки. Бобу назначена роль [Участник для данных больших двоичных объектов хранилища (предварительная версия)](built-in-roles.md#storage-blob-data-contributor-preview) на уровне области учетной записи хранения. Этот пример показан на схеме ниже.

![Управление доступом на основе ролей расширено для поддержки операций управления и операций с данными](./media/role-definitions/rbac-management-data.png)

Для роли [Владелец](built-in-roles.md#owner) Алисы и роли [Участник для данных больших двоичных объектов хранилища (предварительная версия)](built-in-roles.md#storage-blob-data-contributor-preview) Боба доступны следующие действия:

Владелец.

&nbsp;&nbsp;&nbsp;&nbsp;Действия<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Участник данных BLOB-объектов хранилища (предварительная версия)

&nbsp;&nbsp;&nbsp;&nbsp;Действия<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;Действия с данными<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Так как на уровне подписки у Алисы есть действие с подстановочным знаком (`*`), ее разрешения наследуются в порядке спадания, чтобы она могла выполнять все действия по управлению. Тем не менее Алиса не может выполнять операции с данными. Например, по умолчанию Алиса не может считывать большие двоичные объекты в контейнере, но она может выполнять чтение, запись и удаление контейнеров.

Разрешения Боба ограничены только `actions` и `dataActions`, указанными в роли [Участник данных BLOB-объектов хранилища (предварительная версия)](built-in-roles.md#storage-blob-data-contributor-preview). На основе роли Боб может выполнять операции управления и операции с данными. Например, Боб может считывать, записывать и удалять контейнеры в указанной учетной записи хранения, а также выполнять те же операции с большими двоичными объектами.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Какие средства поддерживают использование RBAC для операций с данными?

Для просмотра операций с данными и работы с ними необходимо иметь правильные версии средств или пакетов SDK:

| Средство  | Version (версия)  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 или более поздней версии |
| [интерфейс командной строки Azure](/cli/azure/install-azure-cli) | 2.0.30 или более поздней версии |
| [Azure для .NET](/dotnet/azure/) | 2.8.0-preview или более поздней версии |
| [Пакет Azure SDK для Go](/go/azure/azure-sdk-go-install) | 15.0.0 или более поздней версии |
| [Azure для Java](/java/azure/) | 1.9.0 или более поздней версии |
| [Azure для Python](/python/azure) | 0.40.0 или более поздней версии |
| [Пакет Azure SDK для Ruby ](https://rubygems.org/gems/azure_sdk) | 0.17.1 или более поздней версии |

## <a name="actions"></a>actions

Разрешение `actions` указывает операции управления, к которым роль предоставляет доступ. Это коллекция строк операций, которые определяют защищенные действия поставщиков ресурсов Azure. Ниже приведены некоторые примеры операций управления, которые можно использовать в `actions`.

| Строка операции    | ОПИСАНИЕ         |
| ------------------- | ------------------- |
| `*/read` | Предоставляет доступ к операциям чтения для всех типов ресурсов всех поставщиков ресурсов Azure.|
| `Microsoft.Compute/*` | Предоставляет доступ ко всем операциям для всех типов ресурсов в поставщике ресурсов Microsoft.Compute.|
| `Microsoft.Network/*/read` | Предоставляет доступ к операциям чтения для всех типов ресурсов в поставщике ресурсов Microsoft.Network.|
| `Microsoft.Compute/virtualMachines/*` | Предоставляет доступ ко всем операциям виртуальных машин и их дочерних типов ресурсов.|
| `microsoft.web/sites/restart/Action` | Предоставляет доступ к перезапуску веб-приложения.|

## <a name="notactions"></a>notActions

Разрешение `notActions` указывает операции управления, которые исключаются из разрешенных `actions`. Разрешение `notActions` следует использовать, если для определения набора операций, которые нужно разрешить, проще указать операции, которые необходимо исключить. Доступ, предоставляемый роли (набор действующих разрешений), вычисляется путем вычитания операций `notActions` из операций `actions`.

> [!NOTE]
> Если пользователю назначена роль, которая исключает определенную операцию в `notActions`, а также другая роль, которая предоставляет доступ к той же операции, то пользователю будет разрешено выполнять эту операцию. `notActions` не является запрещающим правилом. Это просто удобный способ создания набора допустимых операций путем исключения некоторых операций.
>

## <a name="dataactions-preview"></a>dataActions (предварительная версия)

Разрешение `dataActions` задает операции с данными, которым роль предоставляет доступ к данным в рамках этого объекта. Например, если у пользователя есть доступ на чтение большого двоичного объекта для учетной записи хранения, это позволит считывать большие двоичные объекты в этой учетной записи хранения. Ниже приведены некоторые примеры операций с данными, которые можно использовать в `dataActions`.

| Строка операции    | ОПИСАНИЕ         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Возвращает большой двоичный объект или список больших двоичных объектов. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Возвращает результат записи большого двоичного объекта. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Возвращает сообщение. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Возвращает сообщение или результат записи или удаления сообщения. |

## <a name="notdataactions-preview"></a>notDataActions (предварительная версия)

Разрешение `notDataActions` указывает операции с данными, которые исключаются из разрешенных `dataActions`. Доступ, предоставляемый роли (набор действующих разрешений), вычисляется путем вычитания операций `notDataActions` из операций `dataActions`. Каждый поставщик ресурсов предоставляет соответствующий набор API-интерфейсов для выполнения операций с данными.

> [!NOTE]
> Если пользователю назначена роль, которая исключает определенную операцию с данными в `notDataActions`, а также другая роль, которая предоставляет доступ к той же операции, то пользователю будет разрешено выполнять эту операцию. `notDataActions` не является запрещающим правилом. Это просто удобный способ создания набора допустимых операций с данными путем исключения некоторых операций.
>

## <a name="assignablescopes"></a>assignableScopes

В разделе `assignableScopes` указываются области (группы управления (сейчас в предварительной версии), подписки, группы ресурсов или ресурсы), в которых роль доступна для назначения. Вы можете разрешить использование роли только в тех подписках или группах ресурсов, в которых она действительно нужна. В остальных подписках и группах ресурсов она просто не будет отображаться, чтобы не отвлекать пользователей. Необходимо использовать по крайней мере одну группу управления, подписку, группу ресурсов или один идентификатор ресурса.

Для встроенных ролей в качестве `assignableScopes` задана корневая область (`"/"`). Корневая область указывает, что роль доступна для назначения во всех областях. В собственных пользовательских ролях корневую область использовать нельзя. При попытке это сделать происходит ошибка авторизации.

Примеры допустимых назначаемых областей:

| Сценарий | Пример |
|----------|---------|
| Роль доступна для назначения в одной подписке | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Роль доступна для назначения в двух подписках | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Роль доступна для назначения только в группе сетевых ресурсов | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Роль доступна для назначения во всех областях | `"/"` |

## <a name="assignablescopes-and-custom-roles"></a>assignableScopes и пользовательские роли

Раздел `assignableScopes` для пользовательской роли также определяет, кто может создавать, удалять, изменять или просматривать пользовательскую роль.

| Задача | Операция | ОПИСАНИЕ |
| --- | --- | --- |
| Создание или удаление пользовательской роли | `Microsoft.Authorization/ roleDefinition/write` | Пользователи с разрешением на эту операцию для всех `assignableScopes` пользовательской роли могут создавать (или удалять) пользовательские роли для использования в этих областях. Например, [владельцы](built-in-roles.md#owner) или [администраторы доступа пользователей](built-in-roles.md#user-access-administrator) подписок, групп ресурсов и ресурсов. |
| Изменение настраиваемой роли | `Microsoft.Authorization/ roleDefinition/write` | Пользователи с разрешением на эту операцию для всех `assignableScopes` пользовательской роли могут изменять пользовательские роли в этих областях. Например, [владельцы](built-in-roles.md#owner) или [администраторы доступа пользователей](built-in-roles.md#user-access-administrator) подписок, групп ресурсов и ресурсов. |
| Просмотр пользовательской роли | `Microsoft.Authorization/ roleDefinition/read` | Пользователи с разрешением на эту операцию в определенной области могут просматривать пользовательские роли, которые доступны для назначения в этой области. Все встроенные роли обеспечивают доступность пользовательских ролей для назначения. |

## <a name="role-definition-examples"></a>Примеры определений ролей

В следующем примере показано определение роли [Читатель](built-in-roles.md#reader), отображаемое с помощью Azure CLI:

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Ниже приведен пример пользовательской роли для мониторинга и перезапуска виртуальных машин, отображаемой с помощью Azure PowerShell:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
                  "Microsoft.Storage/*/read",
                  "Microsoft.Network/*/read",
                  "Microsoft.Compute/*/read",
                  "Microsoft.Compute/virtualMachines/start/action",
                  "Microsoft.Compute/virtualMachines/restart/action",
                  "Microsoft.Authorization/*/read",
                  "Microsoft.Resources/subscriptions/resourceGroups/read",
                  "Microsoft.Insights/alertRules/*",
                  "Microsoft.Insights/diagnosticSettings/*",
                  "Microsoft.Support/*"
  ],
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

## <a name="see-also"></a>См. также

* [Встроенные роли](built-in-roles.md)
* [Пользовательские роли](custom-roles.md)
* [Операции поставщиков ресурсов Azure Resource Manager](resource-provider-operations.md)
