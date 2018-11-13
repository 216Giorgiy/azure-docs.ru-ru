---
title: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
description: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 67cd78d4900b8ce53cf0c50116c02a9c1b967687
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958769"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Управление учетными записями базы данных в Azure Cosmos DB

В этой статье описывается, как управлять учетной записью Cosmos DB для настройки поддержки нескольких веб-сайтов, добавления и удаления региона, настройки нескольких регионов записи и установки приоритетов при отработке отказа. 

## <a name="create-a-database-account"></a>Создание учетной записи базы данных

### <a id="create-database-account-via-portal"></a>Портал Azure

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Интерфейс командной строки Azure

```bash
# Create an account
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Настройка клиентов для поддержки нескольких веб-сайтов

### <a id="configure-clients-multi-homing-dotnet"></a>Пакет SDK для .NET

```csharp
// Create a new Connection Policy
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to Preferred locations
// The name of the location will match what you see in the portal/etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the Connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>Пакет SDK для Java (асинхронная модель)

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>Пакет SDK для Java (синхронная модель)

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>Пакет SDK для Node.js, JavaScript и TypeScript

```javascript
// Set up the connection policy with your preferred regions
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>Пакет SDK для Python

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Добавление и удаление регионов из учетной записи базы данных

### <a id="add-remove-regions-via-portal"></a>Портал Azure

1. Перейдите к своей учетной записи Azure Cosmos DB и откройте меню **Глобальная репликация данных**.

2. Чтобы добавить регионы, выберите один или несколько регионов на карте, щелкнув пустой шестиугольник с меткой **+**, соответствующей вашему региону. Кроме того, вы можете добавить регион, щелкнув параметр **+ Добавить регион** и выбрав регион из раскрывающегося меню.

3. Чтобы удалить регион, отмените выбор одного или нескольких регионов на карте, щелкнув синие шестиугольники с галочкой или выбрав значок корзины для мусора (🗑) рядом с регионом справа.

4. Нажмите кнопку "Сохранить", чтобы сохранить изменения.

   ![Меню добавления и удаления регионов](./media/how-to-manage-database-account/add-region.png)

В режиме записи в одном регионе вы не можете удалить регион записи. Перед удалением текущего региона записи нужно выполнить отработку отказа в другой регион.

В режиме записи в нескольких регионах вы можете добавлять и удалять любой регион при условии, что остается по крайней мере один регион.

### <a id="add-remove-regions-via-cli"></a>Интерфейс командной строки Azure

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'westus=0'
```

## <a name="configure-multiple-write-regions"></a>Настройка нескольких регионов записи

### <a id="configure-multiple-write-regions-portal"></a>Портал Azure

При создании учетной записи базы данных убедитесь, что параметр **Multi-Region Writes** (Записи в нескольких регионах) включен.

![Снимок экрана создания учетной записи Cosmos DB](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Интерфейс командной строки Azure

```bash
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>шаблон Resource Manager

Следующий код JSON представляет собой пример шаблона Resource Manager. Вы можете использовать его для развертывания учетной записи Azure Cosmos DB с политикой согласованности с ограниченным устареванием. При этом максимальный интервал устаревания составляет 5 секунд, а максимальное количество устаревших запросов — 100. Дополнительные сведения о формате шаблона Resource Manager и синтаксисе см. в документации по [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```


## <a name="enable-manual-failover-for-your-cosmos-account"></a>Включение перехода на другой ресурс вручную для учетной записи Cosmos

### <a id="enable-manual-failover-via-portal"></a>Портал Azure

1. Перейдите к своей учетной записи Azure Cosmos DB и откройте меню **Глобальная репликация данных**.

2. В верхней части меню щелкните **Переход на другой ресурс вручную**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. В меню **Отработка отказа вручную** выберите новый регион записи и установите флажок для подтверждения того, что вы понимаете, что это приведет к изменению региона записи.

4. Нажмите кнопку "ОК", чтобы запустить отработку отказа.

   ![Меню перехода на другой ресурс вручную на портале](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Интерфейс командной строки Azure

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a name="enable-automatic-failover-for-your-cosmos-account"></a>Включение автоматического перехода на другой ресурс вручную для учетной записи Cosmos

### <a id="enable-automatic-failover-via-portal"></a>Портал Azure

1. В учетной записи Azure Cosmos DB откройте панель **Глобальная репликация данных**. 

2. В верхней части панели щелкните **Автоматический переход на другой ресурс**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. На панели **Автоматический переход на другой ресурс** убедитесь, что для параметра **Включить автоматическую отработку отказа** установлено значение **ВКЛ**. 

4. В нижней части меню щелкните "Сохранить".

   ![Меню автоматического перехода на другой ресурс на портале](./media/how-to-manage-database-account/automatic-failover.png)

В этом меню также можно задать приоритеты при отработке отказа.

### <a id="enable-automatic-failover-via-cli"></a>Интерфейс командной строки Azure

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-cosmos-account"></a>Настройка приоритетов при отработке отказа для учетной записи Cosmos

### <a id="set-failover-priorities-via-portal"></a>Портал Azure

1. В учетной записи Azure Cosmos DB откройте панель **Глобальная репликация данных**. 

2. В верхней части панели щелкните **Автоматический переход на другой ресурс**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. На панели **Автоматический переход на другой ресурс** убедитесь, что для параметра **Включить автоматическую отработку отказа** установлено значение **ВКЛ**. 

4. Вы можете изменить приоритет при отработке отказа, щелкнув и перетащив регионы чтения, щелкнув три точки в левой части строки, которые появляются при наведении указателя мыши на строку. 

5. В нижней части меню щелкните "Сохранить".

   ![Меню автоматического перехода на другой ресурс на портале](./media/how-to-manage-database-account/automatic-failover.png)

Вы не можете изменить регион записи в этом меню. Чтобы изменить регион записи вручную, вам нужно выполнить переход на другой ресурс вручную.

### <a id="set-failover-priorities-via-cli"></a>Интерфейс командной строки Azure

```bash
az cosmosdb failover-priority-change --name <Cosmos DB Account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Дополнительная информация

Сведения об управлении уровнями согласованности и конфликтами данных в базе данных Cosmos DB см. по следующим ссылкам:

* [Управление согласованностью](how-to-manage-consistency.md)
* [Управление конфликтами между регионами](how-to-manage-conflicts.md)

