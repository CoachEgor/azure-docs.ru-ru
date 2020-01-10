---
title: Создание избыточного между зонами шлюза виртуальной сети в Зонах доступности Azure
description: Развертывание VPN-шлюза и шлюзов ExpressRoute в Зонах доступности Azure
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 04/26/2019
ms.author: cherylmc
ms.openlocfilehash: 250ced13696d6ec34e7c434b26a2917a3c55e91d
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834636"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Создание избыточного между зонами шлюза виртуальной сети в Зонах доступности Azure

Вы можете развернуть VPN-шлюзы и шлюзы ExpressRoute в Зонах доступности Azure. В результате шлюзы виртуальной сети смогут работать на более высоких уровнях отказоустойчивости, масштабируемости и доступности. При развертывании в зонах доступности Azure происходит физическое и логическое разделение шлюзов в пределах региона с одновременной защитой локального сетевого подключения к Azure от сбоев на уровне зоны. Дополнительные сведения см. в статье [об избыточных между зонами шлюзах виртуальной сети](about-zone-redundant-vnet-gateways.md) и статье [Что такое Зоны доступности в Azure?](../availability-zones/az-overview.md).

## <a name="before-you-begin"></a>Перед началом работы

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Можно использовать оболочку PowerShell, установленную локально на компьютере, или Azure Cloud Shell. Чтобы установить и использовать PowerShell локально, вам понадобится модуль PowerShell последней версии.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Локальное использование PowerShell

Если вместо Cloud Shell вы используете PowerShell локально на компьютере, необходимо установить модуль PowerShell 1.0.0 или последующих версий. Чтобы узнать, какая версия PowerShell установлена, используйте приведенную ниже команду.

```azurepowershell
Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
```

Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="variables"></a>1. Объявите переменные

Ниже перечислены значения, применяемые для выполняемых в примере действий. Кроме того, на шагах в некоторых примерах используются объявленные переменные. Если вы следуете эти шагам в собственной среде, необходимо заменить эти значения собственными. При указании расположения проверьте, поддерживается ли задаваемый регион. Дополнительные сведения см. в разделе [Часто задаваемые вопросы](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>2. Создайте виртуальную сеть.

Создайте группу ресурсов.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Создайте виртуальную сеть.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>3. Добавление подсети шлюза

Подсеть шлюза содержит зарезервированные IP-адреса, используемые службами шлюза виртуальной сети. Используйте приведенные далее примеры для добавления и установки подсети шлюза.

Добавьте подсеть шлюза.

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Установка конфигурации подсети шлюза для виртуальной сети.

```azurepowershell-interactive
$getvnet | Set-AzVirtualNetwork
```
## <a name="publicip"></a>4. запрос общедоступного IP-адреса
 
На этом шаге выберите инструкции, которые применяются к создаваемому шлюзу. Выбор зон для развертывания шлюзов зависит от зон, указанных для общедоступных IP-адресов.

### <a name="ipzoneredundant"></a>Для шлюзов, избыточных между зонами

Запросите общедоступный IP-адрес с номером SKU PublicIpaddress типа **Стандартный** и не указывайте никакую зону. В этом случае созданный стандартный общедоступный IP-адрес будет общедоступным IP-адресом, избыточным между зонами.   

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Для зональных шлюзов

Запросите общедоступный IP-адрес с номером SKU PublicIpaddress типа **Стандартный**. Укажите зону (1, 2 или 3). Все экземпляры шлюза будут развернуты в этой зоне.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Для региональных шлюзов

Запросите общедоступный IP-адрес с номером SKU PublicIpaddress типа **Базовый**. В этом случае шлюз развертывается как региональный и не имеет встроенной избыточности в пределах зоны. Экземпляры шлюза создаются в любых зонах.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>5. Создание IP-конфигурации

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>6. Создание шлюза

Создайте шлюз виртуальной сети.

### <a name="for-expressroute"></a>Для шлюза ExpressRoute

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute -GatewaySku ErGw1AZ
```

### <a name="for-vpn-gateway"></a>Для VPN-шлюза

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1AZ
```

## <a name="faq"></a>Часто задаваемые вопросы

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Что изменится после развертывания этих новых номеров SKU?

Вы можете развертывать шлюзы с избыточностью в пределах зоны. Это означает, что все экземпляры шлюзов будут развернуты в зонах доступности Azure, а каждая зона доступности является сочетанием домена сбоя и домена обновления. В результате шлюзы становятся более надежными, доступными и устойчивыми к сбоям на уровне зоны.

### <a name="can-i-use-the-azure-portal"></a>Я могу использовать портал Azure?

Да, вы можете использовать портал Azure для развертывания новых номеров SKU. Но эти новые номера SKU доступны только для тех регионов Azure, в которых есть Зоны доступности Azure.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>В каких регионах доступны новые номера SKU?

Последний список доступных регионов см. в [зоны доступности](../availability-zones/az-overview.md#services-support-by-region) .

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Можно ли обновить существующие шлюзы виртуальной сети до шлюзов, избыточных между зонами, или зональных шлюзов?

Сейчас миграция существующих шлюзов виртуальной сети на избыточные между зонами или зональные шлюзы не поддерживается. Однако вы можете удалить существующий шлюз и повторно создать избыточный между зонами или зональный шлюз.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Можно ли развертывать VPN-шлюзы и шлюзы Express Route в одной виртуальной сети?

Допускается одновременное функционирование VPN-шлюзов и шлюзов Express Route в одной виртуальной сети. Но при этом необходимо зарезервировать диапазон IP-адресов /27 для подсети шлюза.
