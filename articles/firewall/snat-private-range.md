---
title: Диапазоны частных IP-адресов SNAT в брандмауэре Azure
description: Можно настроить диапазоны IP-адресов для SNAT.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 11/16/2020
ms.author: victorh
ms.openlocfilehash: c5613dda7adbbc47f989bc2a772777e716620b3c
ms.sourcegitcommit: fa807e40d729bf066b9b81c76a0e8c5b1c03b536
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "97348039"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Диапазоны частных IP-адресов SNAT в брандмауэре Azure

Брандмауэр Azure предоставляет автоматический режим SNAT для всего исходящего трафика на общедоступные IP-адреса. По умолчанию брандмауэр Azure не поддерживает SNAT с правилами сети, если IP-адрес назначения находится в диапазоне частных IP-адресов на [IANA RFC 1918](https://tools.ietf.org/html/rfc1918). Правила приложений всегда применяются с использованием [прозрачного прокси-сервера](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) , независимо от IP-адреса назначения.

Эта логика хорошо работает при маршрутизации трафика непосредственно в Интернет. Однако если вы включили [принудительное туннелирование](forced-tunneling.md), трафик, привязанный к Интернету, снатед на один из частных IP-адресов брандмауэра в азурефиреваллсубнет, скрывая источник из локального брандмауэра.

Если для частных сетей в организации используются общедоступные IP-адреса, брандмауэр Azure использует SNAT трафика для одного из своих частных IP-адресов AzureFirewallSubnet. Однако вы можете настроить брандмауэр Azure так, чтобы он **не** был ограничен диапазоном общедоступных IP-адресов. Например, чтобы указать отдельный IP-адрес, можно указать его следующим образом: `192.168.1.10` . Чтобы указать диапазон IP-адресов, можно указать его следующим образом: `192.168.1.0/24` .

- Чтобы настроить в брандмауэре Azure параметр " **никогда не** использовать SNAT" независимо от IP-адреса назначения, используйте **0.0.0.0/0** в качестве диапазона частных IP-адресов. В этой конфигурации брандмауэр Azure не может маршрутизировать трафик напрямую в Интернет. 

- Чтобы настроить в брандмауэре значение **Always** SNAT независимо от адреса назначения, используйте адрес **255.255.255.255/32** в качестве диапазона частных IP-адресов.

> [!IMPORTANT]
> Если вы хотите указать собственные диапазоны частных IP-адресов и сохранить диапазоны адресов IANA RFC 1918 по умолчанию, убедитесь, что пользовательский список по-прежнему содержит диапазон IANA RFC 1918. 

## <a name="configure-snat-private-ip-address-ranges---azure-powershell"></a>Настройка диапазонов частных IP-адресов SNAT — Azure PowerShell

Чтобы указать диапазоны частных IP-адресов для брандмауэра, можно использовать Azure PowerShell.

### <a name="new-firewall"></a>Новый брандмауэр

Для нового брандмауэра командлетом Azure PowerShell является:

```azurepowershell
$azFw = @{
    Name               = '<fw-name>'
    ResourceGroupName  = '<resourcegroup-name>'
    Location           = '<location>'
    VirtualNetworkName = '<vnet-name>'
    PublicIpName       = '<public-ip-name>'
    PrivateRange       = @("IANAPrivateRanges", "192.168.1.0/24", "192.168.1.10")
}

New-AzFirewall @azFw
```
> [!NOTE]
> Для развертывания брандмауэра Azure с помощью `New-AzFirewall` требуется существующая виртуальная сеть и общедоступный IP-адрес. Полное пошаговое развертывание см. в статье [развертывание и настройка брандмауэра Azure с помощью Azure PowerShell](deploy-ps.md) .

> [!NOTE]
> Ианаприватеранжес расширяется до текущих значений по умолчанию в брандмауэре Azure, в то время как к нему добавляются другие диапазоны. Чтобы сохранить значение Ианаприватеранжес по умолчанию в спецификации частного диапазона, оно должно остаться в `PrivateRange` спецификации, как показано в следующих примерах.

Дополнительные сведения см. в разделе [New-азфиревалл](/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Существующий брандмауэр

Чтобы настроить существующий брандмауэр, используйте следующие командлеты Azure PowerShell.

```azurepowershell
$azfw = Get-AzFirewall -Name '<fw-name>' -ResourceGroupName '<resourcegroup-name>'
$azfw.PrivateRange = @("IANAPrivateRanges","192.168.1.0/24", "192.168.1.10")
Set-AzFirewall -AzureFirewall $azfw
```

## <a name="configure-snat-private-ip-address-ranges---azure-cli"></a>Настройка диапазонов частных IP-адресов SNAT — Azure CLI

Чтобы указать диапазоны частных IP-адресов для брандмауэра, можно использовать Azure CLI.

### <a name="new-firewall"></a>Новый брандмауэр

Для нового брандмауэра используется команда Azure CLI:

```azurecli-interactive
az network firewall create \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

> [!NOTE]
> Для развертывания брандмауэра Azure с помощью команды Azure CLI `az network firewall create` требуются дополнительные действия по настройке для создания общедоступных IP-адресов и IP-конфигурации. Полное пошаговое развертывание см. в статье [развертывание и настройка брандмауэра Azure с помощью Azure CLI](deploy-cli.md) .

> [!NOTE]
> Ианаприватеранжес расширяется до текущих значений по умолчанию в брандмауэре Azure, в то время как к нему добавляются другие диапазоны. Чтобы сохранить значение Ианаприватеранжес по умолчанию в спецификации частного диапазона, оно должно остаться в `PrivateRange` спецификации, как показано в следующих примерах.

### <a name="existing-firewall"></a>Существующий брандмауэр

Для настройки существующего брандмауэра используется команда Azure CLI:

```azurecli-interactive
az network firewall update \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

## <a name="configure-snat-private-ip-address-ranges---arm-template"></a>Настройка диапазонов частных IP-адресов SNAT — шаблон ARM

Чтобы настроить SNAT во время Шаблоны развертывания ARM, можно добавить в `additionalProperties` свойство следующее:

```json
"additionalProperties": {
   "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
},
```

## <a name="configure-snat-private-ip-address-ranges---azure-portal"></a>Настройка диапазонов частных IP-адресов SNAT — портал Azure

Вы можете использовать портал Azure, чтобы указать диапазоны частных IP-адресов для брандмауэра.

1. Выберите группу ресурсов, а затем выберите свой брандмауэр.
2. На странице **Обзор** в разделе **диапазоны частных IP-адресов** выберите значение по умолчанию **IANA RFC 1918**.

   Откроется страница **Изменение префикса частного IP-адреса** :

   :::image type="content" source="media/snat-private-range/private-ip.png" alt-text="Изменить частные IP-префиксы":::

1. По умолчанию **ианаприватеранжес** настроен.
2. Измените диапазоны частных IP-адресов для своей среды и нажмите кнопку **сохранить**.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [принудительном туннелировании в брандмауэре Azure](forced-tunneling.md).