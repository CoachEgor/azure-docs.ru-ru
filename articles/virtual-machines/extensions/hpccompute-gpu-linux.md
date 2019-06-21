---
title: Расширение драйвера GPU NVIDIA для виртуальных машин Linux в Azure | Документация Майкрософт
description: Расширение Microsoft Azure для установки драйверов GPU NVIDIA на вычислительных виртуальных машинах серии N под управлением Linux.
services: virtual-machines-linux
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/11/2019
ms.author: roiyz
ms.openlocfilehash: 5a184c72da8af0d451902a164c8b71a94a01883f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683174"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>Расширение драйвера GPU NVIDIA для Linux

## <a name="overview"></a>Обзор

Это расширение устанавливает драйверы GPU NVIDIA на виртуальных машинах серии N для Linux. В зависимости от семейства виртуальных машин расширение устанавливает драйверы CUDA или GRID. При установке драйверов NVIDIA с помощью этого расширения требуется принять условия [лицензионного соглашения NVIDIA](https://go.microsoft.com/fwlink/?linkid=874330). Во время установки драйвера виртуальная машина может быть перезагружена для завершения процедуры.

Это расширение также доступно для установки драйверов GPU NVIDIA на [виртуальных машинах Windows серии N](hpccompute-gpu-windows.md).

## <a name="prerequisites"></a>Технические условия

### <a name="operating-system"></a>Операционная система

Это расширение поддерживает указанные ниже дистрибутивы, в зависимости от поддержки драйвера в конкретной версии ОС.

| Дистрибутив | Version |
|---|---|
| Linux: Ubuntu | 16.04 LTS, 18.04 LTS |
| Linux: Red Hat Enterprise Linux. | 7.3, 7.4, 7.5, 7.6 |
| Linux: CentOS | 7.3, 7.4, 7.5, 7.6 |

### <a name="internet-connectivity"></a>Подключение к Интернету

Для работы расширения Microsoft Azure для драйверов GPU NVIDIA требуется, чтобы целевая виртуальная машина была доступна и подключена к Интернету.

## <a name="extension-schema"></a>Схема расширения

В следующем коде JSON показана схема расширения.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>properties

| Name | Значение и пример | Тип данных |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.HpcCompute | string |
| type | NvidiaGpuDriverLinux | string |
| typeHandlerVersion | 1.2 | int |

### <a name="settings"></a>Параметры

Все эти параметры не являются обязательными. По умолчанию ядро не обновляется (если это не требуется для установки драйвера), устанавливается последний поддерживаемый драйвер и набор инструментов CUDA Toolkit (если это применимо).

| Name | Описание | Значение по умолчанию | Допустимые значения | Тип данных |
| ---- | ---- | ---- | ---- | ---- |
| updateOS | Обновление ядра, даже если для установки драйвера это не требуется | false | true, false | boolean |
| driverVersion | NV: версия драйвера GRID.<br> NC/ND: версия набора инструментов CUDA Toolkit. Последние версии драйверов для выбранного набора CUDA Toolkit устанавливаются автоматически. | latest | GRID: "418.70", "410.92", "410.71", "390.75", "390.57", "390.42"<br> CUDA: 10.0.130, 9.2.88, 9.1.85. | string |
| installCUDA | Установка набора инструментов CUDA Toolkit. Это относится только к виртуальным машинам серии NC/ND. | Да | true, false | boolean |


## <a name="deployment"></a>Развертывание


### <a name="azure-resource-manager-template"></a>Шаблон Azure Resource Manager 

Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. Шаблоны идеально подходят для развертывания одной или нескольких виртуальных машин, требующих настройки после развертывания.

Конфигурацию JSON для расширения виртуальной машины можно вложить в ресурс виртуальной машины или поместить в корень или на верхний уровень JSON-файла шаблона Resource Manager. Размещение конфигурации JSON влияет на значения имени и типа ресурса. Дополнительные сведения см. в разделе [Указание имени и типа дочернего ресурса в шаблоне Resource Manager](../../azure-resource-manager/resource-manager-template-child-resource.md). 

В следующем примере предполагается, что расширение виртуальной машины расположено в ресурсе виртуальной машины. При вложении ресурса расширения JSON помещается в объект `"resources": []` виртуальной машины.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

Ниже показаны примеры Azure Resource Manager и PowerShell, о которых говорилось ранее. Также в них добавлены настраиваемые параметры в качестве примера установки драйвера, отличного от драйвера умолчанию. В частности, обновляется ядро ОС и устанавливается конкретный драйвер версии набора инструментов CUDA Toolkit.

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
    "updateOS": true, `
    "driverVersion": "9.1.85", `
  }'
```

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Сведения о состоянии развертывания расширения можно получить на портале Azure, а также с помощью Azure PowerShell или Azure CLI. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Выходные данные выполнения расширения регистрируются в следующем файле:

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Коды выхода

| Код выхода | Значение | Возможное действие |
| :---: | --- | --- |
| 0 | Операция выполнена успешно |
| 1 | Неправильное использование расширения | Проверьте выходные данные журнала выполнения |
| 10 | Службы Integration Services в Linux для Hyper-V и Azure недоступны или не установлены | Проверьте выходные данные lspci |
| 11 | Для этого размера виртуальной машины не обнаружен драйвер GPU NVIDIA | Используйте [поддерживаемые размер виртуальной машины и ОС](../linux/n-series-driver-setup.md) |
| 12 | Предложение образа не поддерживается |
| 13 | Размер виртуальной машины не поддерживается | Для развертывания используйте виртуальную машину серии N |
| 14 | Не удалось выполнить операцию. | Проверьте выходные данные журнала выполнения |


### <a name="support"></a>Поддержка

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/community/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните "Получить поддержку". Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о расширениях виртуальных машин см. в обзоре [расширений и компонентов виртуальной машины для Linux](features-linux.md).

См. дополнительные сведения о [размерах виртуальных машин серии N, оптимизированных для GPU](../linux/sizes-gpu.md).
