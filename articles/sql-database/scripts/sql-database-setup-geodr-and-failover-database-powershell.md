---
title: Пример для PowerShell. Активная георепликация отдельной базы данных SQL Azure | Документация Майкрософт
description: Пример скрипта Azure PowerShell для настройки активной георепликации автономной базы данных в Базе данных SQL Azure и отработки отказа этой базы данных.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: f24caa8e82d8e3f9a7109945d55fad97564bdb76
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59359335"
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-single-database-in-azure-sql-database"></a>Настройка активной георепликации для отдельной базы данных в Базе данных SQL Azure с помощью PowerShell

Этот пример скрипта PowerShell настраивает активную георепликацию для отдельной базы данных и выполняет для нее отработку отказа во вторичную реплику базы данных.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Чтобы установить и использовать PowerShell локально, для работы с этим руководством вам понадобится AZ PowerShell 1.4.0 или более поздней версии. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

## <a name="sample-scripts"></a>Примеры сценариев

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-single-database.ps1?highlight=18-21 "Set up active geo-replication for single database")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Используйте следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```powershell
Remove-AzResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Создает сервер Базы данных SQL, на котором размещены отдельные базы данных и эластичные пулы. |
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | Создает эластичный пул. |
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Обновляет свойства базы данных или перемещает базу данных в эластичный пул, из него или между эластичными пулами. |
| [New-AzSqlDatabaseSecondary](/powershell/module/az.sql/new-azsqldatabasesecondary)| Создает базу данных-получатель для существующей базы данных и начинает репликацию данных. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)| Получает одну или несколько баз данных. |
| [Set-AzSqlDatabaseSecondary](/powershell/module/az.sql/set-azsqldatabasesecondary)| Преобразует базу данных-получатель в базу данных-источник для запуска отработки отказа.|
| [Get-AzSqlDatabaseReplicationLink](/powershell/module/az.sql/get-azsqldatabasereplicationlink) | Получает связи георепликации между базой данных SQL Azure и группой ресурсов или SQL Server. |
| [Remove-AzSqlDatabaseSecondary](/powershell/module/az.sql/remove-azsqldatabasesecondary) | Завершает репликацию данных между базой данных SQL и указанной базой данных-получателем. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов со всеми вложенными ресурсами. |
|||

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев PowerShell для базы данных SQL Azure можно найти [здесь](../sql-database-powershell-samples.md).
