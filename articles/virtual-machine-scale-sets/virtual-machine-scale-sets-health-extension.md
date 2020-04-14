---
title: Используйте расширение работоспособности приложений с наборами виртуальных машин Azure
description: Узнайте, как использовать расширение "Работоспособность приложения" для наблюдения за работоспособностью приложений, развернутых в масштабируемых наборах виртуальных машин.
author: mimckitt
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: mimckitt
ms.openlocfilehash: cb5f1d48bb1a95db004d9da553e19a35071c73b0
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273738"
---
# <a name="using-application-health-extension-with-virtual-machine-scale-sets"></a>Использование расширения "Работоспособность приложения" с масштабируемыми наборами виртуальных машин
Мониторинг работоспособности приложения предоставляет важные сведения, которые позволяют определить, когда требуется администрирование и обновление развертывания. Масштабируемые наборы виртуальных машин Azure поддерживают [последовательные обновления](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model), включая [автоматические обновления образа ОС](virtual-machine-scale-sets-automatic-upgrade.md), которые зависят от мониторинга работоспособности отдельных экземпляров для обновления развертывания.

В этой статье описывается, как использовать расширение "Работоспособность приложения" для наблюдения за работоспособностью приложений, развернутых в масштабируемых наборах виртуальных машин.

## <a name="prerequisites"></a>Предварительные требования
В данной статье предполагается, что вы знакомы с:
-   [Расширения виртуальных](../virtual-machines/extensions/overview.md) машин Azure
-   [Изменением](virtual-machine-scale-sets-upgrade-scale-set.md) масштабируемых наборов виртуальных машин.

## <a name="when-to-use-the-application-health-extension"></a>Когда использовать расширение "Работоспособность приложения"
Расширение "Работоспособность приложения" развертывается внутри экземпляра масштабируемого набора виртуальных машин и сообщает о состоянии работоспособности виртуальной машины изнутри этого экземпляра. Вы можете настроить расширение так, чтобы оно проверяло конечную точку приложения и обновляло состояние приложения на этом экземпляре. Azure проверяет состояние этого экземпляра, чтобы определить, подходит ли экземпляр для операций обновления.

Так как расширение сообщает о состоянии работоспособности из виртуальной машины, это расширение можно применять в ситуациях, когда внешние пробы, например, пробы работоспособности приложений (использующие пользовательские [пробы](../load-balancer/load-balancer-custom-probe-overview.md) Azure Load Balancer), не могут использоваться.

## <a name="extension-schema"></a>Схема расширения

В следующем коде JSON показана схема расширения "Работоспособность приложения". Для расширения требуется как минимум запрос "HTTP" или "TCP" со связанным портом или путем запроса соответственно.

```json
{
  "type": "extensions",
  "name": "HealthExtension",
  "apiVersion": "2018-10-01",
  "location": "<location>",  
  "properties": {
    "publisher": "Microsoft.ManagedServices",
    "type": "< ApplicationHealthLinux or ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}  
```

### <a name="property-values"></a>Значения свойств

| Имя | Значение и пример | Тип данных
| ---- | ---- | ---- 
| версия_API | `2018-10-01` | Дата |
| publisher | `Microsoft.ManagedServices` | строка |
| type | `ApplicationHealthLinux` (Linux), `ApplicationHealthWindows` (Windows) | строка |
| typeHandlerVersion | `1.0` | INT |

### <a name="settings"></a>Параметры

| Имя | Значение и пример | Тип данных
| ---- | ---- | ----
| protocol | `http` или `tcp` | строка |
| порт | Необязательно, если используется протокол `http`; обязательно, если используется протокол `tcp` | INT |
| requestPath | Обязательно, если используется протокол `http`; не разрешено, если используется протокол `tcp` | строка |

## <a name="deploy-the-application-health-extension"></a>Развертывание расширение "Работоспособность приложения"
Существует несколько способов развертывания расширения "Работоспособность приложения" в масштабируемые наборы, которые описаны в приведенных ниже примерах.

### <a name="rest-api"></a>REST API

В следующем примере расширение "Работоспособность приложения" (с именем myHealthExtension) добавляется в атрибут extensionProfile в модели масштабируемого набора на основе Windows.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/myHealthExtension?api-version=2018-10-01`
```

```json
{
  "name": "myHealthExtension",
  "properties": {
    "publisher": " Microsoft.ManagedServices",
    "type": "< ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}
```
Используйте `PATCH` для изменения уже развернутого расширения.

### <a name="azure-powershell"></a>Azure PowerShell

Используйте командлет [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension), чтобы добавить расширение "Работоспособность приложения" в определение модели масштабируемого набора.

В следующем примере расширение "Работоспособность приложения" добавляется в `extensionProfile` модели масштабируемого набора на основе Windows. В примере используется новый модуль Azure PowerShell.

```azurepowershell-interactive
# Define the scale set variables
$vmScaleSetName = "myVMScaleSet"
$vmScaleSetResourceGroup = "myVMScaleSetResourceGroup"

# Define the Application Health extension properties
$publicConfig = @{"protocol" = "http"; "port" = 80; "requestPath" = "/healthEndpoint"};
$extensionName = "myHealthExtension"
$extensionType = "ApplicationHealthWindows"
$publisher = "Microsoft.ManagedServices"

# Get the scale set object
$vmScaleSet = Get-AzVmss `
  -ResourceGroupName $vmScaleSetResourceGroup `
  -VMScaleSetName $vmScaleSetName

# Add the Application Health extension to the scale set model
Add-AzVmssExtension -VirtualMachineScaleSet $vmScaleSet `
  -Name $extensionName `
  -Publisher $publisher `
  -Setting $publicConfig `
  -Type $extensionType `
  -TypeHandlerVersion "1.0" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```


### <a name="azure-cli-20"></a>Azure CLI 2.0

Используйте команду [az vmss extension set](/cli/azure/vmss/extension#az-vmss-extension-set), чтобы добавить расширение "Работоспособность приложения" в определение модели масштабируемого набора.

Следующий пример добавляет расширение работоспособности приложения к модели масштабирования набора масштаба на основе Linux.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthLinux \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --settings ./extension.json
```
Содержимое файла extension.json.

```json
{
  "protocol": "<protocol>",
  "port": "<port>",
  "requestPath": "</requestPath>"
}
```


## <a name="troubleshoot"></a>Диагностика
Выходные данные выполнения расширения регистрируются в файле, расположенном в следующих каталогах:

```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.ManagedServices.ApplicationHealthWindows\<version>\
```

```Linux
/var/lib/waagent/apphealth
```

Журналы также периодически записывают состояние работоспособности приложения.

## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как [развертывать приложение](virtual-machine-scale-sets-deploy-app.md) в масштабируемых наборах виртуальных машин.
