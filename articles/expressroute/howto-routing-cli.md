---
title: 'Azure ExpressRoute: Настройка пиринга: CLI'
description: Эта статья поможет вам создать и подготовить частный пиринг, общедоступный пиринг или пиринг Microsoft для канала ExpressRoute. а также показано, как проверить состояние, обновить или удалить пиринги для канала.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 91a1b6cc877b31fbcef638e34d3147d3377ce85c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79476123"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-cli"></a>Создание и изменение пиринга для канала ExpressRoute с помощью интерфейса командной строки

В этой статье объясняется, как создать конфигурацию и пиринг для канала ExpressRoute и управлять ими в модели развертывания с помощью Resource Manager, используя интерфейс командной строки (CLI). Вы также сможете проверить состояние, обновить, удалить и отозвать пиринги для канала ExpressRoute. Если вы хотите использовать для работы с каналом другой метод, выберите подходящую статью из списка ниже.

> [!div class="op_single_selector"]
> * [Портал Azure](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Общедоступный пиринг](about-public-peering.md)
> * [Видео — частный пиринг](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Видео — пиринг Майкрософт](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (классическая модель)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Предварительные требования для настройки

* Перед началом работы установите последнюю версию команд интерфейса командной строки (версию 2.0 или более позднюю). Сведения об установке команд CLI см. в руководстве по [установке Azure](/cli/azure/install-azure-cli).
* Не забудьте изучить страницы с описанием [предварительных требований](expressroute-prerequisites.md), [требований к маршрутизации](expressroute-routing.md) и [рабочих процессов](expressroute-workflows.md), прежде чем приступать к настройке.
* Вам потребуется активный канал ExpressRoute. Приступая к работе, [создайте канал ExpressRoute](howto-circuit-cli.md) ; он должен быть затем включен на стороне поставщика услуг подключения. Для выполнения команд, описанных в статье, должен быть подготовлен и включен канал ExpressRoute.

Эти инструкции распространяются только на каналы от поставщиков, предоставляющих услуги подключения второго уровня. Если ваш поставщик услуг подключения предлагает услуги третьего уровня (обычно это IPVPN, например MPLS), то он возьмет на себя настройку маршрутизации и управление ею.

Вы можете настроить частный пиринг и пиринг Майкрософт для канала ExpressRoute (общедоступный пиринг Azure является устаревшим для новых каналов). Пиринг можно настроить в любом порядке. главное, выполнять их конфигурацию по очереди. Дополнительную информацию о доменах маршрутизации и пиринге см. в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md). Сведения об общедоступном пиринга см. в статье [общедоступный пиринг ExpressRoute](about-public-peering.md).

## <a name="microsoft-peering"></a><a name="msft"></a>Пиринг Майкрософт

Этот раздел поможет создать, получить, обновить и (или) удалить конфигурацию пиринга Майкрософт для канала ExpressRoute.

