---
title: 'Настройка пиринга для канала — ExpressRoute: PowerShell: Azure | Документация Майкрософт'
description: В этой статье описана процедура создания и подготовки частного пиринга, общедоступного пиринга и пиринга Microsoft для канала ExpressRoute, а также показано, как проверить состояние, обновить или удалить пиринги для канала.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: 813263442bc82254d0cb5ea9e9f7e8a265de5b4a
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748205"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a>Создание и изменение пиринга для канала ExpressRoute с помощью PowerShell

В этой статье описано, как создать конфигурацию маршрутизации ExpressRoute и управлять ею в модели развертывания Azure Resource Manager, используя PowerShell. Вы также сможете проверить состояние, обновить, удалить и отозвать пиринги для канала ExpressRoute. Если вы хотите использовать для работы с каналом другой метод, выберите подходящую статью из списка ниже.

> [!div class="op_single_selector"]
> * [портал Azure](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Интерфейс командной строки Azure](howto-routing-cli.md)
> * [Видео — частный пиринг](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Видео — общедоступный пиринг](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Видео — пиринг Майкрософт](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (классическая модель)](expressroute-howto-routing-classic.md)
> 


Эти инструкции распространяются только на каналы от поставщиков, предоставляющих услуги подключения второго уровня. Если ваш поставщик услуг подключения предлагает услуги третьего уровня (обычно это IPVPN, например MPLS), то он возьмет на себя настройку маршрутизации и управление ею.

> [!IMPORTANT]
> Мы пока не предлагаем использовать пиринги, которые настроены поставщиками услуг на портале управления службами. Такая возможность будет предоставлена позже. Перед настройкой пирингов BGP уточните информацию у поставщика услуг.
> 
> 

Для каждого канала ExpressRoute можно настроить один, два или три пиринга (частный пиринг Azure, общедоступный пиринг Azure и пиринг Microsoft). Пиринги можно настраивать в любом порядке, главное, выполнять их конфигурацию по очереди. Дополнительную информацию о доменах маршрутизации и пиринге см. в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).

## <a name="configuration-prerequisites"></a>Предварительные требования для настройки

* Прежде чем приступить к настройке, обязательно изучите [предварительные требования](expressroute-prerequisites.md), [требования к маршрутизации](expressroute-routing.md) и [рабочие процессы](expressroute-workflows.md).
* Вам потребуется активный канал ExpressRoute. Приступая к работе, [создайте канал ExpressRoute](expressroute-howto-circuit-arm.md) ; он должен быть затем включен на стороне поставщика услуг подключения. Для выполнения командлетов, описанных в статье, нужно подготовить и включить канал ExpressRoute.

### <a name="working-with-azure-powershell"></a>Работа с Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="msft"></a>Пиринг Майкрософт

Этот раздел поможет создать, получить, обновить и (или) удалить конфигурацию пиринга Майкрософт для канала ExpressRoute.

