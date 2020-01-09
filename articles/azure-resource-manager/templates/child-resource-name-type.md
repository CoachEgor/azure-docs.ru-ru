---
title: Дочерние ресурсы в шаблонах
description: Описывает, как задать имя и тип для дочерних ресурсов в шаблоне Azure Resource Manager.
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: 058c28329942a1bd2e5d0e12321022fb022ef74f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75479609"
---
# <a name="set-name-and-type-for-child-resources"></a>Задать имя и тип для дочерних ресурсов

Дочерние ресурсы — это ресурсы, которые существуют только в контексте другого ресурса. Например, [расширение виртуальной машины](/azure/templates/microsoft.compute/2019-03-01/virtualmachines/extensions) не может существовать без [виртуальной машины](/azure/templates/microsoft.compute/2019-03-01/virtualmachines). Ресурс расширения является дочерним для виртуальной машины.

В шаблоне диспетчера ресурсов можно указать дочерний ресурс либо в родительском ресурсе, либо вне родительского ресурса. В следующем примере показан дочерний ресурс, входящий в свойство Resources родительского ресурса.

```json
"resources": [
  {
    <parent-resource>
    "resources": [
      <child-resource>
    ]
  }
]
```

В следующем примере показан дочерний ресурс за пределами родительского ресурса. Этот подход можно использовать, если родительский ресурс не развертывается в том же шаблоне или если требуется использовать [Copy](create-multiple-instances.md) для создания более чем одного дочернего ресурса.

```json
"resources": [
  {
    <parent-resource>
  },
  {
    <child-resource>
  }
]
```

Значения, указанные для имени ресурса и типа, зависят от того, определен ли дочерний ресурс внутри или за пределами родительского ресурса.

## <a name="within-parent-resource"></a>В родительском ресурсе

При определении в родительском типе ресурса значения типа и имени отформатированы как одно слово без косой черты.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

В следующем примере показана виртуальная сеть и подсеть. Обратите внимание, что подсеть включена в массив ресурсов для виртуальной сети. Для имени задано значение **Subnet1** , а для типа — **подсети**. Дочерний ресурс помечается как зависимый от родительского ресурса, так как родительский ресурс должен существовать до развертывания дочернего ресурса.

```json
"resources": [
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    },
    "resources": [
      {
        "apiVersion": "2018-10-01",
        "type": "subnets",
        "location": "[parameters('location')]",
        "name": "Subnet1",
        "dependsOn": [
          "VNet1"
        ],
        "properties": {
          "addressPrefix": "10.0.0.0/24"
        }
      }
    ]
  }
]
```

Полный тип ресурсов по-прежнему — **Microsoft. Network/virtualNetworks/подсети**. Вы не предоставляете **Microsoft. Network/virtualNetworks/** , так как предполагается тип родительского ресурса.

Имя дочернего ресурса имеет значение **Subnet1** , а полное имя включает имя родительского объекта. Вы не предоставляете **VNet1** , так как он предполагается из родительского ресурса.

## <a name="outside-parent-resource"></a>Внешний родительский ресурс

При определении за пределами родительского ресурса необходимо отформатировать тип и с помощью косой черты включить родительский тип и имя.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

В следующем примере показана виртуальная сеть и подсеть, которые определены на корневом уровне. Обратите внимание, что подсеть не входит в массив ресурсов для виртуальной сети. Для имени задано значение **VNet1/Subnet1** , а для типа — значение **Microsoft. Network/virtualNetworks/подсети**. Дочерний ресурс помечается как зависимый от родительского ресурса, так как родительский ресурс должен существовать до развертывания дочернего ресурса.

```json
"resources": [
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    }
  },
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks/subnets",
    "location": "[parameters('location')]",
    "name": "VNet1/Subnet1",
    "dependsOn": [
      "VNet1"
    ],
    "properties": {
      "addressPrefix": "10.0.0.0/24"
    }
  }
]
```

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о создании шаблонов диспетчера ресурсов Azure см. в статье о [создании шаблонов](template-syntax.md). 

* Дополнительные сведения о формате имени ресурса при ссылке на ресурс см. в описании [функции Reference](template-functions-resource.md#reference).
