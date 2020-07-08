---
title: 'Azure ExpressRoute: Настройка Global Reach ExpressRoute: CLI'
description: В этой статье содержатся сведения о том, как связать каналы ExpressRoute, чтобы настроить частную сеть между локальными сетями и включить Global Reach.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: how-to
ms.date: 12/12/2018
ms.author: jaredro
ms.openlocfilehash: df02213f1cba7c25cc77c06f1addcc04a2a4d27a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "84738402"
---
# <a name="configure-expressroute-global-reach-by-using-the-azure-cli"></a>Настройка Global Reach ExpressRoute с помощью Azure CLI

Эта статья поможет вам настроить Azure ExpressRoute Global Reach путем использования Azure CLI. См. дополнительные сведения об [ExpressRoute Global Reach](expressroute-global-reach.md).
 
Перед началом настройки выполните следующие требования.

* Установите последнюю версию Azure CLI. Дополнительные сведения см. в руководстве по [установке Azure CLI](/cli/azure/install-azure-cli), а также в руководстве по [началу работы с Azure CLI](/cli/azure/get-started-with-azure-cli).
* Ознакомьтесь с [рабочими процессами](expressroute-workflows.md) подготовки канала ExpressRoute.
* Убедитесь, что каналы ExpressRoute подготовлены.
* Убедитесь, что частный пиринг Azure настроен для каналов ExpressRoute.  

### <a name="sign-in-to-your-azure-account"></a>Вход в учетную запись Azure

Чтобы начать настройку, войдите в свою учетную запись Azure. Следующая команда открывает браузер по умолчанию, в котором отображается запрос на введение учетных данных для входа в учетную запись Azure.  

```azurecli
az login
```

Если у вас есть несколько подписок Azure, проверьте подписки для этой учетной записи.

```azurecli
az account list
```

Укажите подписку, которую нужно использовать.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Определение каналов ExpressRoute для конфигурации

Можно включить ExpressRoute Global Reach между любыми двумя каналами ExpressRoute, если они находятся в поддерживаемых странах и регионах и были созданы в различных расположениях пиринга. Если подписке принадлежат оба канала, вы можете выбрать канал для выполнения конфигурации, как описано далее в этой статье. Если два канала находятся в разных подписках Azure, вам необходимо будет выполнить авторизацию из одной подписки Azure и передать ключ авторизации при выполнении команды конфигурации в другой подписке Azure.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Настройка подключения между локальными сетями

При выполнении команды для настройки подключения следует учитывать следующие требования для значений параметров:

* Параметр *peer-circuit* должен представлять полный идентификатор ресурса. Пример:

  > /subscriptions/{идентификатор_вашей_подписки}/resourceGroups/{ваша_группа_ресурсов}/providers/Microsoft.Network/expressRouteCircuits/{имя_вашего_канала}

* *префикс адреса* должен быть подсетью IPv4 "/29" (например, "10.0.0.0/29"). Мы используем IP-адреса в этой подсети, чтобы установить подключение между двумя каналами ExpressRoute. Не следует использовать адреса в этой подсети в виртуальных сетях Azure или в локальных сетях.

Запустите следующую команду CLI для подключения двух каналов ExpressRoute.

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

Выходные данные CLI выглядят следующим образом:

```output
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

После завершения этой операции должно появиться подключение между локальными сетями с обеих сторон через два канала ExpressRoute.

## <a name="enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions"></a>Настройка подключения между каналами ExpressRoute в разных подписках Azure

Если два канала находятся в разных подписках Azure, вам потребуется выполнить авторизацию. В следующей конфигурации вы создадите авторизацию в подписке канала 2 и передадите ключ авторизации в канал 1.

1. Создайте ключ авторизации.

   ```azurecli
   az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
   ```

   Выходные данные CLI выглядят следующим образом:

   ```output
   {
     "authorizationKey": "<authorizationKey>",
     "authorizationUseStatus": "Available",
     "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
     "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
     "name": "<AuthorizationName>",
     "provisioningState": "Succeeded",
     "resourceGroup": "<Circuit2ResourceGroupName>",
     "type": "Microsoft.Network/expressRouteCircuits/authorizations"
   }
   ```

1. Запишите идентификатор ресурса и ключ авторизации для канала 2.

1. Выполните следующую команду для канала 1, передав идентификатор ресурса канала 2 и ключ авторизации.

   ```azurecli
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

После завершения этой операции должно появиться подключение между локальными сетями с обеих сторон через два канала ExpressRoute.

## <a name="get-and-verify-the-configuration"></a>Получение и проверка конфигурации

Используйте следующую команду для проверки конфигурации на канале, на котором она создана (в приведенном выше примере это канал 1).

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

В выходных данных CLI вы увидите *CircuitConnectionStatus*. Так вы узнаете, установлено ли подключение между двумя каналами: на основе состояния Connected (Подключено) или Disconnected (Отключено). 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Отключение подключения между локальными сетями

Чтобы отключить подключение, выполните следующую команду на канале, на котором создана конфигурация (в предыдущем примере это канал 1).

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Используйте команду ```show```, чтобы проверить состояние.

После завершения этой операции вы потеряете подключение к локальным сетям через каналы ExpressRoute.

## <a name="next-steps"></a>Дальнейшие шаги

* [Получение дополнительных сведений об ExpressRoute Global Reach](expressroute-global-reach.md).
* [Проверка подключения ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Подключение виртуальной сети к каналу ExpressRoute](expressroute-howto-linkvnet-arm.md)