> [!IMPORTANT]
> Пиринг Майкрософт для каналов ExpressRoute, которые были настроены до 1 августа 2017 г., позволяет объявлять все префиксы служб, даже если фильтры маршрутов не определены. Пиринг Майкрософт для каналов ExpressRoute, настроенных 1 августа 2017 г. или позднее, не будет объявлять префиксы, пока к каналу не будет присоединен фильтр маршрутов. Дополнительные сведения см. в руководстве по [настройке фильтра маршрута для пиринга Майкрософт](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Создание пиринга Майкрософт

1. Установите последнюю версию Azure CLI. Используйте последнюю версию интерфейса командной строки Azure (CLI). Изучите [предварительные требования](expressroute-prerequisites.md) и [рабочие процессы](expressroute-workflows.md), прежде чем приступить к настройке.

   ```azurecli
   az login
   ```

   Выберите подписку, для которой будете создавать канал ExpressRoute.

   ```azurecli
   az account set --subscription "<subscription ID>"
   ```
2. Создайте канал ExpressRoute. Выполните инструкции по созданию [канала ExpressRoute](howto-circuit-cli.md). Поставщик услуг подключения должен подготовить его. Если поставщик услуг подключения оказывает услуги третьего уровня, он может включить для вас пиринг Майкрософт. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала. 

3. Убедитесь, что канал ExpressRoute подготовлен и включен. Используйте следующий пример:

   ```azurecli
   az network express-route list
   ```

   Ответ будет выглядеть примерно так:

   ```output
   "allowClassicOperations": false,
   "authorizations": [],
   "circuitProvisioningState": "Enabled",
   "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
   "gatewayManagerEtag": null,
   "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
   "location": "westus",
   "name": "MyCircuit",
   "peerings": [],
   "provisioningState": "Succeeded",
   "resourceGroup": "ExpressRouteResourceGroup",
   "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
   "serviceProviderNotes": null,
   "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
   },
   "serviceProviderProvisioningState": "Provisioned",
   "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
   },
   "tags": null,
   "type": "Microsoft.Network/expressRouteCircuits]
   ```

4. Настройте пиринг Майкрософт для этого канала. Перед началом работы убедитесь, что у вас есть следующие сведения.

   * Подсеть /30 для основной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4, принадлежащего вам и зарегистрированного в RIR/IRR.
   * Подсеть /30 для дополнительной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4, принадлежащего вам и зарегистрированного в RIR/IRR.
   * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
   * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS.
   * Объявленные префиксы: необходимо предоставить список всех префиксов, которые вы планируете объявить во время сеанса BGP. Допускаются только общедоступные префиксы IP-адресов. Если вы планируете отправить набор префиксов, их можно оформить в виде списка, разделенного запятыми. Эти префиксы должны быть зарегистрированы в RIR/IRR на ваше имя.
   * **Необязательно.** ASN клиента: для объявления префиксов, не зарегистрированных с номером AS для пиринга, можно указать номер AS, с которым они зарегистрированы.
   * Имя реестра маршрутизации: можно указать RIR/IRR, в котором зарегистрированы номер AS и префиксы.
   * **Необязательно.** Хэш MD5, если вы решите его использовать.

   Чтобы настроить пиринг Майкрософт для своего канала, выполните следующий пример кода:

   ```azurecli
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
   ```

### <a name="to-view-microsoft-peering-details"></a><a name="getmsft"></a>Просмотр сведений о пиринге Майкрософт

Для получения сведений о конфигурации можно использовать следующий пример кода:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```
> [!IMPORTANT]
> Корпорация Майкрософт проверяет, назначены ли вам указанные "объявленные общедоступные префиксы" и "одноранговый ASN" (или "клиент ASN") в реестре Интернет-маршрутизации. Если вы получаете общедоступные префиксы из другой сущности и если назначение не записано в реестре маршрутизации, автоматическая проверка не будет завершена и потребуется проверка вручную. Если автоматическая проверка завершается неудачно, в выходных данных вышеуказанной команды будет отображаться "Адвертиседпубликпрефиксесстате" как "Проверка требуется". 
> 
> Если вы видите сообщение "требуется проверка", собирайте документы, которые отображают открытые префиксы, в вашу организацию, используя сущность, указанную в качестве владельца префиксов в реестре маршрутизации, и отправьте эти документы для проверки вручную, открыв запрос в службу поддержки, как показано ниже. 
> 
>

Вы должны увидеть результат, аналогичный приведенному ниже.

```output
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="to-update-microsoft-peering-configuration"></a><a name="updatemsft"></a>Обновление конфигурации пиринга Майкрософт

Вы можете обновить любую часть конфигурации. Следующий пример кода изменяет объявленные для канала префиксы с 123.1.0.0/24 на 124.1.0.0/24:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="to-add-ipv6-microsoft-peering-settings-to-an-existing-ipv4-configuration"></a><a name="addIPv6msft"></a>Добавление параметров пиринга Майкрософт IPv6 к существующей конфигурации IPv4

```azurecli
az network express-route peering update -g ExpressRouteResourceGroup --circuit-name MyCircuit --peering-type MicrosoftPeering --ip-version ipv6 --primary-peer-subnet 2002:db00::/126 --secondary-peer-subnet 2003:db00::/126 --advertised-public-prefixes 2002:db00::/126
```

### <a name="to-delete-microsoft-peering"></a><a name="deletemsft"></a>Удаление пиринга Майкрософт

Для удаления конфигурации пиринга выполните следующий пример кода:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="azure-private-peering"></a><a name="private"></a>Частный пиринг Azure

Этот раздел поможет вам создать, получить, обновить и (или) удалить конфигурацию частного пиринга Azure для канала ExpressRoute.

### <a name="to-create-azure-private-peering"></a>Создание частного пиринга Azure

1. Установите последнюю версию Azure CLI. Используйте последнюю версию интерфейса командной строки (CLI) Azure.* Прежде чем начинать настройку, изучите [предварительные условия](expressroute-prerequisites.md) и [рабочие процессы](expressroute-workflows.md).

   ```azurecli
   az login
   ```

   Выберите подписку, необходимую для создания канала ExpressRoute

   ```azurecli
   az account set --subscription "<subscription ID>"
   ```
2. Создайте канал ExpressRoute. Выполните инструкции по созданию [канала ExpressRoute](howto-circuit-cli.md). Поставщик услуг подключения должен подготовить его. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить для вас частный пиринг Azure. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала.

3. Убедитесь, что канал ExpressRoute подготовлен и включен. Используйте следующий пример:

   ```azurecli
   az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
   ```

   Ответ будет выглядеть примерно так:

   ```output
   "allowClassicOperations": false,
   "authorizations": [],
   "circuitProvisioningState": "Enabled",
   "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
   "gatewayManagerEtag": null,
   "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
   "location": "westus",
   "name": "MyCircuit",
   "peerings": [],
   "provisioningState": "Succeeded",
   "resourceGroup": "ExpressRouteResourceGroup",
   "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
   "serviceProviderNotes": null,
   "serviceProviderProperties": {
   "bandwidthInMbps": 200,
   "peeringLocation": "Silicon Valley",
   "serviceProviderName": "Equinix"
   },
   "serviceProviderProvisioningState": "Provisioned",
   "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
   },
   "tags": null,
   "type": "Microsoft.Network/expressRouteCircuits]
   ```

4. Настройте для канала частный пиринг Azure. Прежде чем продолжить, убедитесь в наличии следующих элементов:

   * Подсеть /30 для основной ссылки. Эта подсеть не должна входить в адресное пространство, зарезервированное для виртуальных сетей.
   * Подсеть /30 для дополнительной ссылки. Эта подсеть не должна входить в адресное пространство, зарезервированное для виртуальных сетей.
   * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
   * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS. Для этого пиринга можно использовать частный номер AS. Не используйте номер 65515.
   * **Необязательно.** Хэш MD5, если вы решите его использовать.

   Чтобы настроить для канала частный пиринг Azure, выполните код из следующего примера:

   ```azurecli
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
   ```

   Если вы решили использовать MD5-хэш, используйте следующий пример:

   ```azurecli
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
   ```

   > [!IMPORTANT]
   > Номер AS должен быть указан в качестве ASN пиринга, а не ASN клиента.
   > 
   > 

### <a name="to-view-azure-private-peering-details"></a><a name="getprivate"></a>Просмотр сведений о частном пиринге Azure

Для получения сведений о конфигурации можно использовать следующий пример кода:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

Вы должны увидеть результат, аналогичный приведенному ниже.

```output
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="to-update-azure-private-peering-configuration"></a><a name="updateprivate"></a>Обновление конфигурации частного пиринга Azure

С помощью следующего примера можно обновить любую часть конфигурации. В приведенном ниже примере значение идентификатора виртуальной локальной сети для канала изменяется со 100 на 500.

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="to-delete-azure-private-peering"></a><a name="deleteprivate"></a>Удаление частного пиринга Azure

Для удаления конфигурации пиринга выполните следующий пример кода:

> [!WARNING]
> Перед запуском этого примера обязательно убедитесь, что все виртуальные сети и подключения Global Reach к ExpressRoute удалены. 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```


## <a name="next-steps"></a>Дальнейшие шаги

Следующий шаг — [связывание виртуальной сети с каналом ExpressRoute](howto-linkvnet-cli.md).

* Дополнительную информацию о рабочих процессах ExpressRoute см. в статье [Процедуры ExpressRoute для подготовки каналов и состояний каналов](expressroute-workflows.md).
* Дополнительную информацию о пиринге канала см. в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).
* Подробнее о работе с виртуальными сетями см. в статье [Обзор виртуальных сетей](../virtual-network/virtual-networks-overview.md).
