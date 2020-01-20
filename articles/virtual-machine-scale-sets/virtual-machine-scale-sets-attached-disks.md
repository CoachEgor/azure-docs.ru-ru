---
title: Масштабируемые наборы виртуальных машин Azure, подключенные диски данных
description: Узнайте, как использовать подключенные диски данных с масштабируемыми наборами виртуальных машин с помощью конкретных вариантов использования.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 4/25/2017
ms.author: manayar
ms.openlocfilehash: c7fd4d89fcc66fb4110029be45ad94e21faea0e0
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278175"
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Масштабируемые наборы виртуальных машин Azure и подключенные диски данных
Для развертывания доступного хранилища в [масштабируемых наборах виртуальных машин](/azure/virtual-machine-scale-sets/) Azure поддерживаются экземпляры виртуальных машин с подключенными дисками данных. Вы можете подключить диски данных к создаваемому или существующему масштабируемому набору.

> [!NOTE]
> При создании масштабируемого набора с подключенными дисками данных для их использования вам необходимо подключать и форматировать диски в виртуальной машине (точно так же, как и для автономных виртуальных машин Azure). Для этого удобно использовать расширение пользовательских скриптов, которое вызывает скрипт, чтобы секционировать и форматировать все диски данных на виртуальной машине. Примеры см. в разделе [Azure CLI](tutorial-use-disks-cli.md#prepare-the-data-disks) [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks).


## <a name="create-and-manage-disks-in-a-scale-set"></a>Создание дисков и управление ими в масштабируемом наборе
Дополнительные сведения о том, как создать масштабируемый набор с подключенными дисками данных, подготовить и отформатировать или добавить и удалить диски данных, см. в одном из следующих руководств:

- [Azure CLI](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

В оставшейся части этой статьи приводятся конкретные варианты использования, например использование кластеров Service Fabric, для которых требуются диски с данными, или подключение существующих дисков данных с содержимым к масштабируемому набору.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Создание кластера Service Fabric с подключенными дисками данных
Каждому [типу узла](../service-fabric/service-fabric-cluster-nodetypes.md) в кластере [Service Fabric](/azure/service-fabric) в Azure соответствует масштабируемый набор виртуальных машин. С помощью шаблона Azure Resource Manager можно подключать диски данных к масштабируемым наборам, которые входят в кластер Service Fabric. В качестве отправной точки можно использовать [существующий шаблон](https://github.com/Azure-Samples/service-fabric-cluster-templates). В шаблоне включите раздел _dataDisks_ в раздел _storageProfile_ ресурсов _Microsoft.Compute/virtualMachineScaleSets_ и разверните шаблон. В следующем примере подключается диск данных размером 128 ГБ:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

При развертывании кластера можно автоматически секционировать, форматировать и подключать диски данных. Добавьте расширение пользовательского скрипта в подраздел _extensionProfile_ раздела _virtualMachineProfile_ масштабируемых наборов.

Чтобы автоматически подготовить диски данных в кластере Windows, добавьте следующие строки:

```json
{
    "name": "customScript",
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.8",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Чтобы автоматически подготовить диски данных в кластере Linux, добавьте следующие строки:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```


## <a name="adding-pre-populated-data-disks-to-an-existing-scale-set"></a>Добавление предварительно заполненных дисков данных в существующий масштабируемый набор
Диски данных, указанные в модели масштабируемого набора, всегда являются пустыми. Тем не менее вы можете подключить существующий диск данных к определенной виртуальной машине в масштабируемом наборе. Эта функция предоставляется в предварительной версии с примерами, доступными на [GitHub](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk). Если вы хотите распространить данные на все виртуальные машины в масштабируемом наборе, вы можете дублировать на данные диск и присоединить его к каждой виртуальной машины в наборе, создать пользовательский образ с данными и подготовить масштабируемый набор на основе этого образа, использовать службу "Файлы Azure" и аналогичные предложения для хранения данных.


## <a name="additional-notes"></a>Дополнительные замечания
Поддержка Управляемых дисков Azure и дисков данных, подключенных к масштабируемым наборам, доступны в версии API [_30-04-2016-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/compute/resource-manager/Microsoft.Compute/preview/2016-04-30-preview/compute.json) или более поздней версии API Microsoft.Compute.

Изначально поддержка портала Azure для подключенных дисков данных в масштабируемых наборах ограничена. В зависимости от требований для управления подключенными дисками можно использовать шаблоны Azure, интерфейс командной строки, PowerShell, пакеты SDK и REST API.


