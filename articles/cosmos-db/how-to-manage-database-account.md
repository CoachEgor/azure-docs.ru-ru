---
title: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
description: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: b2b5e58ca480aa3abaa0766319977b8d1160ebeb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59283007"
---
# <a name="manage-an-azure-cosmos-account"></a>Управление учетной записью Azure Cosmos

В этой статье описывается, как управлять учетной записью Azure Cosmos. Вы узнаете, как настроить множественные адресации, добавить и удалить регион, настроить несколько регионов записи и установить приоритеты при отработке отказа. 

## <a name="create-a-database-account"></a>Создание учетной записи базы данных

### <a id="create-database-account-via-portal"></a>Портал Azure

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Интерфейс командной строки Azure

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Настройка клиентов для поддержки нескольких веб-сайтов

### <a id="configure-clients-multi-homing-dotnet"></a>Пакет SDK для .NET версии 2

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");

// Pass the connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-dotnet-v3"></a>Пакет SDK для .NET версии 3 (предварительная версия)

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

### <a id="configure-clients-multi-homing-java-async"></a>Пакет SDK для Java (асинхронная модель)

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-javascript"></a>Пакет SDK для Node.js, JavaScript и TypeScript

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

### <a id="configure-clients-multi-homing-python"></a>Пакет SDK для Python

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="addremove-regions-from-your-database-account"></a>Добавление и удаление регионов из учетной записи базы данных

### <a id="add-remove-regions-via-portal"></a>Портал Azure

1. Перейдите к учетной записи Azure Cosmos и откройте меню **Глобальная репликация данных**.

2. Чтобы добавить регионы, выберите шестиугольники на карте с меткой **+**, которая соответствует нужным регионам. Кроме того, вы можете добавить регион, щелкнув параметр **+ Добавить регион** и выбрав регион из раскрывающегося меню.

3. Чтобы удалить регионы, очистите один или несколько регионов на карте, выбрав синие шестиугольники с флажками. Или выберите "мусорной" значок (🗑) рядом с регионом с правой стороны.

4. Нажмите кнопку **ОК**, чтобы сохранить изменения.

   ![Меню добавления и удаления регионов](./media/how-to-manage-database-account/add-region.png)

В режиме записи в одном регионе вы не можете удалить регион записи. Перед удалением текущего региона записи нужно выполнить отработку отказа в другой регион.

В режиме записи в нескольких регионах вы можете добавлять и удалять любой регион при условии, что останется по крайней мере один регион.

### <a id="add-remove-regions-via-cli"></a>Интерфейс командной строки Azure

```bash
# Create an account with 1 region
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations eastus=0

# Add a region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations eastus=0 westus=1

# Remove a region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations westus=0
```

## <a name="configure-multiple-write-regions"></a>Настройка нескольких регионов записи

### <a id="configure-multiple-write-regions-portal"></a>Портал Azure

При создании учетной записи базы данных убедитесь, что параметр **Multi-Region Writes** (Записи в нескольких регионах) включен.

![Снимок экрана создания учетной записи Azure Cosmos](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Интерфейс командной строки Azure

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>шаблон Resource Manager

Следующий код JSON представляет собой пример шаблона [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Его можно использовать для развертывания учетной записи Azure Cosmos с [уровнем согласованности с ограниченным устареванием](consistency-levels.md). Максимальный интервал устаревания — 5 секунд. Максимальное допустимое количество устаревших запросов — 100. Дополнительные сведения о формате и синтаксисе шаблона Resource Manager см. в статье [Описание структуры и синтаксиса шаблонов Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

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


## <a id="manual-failover"></a>Включение перехода на другой ресурс вручную для учетной записи Azure Cosmos

### <a id="enable-manual-failover-via-portal"></a>Портал Azure

1. Перейдите к учетной записи Azure Cosmos и откройте меню **Глобальная репликация данных**.

2. В верхней части меню выберите **Переход на другой ресурс вручную**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. В меню **Переход на другой ресурс вручную** выберите новый регион записи. Установите флажок, чтобы указать, что вы понимаете, что этот параметр изменяет ваш регион записи.

4. Нажмите кнопку **ОК**, чтобы активировать отработку отказа.

   ![Меню перехода на другой ресурс вручную на портале](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Интерфейс командной строки Azure

```bash
# Given your account currently has regions with priority: eastus=0 westus=1
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations westus=0 eastus=1
```

## <a id="automatic-failover"></a>Включение автоматического перехода на другой ресурс для учетной записи Azure Cosmos DB

### <a id="enable-automatic-failover-via-portal"></a>Портал Azure

1. В учетной записи Azure Cosmos DB откройте панель **Глобальная репликация данных**. 

2. В верхней части панели выберите **Автоматический переход на другой ресурс**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. На панели **Автоматический переход на другой ресурс** убедитесь, что для параметра **Включить автоматическую отработку отказа** установлено значение **ВКЛ**. 

4. Щелкните **Сохранить**.

   ![Меню автоматического перехода на другой ресурс на портале](./media/how-to-manage-database-account/automatic-failover.png)

В этом меню также можно задать приоритеты отработки отказа.

### <a id="enable-automatic-failover-via-cli"></a>Интерфейс командной строки Azure

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Настройка приоритетов при отработке отказа для учетной записи Azure Cosmos

### <a id="set-failover-priorities-via-portal"></a>Портал Azure

1. В учетной записи Azure Cosmos откройте панель **Глобальная репликация данных**. 

2. В верхней части панели выберите **Автоматический переход на другой ресурс**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. На панели **Автоматический переход на другой ресурс** убедитесь, что для параметра **Включить автоматическую отработку отказа** установлено значение **ВКЛ**. 

4. Чтобы изменить приоритет при отработке отказа, щелкните и перетащите регионы чтения, щелкнув три точки в левой части строки, которые появляются при наведении указателя мыши на строку. 

5. Щелкните **Сохранить**.

   ![Меню автоматического перехода на другой ресурс на портале](./media/how-to-manage-database-account/automatic-failover.png)

Вы не можете изменить регион записи в этом меню. Чтобы изменить регион записи вручную, вам нужно выполнить переход на другой ресурс вручную.

### <a id="set-failover-priorities-via-cli"></a>Интерфейс командной строки Azure

```bash
# Assume region order is initially eastus=0 westus=1 automatic failover on account creation
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies westus=0 eastus=1
```

## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь со следующими статьями:

* [Управление уровнями согласованности в Azure Cosmos DB](how-to-manage-consistency.md)
* [Управление конфликтами между регионами](how-to-manage-conflicts.md)
* [Глобальное распределение (взгляд изнутри)](global-dist-under-the-hood.md)
* [Настройка нескольких источников в приложениях в Azure Cosmos DB](how-to-multi-master.md)
* [Настройка клиентов для поддержки нескольких веб-сайтов](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Добавление и удаление регионов из учетной записи Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Создание пользовательской политики разрешения конфликтов](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)

