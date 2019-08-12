---
title: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
description: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: a5ba45fce2870b44a6ebb1be43cc1f36b3cda311
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815165"
---
# <a name="manage-an-azure-cosmos-account"></a>Управление учетной записью Azure Cosmos

В этой статье описывается, как управлять различными задачами в учетной записи Azure Cosmos с помощью портала Azure, Azure PowerShell, Azure CLI и шаблонов Azure Resource Manager.

## <a name="create-an-account"></a>Создание учетной записи

### <a id="create-database-account-via-portal"></a>Портал Azure

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Интерфейс командной строки Azure

```azurecli-interactive
# Create an account
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname' # must be lower case and < 31 characters

az cosmosdb create \
   --name $accountName \
   --resource-group $resourceGroupName \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=WestUS failoverPriority=0 isZoneRedundant=False \
   --locations regionName=EastUS failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations true
```

### <a id="create-database-account-via-ps"></a>Azure PowerShell
```azurepowershell-interactive
# Create an Azure Cosmos account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case and < 31 characters

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="create-database-account-via-arm-template"></a>Шаблон Azure Resource Manager

Этот шаблон Azure Resource Manager позволяет создать учетную запись Azure Cosmos для любого поддерживаемого API, настроенную для двух регионов и позволяющую выбрать уровень согласованности, автоматический переход на другой ресурс и несколько источников. Чтобы развернуть этот шаблон, нажмите кнопку "Deploy to Azure" (Развернуть в Azure) на странице файла сведений в разделе о [создании учетной записи Azure Cosmos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-create-multi-region-account).

## <a name="addremove-regions-from-your-database-account"></a>Добавление и удаление регионов из учетной записи базы данных

### <a id="add-remove-regions-via-portal"></a>Портал Azure

1. Войдите на [портал Azure](https://portal.azure.com). 

1. Перейдите к учетной записи Azure Cosmos и откройте меню **Глобальная репликация данных**.

1. Чтобы добавить регионы, выберите шестиугольники на карте с меткой **+** , которая соответствует нужным регионам. Кроме того, вы можете добавить регион, щелкнув параметр **+ Добавить регион** и выбрав регион из раскрывающегося меню.

1. Чтобы удалить регионы, очистите один или несколько регионов на карте, выбрав синие шестиугольники с флажками. Или выберите "мусорной" значок (🗑) рядом с регионом с правой стороны.

1. Нажмите кнопку **ОК**, чтобы сохранить изменения.

   ![Меню добавления и удаления регионов](./media/how-to-manage-database-account/add-region.png)

В режиме записи в одном регионе вы не можете удалить регион записи. Перед удалением текущего региона записи нужно выполнить отработку отказа в другой регион.

В режиме записи в нескольких регионах вы можете добавлять и удалять любой регион при условии, что останется по крайней мере один регион.

### <a id="add-remove-regions-via-cli"></a>Интерфейс командной строки Azure

```azurecli-interactive
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname' # must be lower case and <31 characters

# Create an account with 1 region
az cosmosdb create --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False --locations regionName=EastUS failoverPriority=1 isZoneRedundant=False

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False
```

### <a id="add-remove-regions-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Create an account with 1 region
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case and <31 characters

$locations = @( @{ "locationName"="West US"; "failoverPriority"=0 } )
$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Add a region
$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @( 
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East Us"; "failoverPriority"=1 } 
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Azure Resource Manager does not wait on the resource update
Write-Host "Confirm region added before continuing..."

# Remove a region
$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @( @{ "locationName"="West US"; "failoverPriority"=0 } )

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a id="configure-multiple-write-regions"></a>Настройка нескольких регионов записи

### <a id="configure-multiple-write-regions-portal"></a>Портал Azure

Откройте вкладку **Глобальная репликация данных** и выберите **Включить**, чтобы включить операции записи в нескольких регионах. После включения операций записи в нескольких регионах все регионы чтения, которые в текущий момент есть в вашей учетной записи, станут регионами как для чтения, так и для записи. 

> [!NOTE]
> После включения операций записи в нескольких регионах их нельзя отключить. 

![Снимок экрана: настройка нескольких источников в учетной записи Azure Cosmos](./media/how-to-manage-database-account/single-to-multi-master.png)

Пишите нам на псевдоним askcosmosdb@microsoft.com, если у вас возникнут дополнительные вопросы об этой функции. 

### <a id="configure-multiple-write-regions-cli"></a>Интерфейс командной строки Azure

```azurecli-interactive
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'
az cosmosdb update --name $accountName --resource-group $resourceGroupName --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Update an Azure Cosmos account from single to multi-master

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$account.Properties.enableMultipleWriteLocations = "true"
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="configure-multiple-write-regions-arm"></a>шаблон Resource Manager

