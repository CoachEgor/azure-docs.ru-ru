---
title: Развертывание кэша Azure для Redis с помощью Azure Resource Manager
description: Узнайте, как использовать шаблон Azure Resource Manager для развертывания ресурса кэша Azure для Redis. Для распространенных сценариев предусмотрены шаблоны.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 01/23/2017
ms.openlocfilehash: 787edf662aa3a34e167db61b0a89dfc5c2944219
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75412404"
---
# <a name="create-an-azure-cache-for-redis-using-a-template"></a>Создание экземпляра кэша Redis для Azure с помощью шаблона

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

В этом разделе описано, как создать шаблон Azure Resource Manager, который развертывает кэш Redis для Azure. Кэш можно использовать с существующей учетной записи хранения для размещения данных диагностики. Вы узнаете, как определить развертываемые ресурсы и параметры, указываемые при развертывании. Этот шаблон можно использовать для собственных развертываний или настроить его в соответствии с вашими требованиями.

В настоящее время параметры диагностики являются общими для всех кэшей в одном регионе подписки. Обновление одного кэша в регионе влияет на все кэши в нем.

Дополнительные сведения о создании шаблонов см. в статье [Создание шаблонов Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md). Сведения о синтаксисе JSON и свойствах для типов ресурсов кэша см. в справочнике по [типам ресурсов Microsoft.Cache](/azure/templates/microsoft.cache/allversions).

См. [полный шаблон кэша Redis для Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Доступны шаблоны Resource Manager для нового [уровня "Премиум"](cache-premium-tier-intro.md) . 
> 
> * [Создание кэша Azure для Redis уровня "Премиум" с кластеризацией](https://azure.microsoft.com/resources/templates/201-redis-premium-cluster-diagnostics/)
> * [Создание кэша Redis для Azure уровня "Премиум" с сохранением данных](https://azure.microsoft.com/resources/templates/201-redis-premium-persistence/)
> * [Создание кэша Premium Redis, развернутого в виртуальной сети](https://azure.microsoft.com/resources/templates/201-redis-premium-vnet/)
> 
> Чтобы узнать о новых шаблонах, обратитесь к статье [Шаблоны быстрого запуска Azure](https://azure.microsoft.com/documentation/templates/) и выполните поиск по критерию `Azure Cache for Redis`.
> 
> 

## <a name="what-you-will-deploy"></a>Что именно развертывается
С помощью этого шаблона вы развернете кэш Redis для Azure, который использует существующую учетную запись хранения для диагностических данных.

Чтобы выполнить развертывание автоматически, нажмите следующую кнопку.

[![Развертывание в Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Параметры
С помощью диспетчера ресурсов Azure можно определить параметры значений, которые должны указываться на этапе развертывания шаблона. В шаблоне есть раздел "Параметры", содержащий все значения параметров.
Для этих значений необходимо определить параметры, которые будут зависеть от развертываемого проекта либо от среды, в которой выполняется развертывание. Не определяйте параметры для значений, которые не меняются. Значение каждого параметра в шаблоне определяет развертываемые ресурсы. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Расположение кэша Redis для Azure. Для наилучшей производительности используйте то же расположение, где находится приложение, которое будет пользоваться кэшем.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
Имя существующей учетной записи хранения, которую вы хотите использовать для диагностики. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Логическое значение, указывающее, следует ли разрешить доступ к портам, отличным от SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Значение, указывающее, включена ли диагностика. Используйте значение ON или OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Развертываемые ресурсы
### <a name="azure-cache-for-redis"></a>Кэш Redis для Azure
Создает кэш Redis для Azure.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2017-05-01-preview",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Команды для выполнения развертывания
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup
