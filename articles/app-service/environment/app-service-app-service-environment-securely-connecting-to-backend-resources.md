---
title: Подключение к серверной части v1
description: Подробные сведения о безопасном подключении к серверным ресурсам из среды службы приложений. Этот документ предоставляется только для клиентов, использующих устаревшую версию ASE (версию 1).
author: stefsch
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 9f8e288f771b9d584a0fd3430115f5fa60f68e47
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88961811"
---
# <a name="connect-securely-to-back-end-resources-from-an-app-service-environment"></a>Безопасное подключение к серверным ресурсам из Среды службы приложений
Так как среда службы приложений всегда создается **либо** в виртуальной сети Azure Resource Manager, **либо** в [виртуальной сети][virtualnetwork], использующей классическую модель развертывания, исходящие подключения из среды службы приложений к другим внутренним ресурсам могут передаваться исключительно по виртуальной сети. Начиная с июня 2016 среды ASE можно развернуть в виртуальных сетях, использующих либо общедоступные диапазоны адресов, либо АДРЕСНЫЕ пространства RFC1918 адресные пространства (частные адреса).  

Например, SQL Server может работать в кластере виртуальных машин с заблокированным портом 1433.  Конечная точка может быть ACLd, разрешая только доступ от других ресурсов в той же виртуальной сети.  

Другой пример: конфиденциальные конечные точки могут выполняться локально и быть подключены к Azure через подключение типа [сеть — сеть][SiteToSite] или канал [Azure ExpressRoute][ExpressRoute].  В результате только ресурсы в виртуальных сетях, подключенных к туннелям "сеть — сеть" или ExpressRoute, могут получить доступ к локальным конечным точкам.

Во всех этих сценариях приложения, работающие на Среда службы приложений, могут безопасно подключаться к различным серверам и ресурсам. Если исходящий трафик из приложений выполняется в Среда службы приложений в частные конечные точки в той же виртуальной сети или подключенные к одной виртуальной сети, она будет передаваться только через виртуальную сеть.  Исходящий трафик к частным конечным точкам не будет передаваться через общедоступный Интернет.

Одна из проблем связана с исходящим трафиком из Среда службы приложений в конечные точки в виртуальной сети. Среды службы приложений не могут достигать конечных точек виртуальных машин, расположенных в той **же** подсети, что и среда службы приложений. Это ограничение обычно не должно быть проблемой, если среды службы приложений развернуты в подсети, зарезервированной для использования исключительно Среда службы приложений.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Требования к DNS и исходящим подключениям
Для надлежащего функционирования среды службы приложений требуется исходящий доступ к различным конечным точкам. Полный список внешних конечных точек, используемых в ASE, приведен в разделе "Необходимое сетевое подключение" статьи [Сведения о конфигурации сети для сред службы приложений с ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

Для сред службы приложений требуется, чтобы для виртуальной сети была настроена допустимая инфраструктура DNS.  Если конфигурация DNS изменилась после создания Среда службы приложений, разработчики могут принудительно принудить Среда службы приложений выбрать новую конфигурацию DNS. В верхней части колонки управления Среда службы приложений на портале щелкните значок **Перезагрузка** , чтобы запустить многошаговую перезагрузку среды, в результате чего среда выберет новую конфигурацию DNS.

Также рекомендуется заранее настроить все пользовательские DNS-серверы в виртуальной сети перед созданием Среда службы приложений.  Если конфигурация DNS виртуальной сети изменилась во время создания Среда службы приложений, это приведет к сбою процесса создания Среда службы приложений. На другом конце шлюза VPN, если имеется пользовательский DNS-сервер, который недоступен или недоступен, процесс создания Среда службы приложений также завершится ошибкой.

## <a name="connecting-to-a-sql-server"></a>Подключение к SQL Server
В типичной конфигурации SQL Server конечная точка прослушивает порт 1433:

![Конечная точка сервера SQL Server][SqlServerEndpoint]

Существует два подхода к ограничению трафика к этой конечной точке.

* [Сетевые списки управления доступом][NetworkAccessControlLists] (сетевые ACL).
* [Группы безопасности сети][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Ограничение доступа с помощью сетевых списков управления доступом
Порт 1433 можно защитить с помощью сетевого списка управления доступом.  Приведенный ниже пример добавляет к разрешениям назначения адрес клиента, исходящий из виртуальной сети, и блокирует доступ ко всем остальным клиентам.

![Пример сетевого списка управления доступом][NetworkAccessControlListExample]

Все приложения, работающие в Среда службы приложений, в той же виртуальной сети, что и SQL Server, могут подключаться к экземпляру SQL Server. Используйте внутренний IP-адрес виртуальной **сети** для SQL Server виртуальной машины.  

Пример строки подключения ниже ссылается на SQL Server, использующий частный IP-адрес.

`Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient`

Хотя виртуальная машина также имеет общедоступную конечную точку, попытки подключения к общедоступному IP-адресу будут отклонены из-за сетевого списка ACL. 

## <a name="restricting-access-with-a-network-security-group"></a>Ограничение доступа с помощью группы сетевой безопасности
Альтернативный подход для обеспечения безопасного доступа заключается в использовании группы сетевой безопасности.  Группы сетевой безопасности можно применять к отдельным виртуальным машинам или к подсети, содержащей виртуальные машины.

Сначала необходимо создать группу безопасности сети.

```azurepowershell-interactive
New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"
```

С помощью группы безопасности сети можно легко ограничивать доступ только к внутреннему трафику виртуальной сети.  Правила по умолчанию в группе сетевой безопасности разрешают доступ только от других сетевых клиентов в той же виртуальной сети.

В результате блокировка доступа к SQL Server проста. Просто примените группу безопасности сети с правилами по умолчанию либо к виртуальным машинам, на которых работает SQL Server, либо к подсети, содержащей виртуальные машины.

В следующем примере группа сетевой безопасности применяется к содержащей подсети:

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
```

Окончательный результат представляет собой набор правил безопасности, блокирующих внешний доступ, при этом обеспечивается внутренний доступ к виртуальной сети.

![Правила сетевой безопасности по умолчанию][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Начало работы
Сведения о том, как начать работу со средами службы приложений, см. в статье [Введение в среду службы приложений][IntroToAppServiceEnvironment].

Дополнительные сведения об управлении входящим трафиком в среде службы приложений см. в разделе [Как управлять входящим трафиком в среде службы приложений][ControlInboundASE].

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: ../../virtual-network/virtual-networks-faq.md
[ControlInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-multi-site.md
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: /previous-versions/azure/virtual-network/virtual-networks-acl
[NetworkSecurityGroups]: ../../virtual-network/virtual-network-vnet-plan-design-arm.md
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[ControlInboundASE]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png