> [!IMPORTANT]
> Пиринг Майкрософт для каналов ExpressRoute, которые были настроены до 1 августа 2017 г., позволяет объявлять все префиксы служб, даже если фильтры маршрутов не определены. Пиринг Майкрософт для каналов ExpressRoute, настроенных 1 августа 2017 г. или позднее, не будет объявлять префиксы, пока к каналу не будет присоединен фильтр маршрутов. Дополнительные сведения см. в руководстве по [настройке фильтра маршрута для пиринга Майкрософт](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Создание пиринга Майкрософт

1. Выполните вход и выберите подписку.

   Если вы установили PowerShell на локальном компьютере, выполните вход. Если вы используете Azure Cloud Shell, этот шаг можно пропустить.

   ```azurepowershell
   Connect-AzAccount
   ```

   Выберите подписку для создания канала ExpressRoute.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionId "<subscription ID>"
   ```
2. Создайте канал ExpressRoute.

   Выполните инструкции по созданию [канала ExpressRoute](expressroute-howto-circuit-arm.md). Поставщик услуг подключения должен подготовить его. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить для вас пиринг Майкрософт. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала. 

3. Убедитесь, что канал ExpressRoute подготовлен и включен. Используйте следующий пример:

   ```azurepowershell-interactive
   Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```

   Ответ будет выглядеть примерно так:

   ```
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"################################"
   ProvisioningState                : Succeeded
   Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
   CircuitProvisioningState         : Enabled
   ServiceProviderProvisioningState : Provisioned
   ServiceProviderNotes             : 
   ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
   ServiceKey                       : **************************************
   Peerings                         : []
   ```
4. Настройте пиринг Майкрософт для этого канала. Перед началом работы убедитесь, что у вас есть следующие сведения.

   * Подсеть /30 или /126 для основной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4 или IPv6, принадлежащего вам и зарегистрированного в RIR/IRR.
   * Подсеть /30 или /126 для дополнительной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4 или IPv6, принадлежащего вам и зарегистрированного в RIR/IRR.
   * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
   * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS.
   * Объявленные префиксы: необходимо предоставить список всех префиксов, которые вы планируете объявить во время сеанса BGP. Допускаются только общедоступные префиксы IP-адресов. Если вы планируете отправить набор префиксов, их можно оформить в виде списка, разделенного запятыми. Эти префиксы должны быть зарегистрированы в RIR/IRR на ваше имя. Для сеансов BGP IPv4 требуются объявленные префиксы IPv4, а для сеансов BGP IPv6 — объявленные префиксы IPv6. 
   * Имя реестра маршрутизации: можно указать RIR/IRR, в котором зарегистрированы номер AS и префиксы.
   * Необязательно:
     * Клиент ASN: для объявления префиксов, не зарегистрированных с номером AS для пиринга, можно указать номер AS, с которым они зарегистрированы.
     * Хэш MD5, если вы решите его использовать.

> [!IMPORTANT]
> Корпорация Майкрософт проверяет, назначены ли вам указанные "объявленные общедоступные префиксы" и "одноранговый ASN" (или "клиент ASN") в реестре Интернет-маршрутизации. Если вы получаете общедоступные префиксы из другой сущности и если назначение не записано в реестре маршрутизации, автоматическая проверка не будет завершена и потребуется проверка вручную. Если автоматическая проверка не удалась, в выходных данных "Get-АзекспрессраутеЦиркуитпирингконфиг" будет отображаться "Адвертиседпубликпрефиксесстате" (требуется проверка) (см. раздел "получение сведений об пиринга Майкрософт" ниже). 
> 
> Если отображается сообщение "требуется проверка", собирайте документы, которые отображают открытые префиксы, для вашей организации назначается сущность, указанная в качестве владельца префиксов в реестре маршрутизации, и отправка этих документов для проверки вручную открыв запрос в службу поддержки, как показано ниже. 
> 
>

   Чтобы настроить пиринг Майкрософт для своего канала, воспользуйтесь следующим примером:

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv4 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

   Add-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv6 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "3FFE:FFFF:0:CD31::/120" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

### <a name="getmsft"></a>Получение сведений о пиринге Майкрософт

Для получения сведений о конфигурации можно использовать следующий пример:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="updatemsft"></a>Обновление конфигурации пиринга Майкрософт

С помощью следующего примера можно обновить любую часть конфигурации:

```azurepowershell-interactive
Set-AzExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv4 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv6 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "3FFE:FFFF:0:CD31::/120" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="deletemsft"></a>Удаление пиринга Майкрософт

Для удаления конфигурации пиринга выполните следующий командлет.

```azurepowershell-interactive
Remove-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="private"></a>Частный пиринг Azure

Этот раздел поможет вам создать, получить, обновить и (или) удалить конфигурацию частного пиринга Azure для канала ExpressRoute.

### <a name="to-create-azure-private-peering"></a>Создание частного пиринга Azure

1. Импортируйте модуль PowerShell для ExpressRoute.

   Чтобы начать использовать командлеты ExpressRoute, необходимо установить последнюю версию установщика Powershell из [коллекции PowerShell](https://www.powershellgallery.com/) и импортировать модули диспетчера ресурсов Azure в сеанс PowerShell. Будет необходимо запустить PowerShell от имени администратора.

   ```azurepowershell-interactive
   Install-Module Az
   ```

   Импортируйте все модули AZ.\* в пределах известного диапазона семантических версий.

   ```azurepowershell-interactive
   Import-Module Az
   ```

   Можно также просто импортировать выбранный модуль в рамках диапазона известных семантических версий.

   ```azurepowershell-interactive
   Import-Module Az.Network 
   ```

   Войдите в свою учетную запись.

   ```azurepowershell-interactive
   Connect-AzAccount
   ```

   Выберите подписку для создания канала ExpressRoute.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionId "<subscription ID>"
   ```
2. Создайте канал ExpressRoute.

   Выполните инструкции по созданию [канала ExpressRoute](expressroute-howto-circuit-arm.md). Поставщик услуг подключения должен подготовить его. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить для вас частный пиринг Azure. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала.

3. Убедитесь, что канал ExpressRoute подготовлен и включен. Используйте следующий пример:

   ```azurepowershell-interactive
   Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```

   Ответ будет выглядеть примерно так:

   ```
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"################################"
   ProvisioningState                : Succeeded
   Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
   CircuitProvisioningState         : Enabled
   ServiceProviderProvisioningState : Provisioned
   ServiceProviderNotes             : 
   ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
   ServiceKey                       : **************************************
   Peerings                         : []
   ```
4. Настройте для канала частный пиринг Azure. Прежде чем продолжить, убедитесь в наличии следующих элементов:

   * Подсеть /30 для основной ссылки. Эта подсеть не должна входить в адресное пространство, зарезервированное для виртуальных сетей.
   * Подсеть /30 для дополнительной ссылки. Эта подсеть не должна входить в адресное пространство, зарезервированное для виртуальных сетей.
   * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
   * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS. Для этого пиринга можно использовать частный номер AS. Не используйте номер 65515.
   * Необязательно:
     * Хэш MD5, если вы решите его использовать.

   Чтобы настроить для канала частный пиринг Azure, выполните код из следующего примера:

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

   Если вы решили использовать MD5-хэш, используйте следующий пример:

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
   ```

   > [!IMPORTANT]
   > Номер AS должен быть указан в качестве ASN пиринга, а не ASN клиента.
   > 
   >

### <a name="getprivate"></a>Получение сведений о частном пиринге Azure

Для получения сведений о конфигурации можно использовать следующий пример кода:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
```

### <a name="updateprivate"></a>Обновление конфигурации частного пиринга Azure

С помощью следующего примера можно обновить любую часть конфигурации. В приведенном ниже примере значение идентификатора виртуальной локальной сети для канала изменяется со 100 на 500.

```azurepowershell-interactive
Set-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="deleteprivate"></a>Удаление частного пиринга Azure

Для удаления конфигурации пиринга выполните следующий пример кода:

> [!WARNING]
> Перед запуском этого примера обязательно убедитесь, что все виртуальные сети и подключения Global Reach к ExpressRoute удалены. 
> 
> 

```azurepowershell-interactive
Remove-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="public"></a>Общедоступный пиринг Azure

Этот раздел поможет создать, получить, обновить и (или) удалить конфигурацию общедоступного пиринга Azure для канала ExpressRoute.

> [!Note]
> Общедоступный пиринг Azure устарел для новых каналов. Дополнительные сведения см. в разделе [пиринг ExpressRoute](expressroute-circuit-peerings.md).
>

### <a name="to-create-azure-public-peering"></a>Создание общедоступного пиринга Azure

1. Импортируйте модуль PowerShell для ExpressRoute.

   Чтобы начать использовать командлеты ExpressRoute, необходимо установить последнюю версию установщика Powershell из [коллекции PowerShell](https://www.powershellgallery.com/) и импортировать модули диспетчера ресурсов Azure в сеанс PowerShell. Будет необходимо запустить PowerShell от имени администратора.

   ```azurepowershell-interactive
   Install-Module Az
   ```

   Импортируйте все модули AZ.\* в пределах известного диапазона семантических версий.

   ```azurepowershell-interactive
   Import-Module Az
   ```

   Можно также просто импортировать выбранный модуль в рамках диапазона известных семантических версий.

   ```azurepowershell-interactive
   Import-Module Az.Network
   ```

   Войдите в свою учетную запись.

   ```azurepowershell-interactive
   Connect-AzAccount
   ```

   Выберите подписку для создания канала ExpressRoute.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionId "<subscription ID>"
   ```
2. Создайте канал ExpressRoute.

   Выполните инструкции по созданию [канала ExpressRoute](expressroute-howto-circuit-arm.md). Поставщик услуг подключения должен подготовить его. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить для вас общедоступный пиринг Azure. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала.

3. Убедитесь, что канал ExpressRoute подготовлен и включен. Используйте следующий пример:

   ```azurepowershell-interactive
   Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```

   Ответ будет выглядеть примерно так:

   ```
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"################################"
   ProvisioningState                : Succeeded
   Sku                              : {
                                      "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
   CircuitProvisioningState         : Enabled
   ServiceProviderProvisioningState : Provisioned
   ServiceProviderNotes             : 
   ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
   ServiceKey                       : **************************************
   Peerings                         : []
   ```
4. Настройте для канала общедоступный пиринг Azure. Прежде чем продолжить, убедитесь, что у вас есть следующие сведения:

   * Подсеть /30 для основной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4.
   * Подсеть /30 для дополнительной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4.
   * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
   * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS.
   * Необязательно:
     * Хэш MD5, если вы решите его использовать.

   Чтобы настроить для канала общедоступный пиринг Azure, выполните код из примера ниже

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

   Если вы решили использовать MD5-хэш, используйте следующий пример:

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

   > [!IMPORTANT]
   > Номер AS должен быть указан в качестве ASN пиринга, а не ASN клиента.
   > 
   >

### <a name="getpublic"></a>Получение сведений об общедоступном пиринге Azure

Для получения сведений о конфигурации можно использовать следующий командлет.

```azurepowershell-interactive
  $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="updatepublic"></a>Обновление конфигурации общедоступного пиринга Azure

С помощью следующего примера можно обновить любую часть конфигурации. В приведенном ниже примере значение идентификатора виртуальной локальной сети для канала изменяется с 200 на 600.

```azurepowershell-interactive
Set-AzExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="deletepublic"></a>Удаление общедоступного пиринга Azure

Для удаления конфигурации пиринга выполните следующий пример кода:

```azurepowershell-interactive
Remove-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a>Дальнейшие действия

Следующий шаг — [связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-arm.md).

* Дополнительную информацию о рабочих процессах ExpressRoute см. в статье [Процедуры ExpressRoute для подготовки каналов и состояний каналов](expressroute-workflows.md).
* Дополнительную информацию о пиринге канала см. в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).
* Подробнее о работе с виртуальными сетями см. в статье [Обзор виртуальных сетей](../virtual-network/virtual-networks-overview.md).
