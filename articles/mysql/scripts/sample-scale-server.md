---
title: Скрипт Azure CLI. Масштабирование сервера службы "База данных Azure для MySQL"
description: Этот пример скрипта CLI масштабирует сервер базы данных Azure для MySQL до нужного уровня производительности после выполнения запроса к метрикам.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 04/05/2018
ms.openlocfilehash: 38ae923d86965e493501e8df64f7a064929c3cfb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422660"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Мониторинг и масштабирование сервера базы данных Azure для MySQL с помощью Azure CLI
Этот пример скрипта CLI масштабирует отдельный сервер базы данных Azure для MySQL до нужного уровня производительности после выполнения запроса к метрикам.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Если вы решили выполнять CLI локально, для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Проверьте версию, выполнив команду `az --version`. Дополнительные сведения об установке или обновлении версии Azure CLI 2.0 см. в [этой]( /cli/azure/install-azure-cli) статье. 

## <a name="sample-script"></a>Пример скрипта
В этом примере скрипта отредактируйте выделенные строки, чтобы изменить имя пользователя и пароль администратора на собственные. Замените идентификатор подписки, используемый в командах `az monitor`, собственным.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=18-19 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Очистка развертывания
После выполнения примера скрипта можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Описание скрипта
Этот скрипт использует команды, описанные в следующей таблице:

| **Команда** | **Примечания** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Создает сервер MySQL, на котором размещены базы данных. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Выводит список значений метрики для ресурсов. |
| [az group delete](/cli/azure/group#az-group-delete) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация
- Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).
- Попробуйте использовать другие скрипты на основе [примеров Azure CLI для базы данных Azure для MySQL](../sample-scripts-azure-cli.md).
- Дополнительные сведения о масштабировании см. в статьях об [уровнях служб](../concepts-service-tiers.md) и [единицах вычислений и единицах хранения](../concepts-compute-unit-and-storage.md).
