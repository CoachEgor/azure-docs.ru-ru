---
title: Включить имя файла
description: включить файл
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/15/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6a90b23c10e08e8b14a18f9619cff5aaeb003cab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "76045700"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress---no-gateway-connection"></a><a name="gwipnoconnection"></a> Изменение шлюза локальной сети с использованием GatewayIpAddress при отсутствии подключения к шлюзу

Если общедоступный IP-адрес VPN-устройства, к которому вы хотите подключиться, изменился, измените шлюз локальной сети в соответствии с изменениями. Используйте пример ниже, чтобы изменить шлюз локальной сети, к которому нет подключения.

При изменении этого значения вы также можете изменить префиксы адресов. Не забудьте указать имеющееся имя шлюза локальной сети для перезаписи текущих параметров. Если используется другое имя, необходимо создать новый шлюз локальной сети вместо перезаписи существующего.

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="to-modify-the-local-network-gateway-gatewayipaddress---existing-gateway-connection"></a><a name="gwipwithconnection"></a> Изменение шлюза локальной сети с использованием GatewayIpAddress, если подключение к шлюзу установлено

Если общедоступный IP-адрес VPN-устройства, к которому вы хотите подключиться, изменился, измените шлюз локальной сети в соответствии с изменениями. Если шлюз уже подключен, сначала вам нужно удалить подключение. Затем вы сможете изменить IP-адрес шлюза и создать новое подключение. При этом вы также можете изменить префиксы адресов. После этого VPN-подключение будет некоторое время недоступно. Не удаляйте VPN-шлюз при изменении IP-адреса шлюза. Необходимо удалить только подключение.
 

1. Удалите подключение. Вы можете найти имя своего подключения с помощью командлета Get-AzVirtualNetworkGatewayConnection.

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1
   ```
2. Измените значение GatewayIpAddress. При этом вы также можете изменить префиксы адресов. Не забудьте указать существующее имя шлюза локальной сети для перезаписи текущих параметров. Иначе вы создадите новый шлюз локальной сети, а не перезапишете существующий.

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name Site1 `
   -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
   -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
   ```
3. Создайте подключение. В этом примере мы настраиваем тип подключения IPsec. При повторном создании подключения используйте тип соединения, указанный для вашей конфигурации. Дополнительные типы подключений см. на странице с [командлетами PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).  Чтобы получить имя VirtualNetworkGateway, запустите командлет Get-AzVirtualNetworkGateway.
   
    Задайте переменные.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1

   $vnetgw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
   ```
   
    Создайте подключение.

   ```azurepowershell-interactive 
   New-AzVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
   -Location "East US" `
   -VirtualNetworkGateway1 $vnetgw `
   -LocalNetworkGateway2 $local `
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```