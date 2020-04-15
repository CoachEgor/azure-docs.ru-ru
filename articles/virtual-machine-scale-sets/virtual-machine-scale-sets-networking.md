---
title: Сеть для масштабируемых наборов виртуальных машин Azure
description: Как узнать некоторые из более продвинутых сетевых свойств для наборов виртуальных машин Azure.
author: mimckitt
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 07/17/2017
ms.author: mimckitt
ms.openlocfilehash: 9f048c7d89da0ab75c321cd8e3932ea97c7ed09c
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310014"
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Сеть для масштабируемых наборов виртуальных машин Azure

При развертывании масштабируемого набора виртуальных машин Azure с помощью портала определенные свойства сети заданы по умолчанию, например Azure Load Balancer с правилами преобразования сетевых адресов для входящих подключений. В этой статье описывается, как использовать некоторые расширенные сетевые функции, которые можно настроить с помощью наборов масштабирования.

Вы можете настроить все функции, описанные в этой статье, с помощью шаблонов Azure Resource Manager. Кроме того, будут добавлены примеры Azure CLI и PowerShell для выбранных компонентов.

## <a name="accelerated-networking"></a>Ускорение работы в сети
Ускорение работы в сети Azure достигается за счет виртуализации ввода-вывода с единым корнем (SR-IOV) для виртуальной машины. Дополнительные сведения см. в разделах об ускорении работы в сети для виртуальных машин [Windows](../virtual-network/create-vm-accelerated-networking-powershell.md) или [Linux](../virtual-network/create-vm-accelerated-networking-cli.md). Чтобы использовать ускоренную сеть с масштабируемыми наборами, в настройках networkInterfaceConfigurations масштабируемого набора задайте для параметра enableAcceleratedNetworking значение **true**. Пример:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Создание набора масштабирования, который ссылается на существующую службу Azure Load Balancer
Если набор масштабирования создается с помощью портала Azure, для большинства параметров конфигурации создается новая подсистема балансировки нагрузки. Если вы создаете набор масштабирования, которому необходимо ссылаться на существующую подсистему балансировки нагрузки, вы можете создать его с помощью интерфейса командной строки. Следующий пример сценария создает подсистему балансировки нагрузки, а затем набор масштабирования, который ссылается на нее:

```azurecli
az network lb create \
    -g lbtest \
    -n mylb \
    --vnet-name myvnet \
    --subnet mysubnet \
    --public-ip-address-allocation Static \
    --backend-pool-name mybackendpool

az vmss create \
    -g lbtest \
    -n myvmss \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username negat \
    --ssh-key-value /home/myuser/.ssh/id_rsa.pub \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name myvnet \
    --subnet mysubnet \
    --lb mylb \
    --backend-pool-name mybackendpool
```

>[!NOTE]
> После создания набора масштабов порт бэкэнда не может быть изменен для правила балансировки нагрузки, используемого зондом работоспособности балансоилира нагрузки. Чтобы изменить порт, можно удалить зонд работоспособности, обновив набор виртуальной машины Azure, обновить порт, а затем настроить зонд работоспособности снова. 

Для получения дополнительной информации о балансе и виртуальных наборах масштабов машин [см.](../../articles/virtual-machines/windows/network-overview.md)

Следующие методы могут быть использованы для развертывания виртуального набора масштабов машин с существующим балансистом нагрузки Azure.

* [Настроили виртуальный набор масштабов машины с существующим балансом загрузки Azure с помощью портала Azure.](../../articles/load-balancer/configure-vm-scale-set-portal.md)
* [Настроили виртуальный набор масштабов машины с существующим балансом загрузки Azure с помощью Azure PowerShell.](../../articles/load-balancer/configure-vm-scale-set-powershell.md)
* [Настроили виртуальный набор масштабов машины с существующим балансом загрузки Azure с помощью Azure CLI.](../../articles/load-balancer/configure-vm-scale-set-cli.md)

## <a name="create-a-scale-set-that-references-an-application-gateway"></a>Создание масштабируемого набора, который ссылается на шлюз приложений
Чтобы создать масштабируемый набор, который использует шлюз приложений, создайте ссылку на серверный пул адресов шлюза приложений в разделе ipConfigurations в масштабируемом наборе, как в этой конфигурации шаблона ARM:

```json
"ipConfigurations": [{
  "name": "{config-name}",
  "properties": {
  "subnet": {
    "id": "{subnet-id}"
  },
  "ApplicationGatewayBackendAddressPools": [{
    "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/applicationGateways/{gateway-name}/backendAddressPools/{pool-name}"
  }]
}]
```

