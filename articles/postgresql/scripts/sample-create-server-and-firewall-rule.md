---
title: "Скрипт Azure CLI. Создание базы данных Azure для PostgreSQL | Документация Майкрософт"
description: "Здесь приведен пример скрипта Azure CLI, который создает сервер базы данных Azure для PostgreSQL и настраивает правило брандмауэра на уровне сервера."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0f61358aa655a44ac7982c80eb1d796667c47bf4
ms.contentlocale: ru-ru
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Создание сервера базы данных Azure для PostgreSQL и настройка правила брандмауэра с помощью Azure CLI
Этот пример скрипта CLI создает сервер базы данных Azure для PostgreSQL и настраивает правило брандмауэра на уровне сервера. После успешного выполнения скрипта доступ к серверу PostgreSQL можно получить из всех служб Azure, а также по настроенному IP-адресу.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Пример скрипта
```azurecli
#!/bin/bash

# Create a resource group
az group create \
--name myresourcegroup \
--location westus

# Create a PostgreSQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az postgres server create \
--name mypgserver-20170401 \
--resource-group myresourcegroup \
--location westus \
--admin-user mylogin \
--admin-password <server_admin_password> \
--performance-tier Basic \
--compute-units 50 \

# Configure a firewall rule for the server
# The ip address range that you want to allow to access your server
az postgres server firewall-rule create \
--resource-group myresourcegroup \
--server mypgserver-20170401 \
--name AllowIps \
--start-ip-address 0.0.0.0 \
--end-ip-address 255.255.255.255

# Default database ‘postgres’ gets created on the server.
```

## <a name="clean-up-deployment"></a>Очистка развертывания
После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.
```azurecli
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure/overview).
- Дополнительные примеры скриптов CLI для базы данных Azure для PostgreSQL можно найти в [документации по базе данных Azure для PostgreSQL](../sample-scripts-azure-cli.md).

