---
title: Перемещение виртуальной машины Linux в Azure
description: Из этой статьи вы узнаете, как перенести виртуальную машину Linux в другую подписку или группу ресурсов Azure в рамках модели развертывания Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: 2ba8a8cdc324f46e25f9665cfce0aa07fc948e88
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75979040"
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Перемещение виртуальной машины Linux в другую подписку или группу ресурсов
В этой статье описано перемещение виртуальной машины Linux между группами ресурсов или подписками. Перемещение ВМ между подписками может понадобиться, если вы создали виртуальную машину в личной подписке и вам нужно переместить ее в корпоративную подписку.

> [!IMPORTANT]
>В настоящее время переместить управляемые диски Azure невозможно.
>
>Во время перемещения создаются новые идентификаторы ресурсов. После перемещения виртуальной машины вам нужно будет обновить средства и сценарии, чтобы использовать новые идентификаторы ресурсов.
>
>

## <a name="use-the-azure-cli-to-move-a-vm"></a>Использование Azure CLI для перемещения виртуальной машины


Перед перемещением виртуальной машины с помощью Azure CLI удостоверьтесь, что исходная и целевая подписки находятся в пределах одного клиента. Чтобы убедиться, что обе подписки имеют один и тот же идентификатор клиента, выполните команду [az account show](/cli/azure/account).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Если идентификаторы клиентов для исходной и целевой подписок не совпадают, необходимо обратиться в [службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) для перемещения ресурсов в новый клиент.

Для успешного перемещения виртуальной машины необходимо перенести саму виртуальную машину и все вспомогательные ресурсы. Чтобы вывести список всех ресурсов в группе и их идентификаторы, используйте команду [az resource list](/cli/azure/resource). Желательно сохранить выходные данные этой команды в файл, чтобы позже можно было копировать и вставлять идентификаторы в командах.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Чтобы переместить виртуальную машину и ее ресурсы в другую группу ресурсов, выполните команду [az resource move](/cli/azure/resource). Ниже показано, как переместить ВМ и наиболее распространенные ресурсы, которые для этого понадобятся. Используйте параметр **-ids** и передайте разделенный запятыми список (без пробелов) идентификаторов ресурсов для перемещения.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Если нужно переместить виртуальную машину и ее ресурсы в другую подписку, добавьте параметр **--destination-subscriptionId**, чтобы указать целевую подписку.

При появлении запроса на подтверждение перемещения указанных ресурсов введите **Да** для подтверждения.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Дальнейшие действия
Вы можете перемещать разные типы ресурсов между группами ресурсов и подписками. Дополнительные сведения см. в статье [Перемещение ресурсов в новую группу ресурсов или подписку](../../azure-resource-manager/management/move-resource-group-and-subscription.md).    
