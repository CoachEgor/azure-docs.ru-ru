---
title: Сведения об управлении учетными записями базы данных в Azure Cosmos DB
description: Узнайте, как управлять ресурсами Azure Cosmos DB с помощью шаблонов портал Azure, PowerShell, CLI и Azure Resource Manager
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 61670d757611bd0c1dd11c389282b18edb3d7fa1
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873681"
---
# <a name="manage-an-azure-cosmos-account"></a>Управление учетной записью Azure Cosmos

В этой статье описывается, как управлять различными задачами в учетной записи Azure Cosmos с помощью портала Azure, Azure PowerShell, Azure CLI и шаблонов Azure Resource Manager.

## <a name="create-an-account"></a>Создание учетной записи

### <a id="create-database-account-via-portal"></a>Портал Azure

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Интерфейс командной строки Azure

См. статью [Создание учетной записи Azure Cosmos DB с помощью Azure CLI](manage-with-cli.md#create-an-azure-cosmos-db-account)

### <a id="create-database-account-via-ps"></a>Azure PowerShell

См. статью [Создание учетной записи Azure Cosmos DB с помощью PowerShell](manage-with-powershell.md#create-account) .

### <a id="create-database-account-via-arm-template"></a>Шаблон Azure Resource Manager

Этот шаблон Azure Resource Manager создаст учетную запись Azure Cosmos для API SQL, настроенную с двумя регионами и параметрами для выбора уровня согласованности, автоматической отработки отказа и нескольких хозяев. Чтобы развернуть этот шаблон, нажмите кнопку "Deploy to Azure" (Развернуть в Azure) на странице файла сведений в разделе о [создании учетной записи Azure Cosmos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql).

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

См. раздел [Добавление и удаление регионов с Azure CLI](manage-with-cli.md#add-or-remove-regions)

### <a id="add-remove-regions-via-ps"></a>Azure PowerShell

См. раздел [Добавление и удаление регионов с помощью PowerShell](manage-with-powershell.md#update-account) .

## <a id="configure-multiple-write-regions"></a>Настройка нескольких регионов записи

### <a id="configure-multiple-write-regions-portal"></a>Портал Azure

Откройте вкладку **Глобальная репликация данных** и выберите **Включить**, чтобы включить операции записи в нескольких регионах. После включения операций записи в нескольких регионах все регионы чтения, которые в текущий момент есть в вашей учетной записи, станут регионами как для чтения, так и для записи.

![Снимок экрана: настройка нескольких источников в учетной записи Azure Cosmos](./media/how-to-manage-database-account/single-to-multi-master.png)

### <a id="configure-multiple-write-regions-cli"></a>Интерфейс командной строки Azure

См. раздел [Включение нескольких регионов записи с помощью Azure CLI](manage-with-cli.md#enable-multiple-write-regions)

### <a id="configure-multiple-write-regions-ps"></a>Azure PowerShell

См. раздел [Включение нескольких регионов записи с помощью PowerShell](manage-with-powershell.md#multi-master) .

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
            "apiVersion": "2019-08-01",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": { "defaultConsistencyLevel": "Session" },
                "locations":
                [
                    {
                        "locationName": "[parameters('primaryRegion')]",
                        "failoverPriority": 0,
                        "isZoneRedundant": false
                    },
                    {
                        "locationName": "[parameters('secondaryRegion')]",
                        "failoverPriority": 1,
                        "isZoneRedundant": false
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

См. раздел [Включение автоматической отработки отказа с помощью Azure CLI](manage-with-cli.md#enable-automatic-failover)

### <a id="enable-automatic-failover-via-ps"></a>Azure PowerShell

См. раздел [Включение автоматической отработки отказа с помощью PowerShell](manage-with-powershell.md#enable-automatic-failover) .

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

См. раздел [Установка приоритета отработки отказа с помощью Azure CLI](manage-with-cli.md#set-failover-priority)

### <a id="set-failover-priorities-via-ps"></a>Azure PowerShell

См. раздел [Установка приоритета отработки отказа с помощью PowerShell](manage-with-powershell.md#modify-failover-priority)

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

См. статью [Активация перехода на другой ресурс вручную с помощью Azure CLI](manage-with-cli.md#trigger-manual-failover)

### <a id="enable-manual-failover-via-ps"></a>Azure PowerShell

См. раздел [Активация перехода на другой ресурс вручную с помощью PowerShell](manage-with-powershell.md#trigger-manual-failover) .

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения и примеры по управлению учетной записью Azure Cosmos, а также базами данных и контейнерами см. в следующих статьях:

* [Manage Azure Cosmos DB SQL API resources using PowerShell](manage-with-powershell.md) (Управление ресурсами API SQL Azure Cosmos DB с помощью PowerShell)
* [Manage Azure Cosmos resources using Azure CLI](manage-with-cli.md) (Управление ресурсами Azure Cosmos с помощью Azure CLI)