Учетную запись можно преобразовать из записи с одним источником в учетную запись с несколькими источниками путем развертывания шаблона Resource Manager, используемого для создания учетной записи, и применения параметра `enableMultipleWriteLocations: true`. Следующий шаблон Azure Resource Manager — это самый простой шаблон, который позволяет развернуть учетную запись Azure Cosmos для API SQL с двумя регионами и несколькими включенными расположениями записи.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String",
            "defaultValue": "[resourceGroup().location]"
        },
        "primaryRegion":{
            "type":"string",
            "metadata": {
                "description": "The primary replica region for the Cosmos DB account."
            }
        },
        "secondaryRegion":{
            "type":"string",
            "metadata": {
              "description": "The secondary replica region for the Cosmos DB account."
          }
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": { "defaultConsistencyLevel": "Session" },
                "locations":
                [
                    {
                        "locationName": "[parameters('primaryRegion')]",
                        "failoverPriority": 0
                    },
                    {
                        "locationName": "[parameters('secondaryRegion')]",
                        "failoverPriority": 1
                    }
                ],
                "enableMultipleWriteLocations": true
            }
        }
    ]
}
```

## <a id="automatic-failover"></a>Включение автоматического перехода на другой ресурс для учетной записи Azure Cosmos

Если какой-либо регион становится недоступным, возможность автоматического перехода на другой ресурс позволяет Azure Cosmos DB выполнять отработку отказа в регион с наибольшим приоритетом отработки отказа без каких-либо действий со стороны пользователя. При включенном автоматическом переходе на другой ресурс приоритет региона можно изменить. В учетной записи должно быть два или более регионов, чтобы включить автоматический переход на другой ресурс.

### <a id="enable-automatic-failover-via-portal"></a>Портал Azure

1. В учетной записи Azure Cosmos откройте панель **Глобальная репликация данных**.

2. В верхней части панели выберите **Автоматический переход на другой ресурс**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. На панели **Автоматический переход на другой ресурс** убедитесь, что для параметра **Включить автоматическую отработку отказа** установлено значение **ВКЛ**. 

4. Щелкните **Сохранить**.

   ![Меню автоматического перехода на другой ресурс на портале](./media/how-to-manage-database-account/automatic-failover.png)

### <a id="enable-automatic-failover-via-cli"></a>Интерфейс командной строки Azure

```azurecli-interactive
# Enable automatic failover on an existing account
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb update --name $accountName --resource-group $resourceGroupName --enable-automatic-failover true
```

### <a id="enable-automatic-failover-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$account.Properties.enableAutomaticFailover="true";
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Настройка приоритетов при отработке отказа для учетной записи Azure Cosmos

После настройки автоматического перехода на другой ресурс в учетной записи Cosmos можно изменить приоритет при отработке отказа для других регионов.

> [!IMPORTANT]
> Вы не можете изменить регион записи (приоритет отработки отказа равен нулю) после настройки учетной записи для автоматического перехода на другой ресурс. Чтобы изменить регион записи, необходимо отключить автоматический переход на другой ресурс и выполнить переход на другой ресурс вручную.

### <a id="set-failover-priorities-via-portal"></a>Портал Azure

1. В учетной записи Azure Cosmos откройте панель **Глобальная репликация данных**.

2. В верхней части панели выберите **Автоматический переход на другой ресурс**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. На панели **Автоматический переход на другой ресурс** убедитесь, что для параметра **Включить автоматическую отработку отказа** установлено значение **ВКЛ**.

4. Чтобы изменить приоритет при отработке отказа, щелкните и перетащите регионы чтения, щелкнув три точки в левой части строки, которые появляются при наведении указателя мыши на строку.

5. Щелкните **Сохранить**.

   ![Меню автоматического перехода на другой ресурс на портале](./media/how-to-manage-database-account/automatic-failover.png)

### <a id="set-failover-priorities-via-cli"></a>Интерфейс командной строки Azure

```azurecli-interactive
# Assume region order is initially eastus=0 westus=1 southeastasia=2 on account creation
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb failover-priority-change --name $accountName --resource-group $resourceGroupName --failover-policies eastus=0 southeastasia=1 westus=2
```

### <a id="set-failover-priorities-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Assume account currently has regions with priority: West US = 0, East US = 1, Southeast Asia = 2
$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$failoverPolicies = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="Southeast Asia"; "failoverPriority"=1 },
    @{ "locationName"="East US"; "failoverPriority"=2 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a id="manual-failover"></a>Выполнение перехода на другой ресурс вручную для учетной записи Azure Cosmos

> [!IMPORTANT]
> Для успеха этой операции учетная запись Azure Cosmos должна быть настроена на переход на другой ресурс вручную.

Процесс выполнения перехода на другой ресурс вручную включает изменение региона записи учетной записи (приоритет отработки отказа = 0) на другой регион, настроенный для учетной записи.

> [!NOTE]
> Выполнить отработку отказа для учетной записи с несколькими источниками вручную невозможно. Для приложений, использующих пакет SDK для Azure Cosmos, пакет SDK будет определять, когда регион становится недоступным, а затем выполнять автоматическое перенаправление в следующий ближайший регион при использовании API множественной адресации в пакете SDK.

### <a id="enable-manual-failover-via-portal"></a>Портал Azure

1. Перейдите к учетной записи Azure Cosmos и откройте меню **Глобальная репликация данных**.

2. В верхней части меню выберите **Переход на другой ресурс вручную**.

   ![Меню глобальной репликации данных](./media/how-to-manage-database-account/replicate-data-globally.png)

3. В меню **Переход на другой ресурс вручную** выберите новый регион записи. Установите флажок, чтобы указать, что вы понимаете, что этот параметр изменяет ваш регион записи.

4. Нажмите кнопку **ОК**, чтобы активировать отработку отказа.

   ![Меню перехода на другой ресурс вручную на портале](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Интерфейс командной строки Azure

```azurecli-interactive
# Assume account currently has regions with priority: eastus=0 westus=1
# Change the priority order to trigger a failover of the write region
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False --locations regionName=eastus failoverPriority=1 isZoneRedundant=False
```

### <a id="enable-manual-failover-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Assume account currently has regions with priority: West US = 0, East US = 1
# Change the priority order to trigger a failover of the write region
$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$locations = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

$account.Properties.locations=$locations;
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения и примеры по управлению учетной записью Azure Cosmos, а также базами данных и контейнерами см. в следующих статьях:

* [Manage Azure Cosmos DB SQL API resources using PowerShell](manage-with-powershell.md) (Управление ресурсами API SQL Azure Cosmos DB с помощью PowerShell)
* [Manage Azure Cosmos resources using Azure CLI](manage-with-cli.md) (Управление ресурсами Azure Cosmos с помощью Azure CLI)
