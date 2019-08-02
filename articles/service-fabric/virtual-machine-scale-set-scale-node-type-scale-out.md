---
title: Добавление типа узла к кластеру Azure Service Fabric | Документация Майкрософт
description: Узнайте, как масштабировать кластер Service Fabric путем добавления масштабируемого набора виртуальных машин.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/13/2019
ms.author: atsenthi
ms.openlocfilehash: 1414e656a358af1e258c823cc7ec747fefa986ba
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598695"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Масштабирование кластера Service Fabric путем добавления масштабируемого набора виртуальных машин
В этой статье описывается, как масштабировать кластер Azure Service Fabric путем добавления нового типа узла в существующий кластер. Кластер Service Fabric — это подключенный к сети набор виртуальных машин или физических компьютеров, в котором вы развертываете микрослужбы и управляете ими. Компьютер или виртуальная машина, которая входит в состав кластера. Масштабируемые наборы виртуальных машин относятся к вычислительным ресурсам Azure. Их можно использовать для развертывания коллекции виртуальных машин и управления ею как набором. Каждый тип узла, определенный в кластере Azure, [настроен как отдельный масштабируемый набор](service-fabric-cluster-nodetypes.md). Затем каждым типом узла можно управлять отдельно. После создания кластера Service Fabric вы можете масштабировать кластер по горизонтали, добавив тип узла (масштабируемый набор виртуальных машин) к существующему кластеру.  Кластер можно масштабировать в любое время, даже когда в нем выполняются рабочие нагрузки.  Вместе с кластером автоматически масштабируются ваши приложения.

## <a name="add-an-additional-scale-set-to-an-existing-cluster"></a>Добавление дополнительного масштабируемого набора в существующий кластер
Добавление нового типа узла (который поддерживается масштабируемым набором виртуальных машин) в существующий кластер аналогичен [обновлению типа первичного узла](service-fabric-scale-up-node-type.md), за исключением того, что вы не будете использовать тот же NodeTypeRef; Очевидно, что не будет отключен ни один из активных масштабируемых наборов виртуальных машин, и доступность кластера не будет потеряна, если не обновить тип первичного узла. 

Свойство NodeTypeRef объявляется в свойствах расширения Service Fabric масштабируемого набора виртуальных машин.
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Кроме того, вам нужно будет добавить этот новый тип узла в ресурс кластера Service Fabric:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Следующие шаги
* Узнайте как [масштабировать тип первичного узла](service-fabric-scale-up-node-type.md).
* Дополнительные сведения об [обновлениях приложений](service-fabric-concepts-scalability.md).
* [Руководство. Масштабирование кластера Service Fabric](service-fabric-tutorial-scale-cluster.md)
* [Масштабируйте кластер Azure программно](service-fabric-cluster-programmatic-scaling.md), используя свободный пакет SDK для вычислений Azure.
* [Увеличение и уменьшение масштаба автономного кластера](service-fabric-cluster-windows-server-add-remove-nodes.md).

