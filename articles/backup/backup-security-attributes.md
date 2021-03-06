---
title: Общие атрибуты безопасности для службы Azure Backup
description: Контрольный список общих атрибутов безопасности для оценки службы Azure Backup
services: backup
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 93dd5372bffb278894987cd3cc2b8322cbc5e577
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679693"
---
# <a name="common-security-attributes-for-azure-backup"></a>Общие атрибуты безопасности для службы Azure Backup

Средства безопасности интегрированы в каждом аспекте службы Azure. В этой статье описываются наиболее распространенные атрибуты безопасности, встроенных в Azure Backup. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>профилактическая;

| Атрибут безопасности | Да/нет | Примечания |
|---|---|--|
| Шифрование при хранении:<ul><li>Шифрование на стороне сервера</li><li>Шифрование на стороне сервера с использованием управляемых клиентом ключей</li><li>Другие возможности шифрования (например, на стороне клиента, постоянное шифрование и т. д.)</ul>| Yes | Использование шифрования службы хранилища для учетных записей хранения. |
| Шифрование при передаче:<ul><li>Шифрование ExpressRoute</li><li>Шифрование в виртуальной сети</li><li>Шифрование между виртуальными сетями</ul>| Нет  | Использование HTTPS. |
| Обработка ключа шифрования (CMK, BYOK и т. д.)| Нет  |  |
| Шифрование на уровне столбцов (службы данных Azure)| Нет  |  |
| Вызовы API в зашифрованном виде| Yes |  |

## <a name="network-segmentation"></a>Сегментация сети

| Атрибут безопасности | Да/нет | Примечания |
|---|---|--|
| Поддержка конечной точки службы| Нет  |  |
| Поддержка внедрения виртуальной сети| Нет  |  |
| Сетевая изоляция / Поддержка защиты брандмауэром| Yes | Принудительное туннелирование поддерживается для резервного копирования виртуальных машин. Принудительное туннелирование не поддерживается для рабочих нагрузок, выполняющихся на виртуальных машинах. |
| Поддержка принудительного туннелирования | Нет  |  |

## <a name="detection"></a>Обнаружение

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Azure Monitor поддержки (Log analytics, Application insights, и т.д.)| Yes | Log Analytics поддерживается с помощью журналов диагностики. См. в разделе [монитор службы архивации Azure защищенные рабочие нагрузки с помощью Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) Дополнительные сведения. |

## <a name="iam-support"></a>Поддержка IAM

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Управление доступом — аутентификация| Yes | Аутентификация выполняется с помощью Azure Active Directory. |
| Управление доступом — авторизация| Yes | Используются встроенные роли RBAC и роли, созданные клиентом. См. в разделе [раздел контроля доступа для управления точками восстановления службы архивации Azure](/azure/backup/backup-rbac-rs-vault) Дополнительные сведения. |


## <a name="audit-trail"></a>Журнал аудита

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Ведение журнала и аудит плана управления и контроля| Yes | Все действия, активированные клиентом на портале Azure, регистрируются в журналах действий. |
| Ведение журналов и аудит плоскости данных| Нет  | К плоскости данных Azure Backup невозможно обратиться напрямую.  |

## <a name="configuration-management"></a>Управление конфигурацией

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Поддержка конфигурации management (Управление версиями конфигурации и т. д.)| Yes|  |