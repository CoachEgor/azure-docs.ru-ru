---
title: Service Fabric Azure Resource Manager снятие развертывания
description: В этой статье представлен обзор распространенных ошибок, возникших при развертывании кластера Service Fabric с помощью Azure Resource Manager, и способы их устранения.
services: service-fabric
documentationcenter: .net
author: peterpogorski
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: pepogors
ms.openlocfilehash: fe5ff2a5eeb4b2c73165d1577702eb6af7079b61
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426740"
---
# <a name="service-fabric-guardrails"></a>Service Fabric снятие 
При развертывании кластера Service Fabric помещаются снятие, что приведет к сбою развертывания Azure Resource Manager в случае недопустимой конфигурации кластера. В следующих разделах приведены общие сведения о распространенных проблемах с конфигурацией кластера и действия, необходимые для устранения этих проблем. 

## <a name="durability-mismatch"></a>Несоответствие устойчивости
### <a name="overview"></a>Обзор
Значение устойчивости для типа узла Service Fabric определяется в двух разных разделах шаблона Azure Resource Manager. Раздел расширения масштабируемого набора виртуальных машин в ресурсе масштабируемого набора виртуальных машин и раздел типа узла в ресурсе кластера Service Fabric. Необходимо, чтобы значение устойчивости в этих разделах совпадало, в противном случае произойдет сбой развертывания ресурса.

Следующий раздел содержит пример несоответствия устойчивости между параметром устойчивости расширения масштабируемого набора виртуальных машин и параметром устойчивости типа узла Service Fabric.  

**Параметр устойчивости масштабируемого набора виртуальных машин**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Параметр устойчивости типа узла Service Fabric** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>Сообщения об ошибках
* Несоответствие устойчивости масштабируемого набора виртуальных машин не соответствует текущему уровню устойчивости типа узла Service Fabric
* Устойчивость масштабируемого набора виртуальных машин не соответствует целевому Service Fabric уровня устойчивости типа узла
* Устойчивость масштабируемого набора виртуальных машин соответствует текущему уровню устойчивости Service Fabric или целевому Service Fabricному типу узла уровня устойчивости 


### <a name="mitigation"></a>Меры по снижению риска
Чтобы устранить несоответствие устойчивости, оно обозначается любым из указанных выше сообщений об ошибке:
1. Обновите уровень устойчивости в разделе "расширение масштабируемого набора виртуальных машин" или "тип узла Service Fabric" шаблона Azure Resource Manager, чтобы убедиться, что значения совпадают.
2. Повторно разверните шаблон Azure Resource Manager с обновленными значениями.

## <a name="next-steps"></a>Дальнейшие действия
* Создание кластера на основе виртуальных машин или компьютеров под управлением Windows Server: [Создание кластера Azure Service Fabric в локальной или облачной средах](service-fabric-cluster-creation-for-windows-server.md)
* Создание кластера на основе виртуальных машин или компьютеров под управлением Linux: [Создание кластера Linux](service-fabric-cluster-creation-via-portal.md).
* Устранение неполадок Service Fabric: [руководства по устранению неполадок](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
