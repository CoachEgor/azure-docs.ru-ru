---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e368b1590f263618969423d57cdf0531fc2bb54d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121035"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>Изменение шлюза локальной сети gateway-ip-address

Если общедоступный IP-адрес VPN-устройства, к которому вы хотите подключиться, изменился, измените шлюз локальной сети в соответствии с изменениями. IP-адрес шлюза можно изменить, не удаляя существующее подключение VPN-шлюза (если имеется). Чтобы изменить IP-адрес шлюза, замените значения Site2 и TestRG1 собственными, используя команду [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway).

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Проверьте правильность IP-адреса в выходных данных:

```
"gatewayIpAddress": "23.99.222.170",
```