>[!NOTE]
> Обратите внимание, что шлюз приложений должен находиться в той же виртуальной сети, что и масштабируемый набор, но в другой подсети.


## <a name="configurable-dns-settings"></a>Настраиваемые параметры DNS
По умолчанию наборы масштабирования принимают определенные параметры DNS виртуальной сети и подсети, в которых они были созданы. Однако вы можете настроить параметры DNS непосредственно для набора масштабирования.

### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Создание масштабируемого набора с настраиваемыми DNS-серверами
Чтобы создать масштабируемый набор с настраиваемой конфигурацией DNS с помощью Azure CLI, добавьте в команду **vmss create** аргумент **--dns-servers**, за которым следуют разделенные пробелами IP-адреса серверов. Пример:

```bash
--dns-servers 10.0.0.6 10.0.0.5
```

Чтобы настроить пользовательские DNS-серверы в шаблоне Azure, добавьте свойство dnsSettings в раздел networkInterfaceConfigurations набора масштабирования. Пример:

```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Создание масштабируемого набора с настраиваемыми именами доменов виртуальных машин
Чтобы создать масштабируемый набор с настраиваемым DNS-именем для виртуальных машин с помощью CLI, добавьте в команду **virtual machine scale set create** аргумент **--vm-domain-name**, за которым следует строка, представляющая доменное имя.

Чтобы настроить доменное имя в шаблоне Azure, добавьте свойство **dnsSettings** в раздел масштабируемого набора **networkInterfaceConfigurations**. Пример:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": true,
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

Выходные данные для DNS-имени отдельной виртуальной машины будут представлены следующим образом:

```output
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a>Общедоступный IPv4 на каждую виртуальную машину
Как правило, виртуальным машинам в масштабируемом наборе Azure не требуются собственные общедоступные IP-адреса. Для большинства сценариев наиболее безопасно, а также экономически целесообразно связать общедоступный IP-адрес с подсистемой балансировки нагрузки или отдельной виртуальной машиной (также называемой jumpbox), которая затем (при необходимости) направит входящие подключения на виртуальные машины в масштабируемом наборе (например, через правила преобразования сетевых адресов для входящих подключений).

Тем не менее, в некоторых сценариях виртуальным машинам в масштабируемом наборе обязательно нужны собственные общедоступные IP-адреса. В качестве примера можно привести игры, в которых консоли необходимо установить прямое подключение к облачной виртуальной машине, выполняющей физическую обработку игр. Другим примером можно назвать случай, когда виртуальным машинам необходимо установить внешние подключения между собой в регионах в распределенной базе данных.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Создание масштабируемого набора с общедоступным IP-адресом на виртуальную машину
Чтобы создать масштабируемый набор, который назначает общедоступный IP-адрес каждой виртуальной машине с помощью CLI, добавьте параметр **--public-ip-per-vm** в команду **vmss create**. 

Чтобы создать масштабируемый набор с помощью шаблона Azure версии API ресурса Microsoft.Compute/virtualMachineScaleSets должна быть по крайней мере **30-03-2017**. Добавьте свойство JSON **publicIpAddressConfiguration** в раздел ipConfigurations масштабируемого набора. Пример:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```

Пример шаблона: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-the-public-ip-addresses-of-the-virtual-machines-in-a-scale-set"></a>Запрос общедоступных IP-адресов виртуальных машин в масштабируемом наборе
Чтобы получить список общедоступных IP-адресов, назначенных виртуальным машинам в масштабируемом наборе, с помощью CLI, используйте команду **az vmss list-instance-public-ips**.

Используйте команду _Get-AzPublicIpAddress_, чтобы вывести список общедоступных IP-адресов масштабируемого набора, используя PowerShell. Пример:

```powershell
Get-AzPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

Вы также можете запрашивать общедоступные IP-адреса, напрямую ссылаясь на идентификатор ресурса конфигурации общедоступного IP-адреса. Пример:

```powershell
Get-AzPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

Вы также можете отобразить общедоступные IP-адреса, назначенные виртуальным машинам в масштабируемом наборе, с помощью [обозревателя ресурсов Azure](https://resources.azure.com) или Azure REST API версии **2017-03-30** или выше.

Чтобы отправить запрос в [обозреватель ресурсов Azure](https://resources.azure.com), сделайте следующее.

1. В веб-браузере откройте [обозреватель ресурсов Azure](https://resources.azure.com).
1. В левой части окна разверните *подписки*, щелкнув *+* рядом с ними. Если в *подписках* всего один элемент, он может быть уже развернут.
1. Разверните подписки.
1. Разверните группу ресурсов.
1. Разверните *поставщики*.
1. Разверните *Microsoft.Compute*.
1. Разверните *virtualMachineScaleSets*.
1. Разверните масштабируемый набор.
1. Щелкните *publicipaddresses*.

Чтобы отправить запрос в Azure REST API, сделайте следующее:

```bash
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Пример выходных данных из [Azure Resource Explorer](https://resources.azure.com) и Azure REST API:

```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Несколько IP-адресов на сетевой адаптер
У каждого сетевого адаптера, подключенного к виртуальной машине в масштабируемом наборе, может быть одна или несколько конфигураций IP-адресов. Каждая конфигурация получает один частный IP-адрес. Кроме того, каждой конфигурации также может быть присвоен один ресурс общедоступного IP-адреса. Чтобы узнать, сколько IP-адресов можно назначить сетевому адаптеру, а также сколько общедоступных IP-адресов можно использовать в подписке Azure, ознакомьтесь с [ограничениями Azure](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Несколько сетевых адаптеров на виртуальную машину
У вам может быть до 8 сетевых адаптеров на виртуальную машину в зависимости от размера машины. Узнать о максимальном количестве сетевых адаптеров на машину вы можете в статье [Размеры виртуальных машин Windows в Azure](../virtual-machines/windows/sizes.md). Все сетевые адаптеры, подключенные к экземпляру виртуальной машины, должны быть подключены к одной виртуальной сети. Сетевые адаптеры могут подключаться к разным подсетям, но все подсети должны быть частью одной и той же виртуальной сети.

Пример ниже представляет сетевой профиль в масштабируемом наборе, отображающий несколько записей сетевых адаптеров, а также несколько общедоступных IP-адресов на виртуальную машину:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": true,
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": false,
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg--asgs-per-scale-set"></a>Группа безопасности сети и группы безопасности приложений на масштабируемый набор
[Группы безопасности сети](../virtual-network/security-overview.md) позволяют отфильтровать входящий и исходящий трафик ресурсов Azure в виртуальной сети Azure с помощью правил безопасности. [Группы безопасности приложений](../virtual-network/security-overview.md#application-security-groups) позволяют обеспечить сетевую безопасность ресурсов Azure и сгруппировать их как расширение в структуре приложения.

Группы безопасности сети можно применить непосредственно к масштабируемому набору, добавив ссылку в раздел конфигурации сетевого интерфейса свойств виртуальной машины в масштабируемом наборе.

Группы безопасности приложений можно также указать непосредственно для масштабируемого набора, добавив ссылку в раздел конфигурации IP-адреса для сетевого интерфейса свойств виртуальной машины в масштабируемом наборе.

Пример:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": true,
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            },
                            "applicationSecurityGroups": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/applicationSecurityGroups/', variables('asgName'))]"
                                }
                            ],
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

Чтобы подтвердить, что группа безопасности сети связана с масштабируемым набором, используйте команду `az vmss show`. В приведенном ниже примере используется `--query` для фильтрации результатов и отображается только соответствующая секция выходных данных.

```azurecli
az vmss show \
    -g myResourceGroup \
    -n myScaleSet \
    --query virtualMachineProfile.networkProfile.networkInterfaceConfigurations[].networkSecurityGroup

[
  {
    "id": "/subscriptions/.../resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/nsgName",
    "resourceGroup": "myResourceGroup"
  }
]
```

Чтобы убедиться, что группа безопасности приложений связана с масштабируемым набором, используйте команду `az vmss show`. В приведенном ниже примере используется `--query` для фильтрации результатов и отображается только соответствующая секция выходных данных.

```azurecli
az vmss show \
    -g myResourceGroup \
    -n myScaleSet \
    --query virtualMachineProfile.networkProfile.networkInterfaceConfigurations[].ipConfigurations[].applicationSecurityGroups

[
  [
    {
      "id": "/subscriptions/.../resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationSecurityGroups/asgName",
      "resourceGroup": "myResourceGroup"
    }
  ]
]
```



## <a name="next-steps"></a>Следующие шаги
Дополнительные сведения о виртуальных сетях Azure см. в [этой статье](../virtual-network/virtual-networks-overview.md).
