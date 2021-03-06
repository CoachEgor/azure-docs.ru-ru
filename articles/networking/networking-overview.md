---
title: Обзор сетевых служб Azure
description: Сведения о сетевых службах в Azure, включая подключение, защиту приложений, доставку приложений и службы мониторинга сети.
services: networking
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/28/2020
ms.author: kumud
ms.openlocfilehash: f49a340a004a4aef37bcae9e3ae1c2b02ae030b9
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913095"
---
# <a name="azure-networking-services-overview"></a>Обзор сетевых служб Azure

Сетевые службы в Azure предоставляют разнообразные сетевые возможности, которые можно использовать совместно или отдельно. Щелкните одну из приведенных ниже ключевых возможностей, чтобы подробнее узнать о ней.
- [**Службы подключения**](#connect). Подключите ресурсы Azure и локальные ресурсы с помощью любой или комбинации этих сетевых служб в Azure — виртуальная сеть, ВИРТУАЛЬная Глобальная сеть, ExpressRoute, VPN-шлюз, шлюз NAT виртуальной сети, Azure DNS, служба пиринга и Azure бастиона.
- [**Службы защиты приложений**](#protect). Защитите свои приложения с помощью любой или комбинации этих сетевых служб в Azure-Private Link, от атак DDoS защиты, брандмауэра, групп безопасности сети, брандмауэра веб-приложения и конечных точек виртуальной сети.
- [**Службы доставки приложений**](#deliver). доставляют приложения в сети Azure с помощью любой или комбинации этих сетевых служб в Azure — сети доставки содержимого (CDN), службы "Передняя дверца Azure", диспетчера трафика, шлюза приложений, анализатора интернета и Load Balancer.
- [**Мониторинг**](#monitor)сети. Отслеживайте сетевые ресурсы с помощью любых или сочетаний сетевых служб в Azure — наблюдателя за сетями, монитора ExpressRoute, Azure Monitor или точки доступа к терминалу виртуальной сети (TAP).

## <a name="connectivity-services"></a><a name="connect"></a>Службы подключения
 
В этом разделе описываются службы, обеспечивающие подключение между ресурсами Azure, подключение из локальной сети к ресурсам Azure и подключение филиала в Azure — виртуальная сеть, ExpressRoute, VPN-шлюз, Виртуальная глобальная сеть, шлюз NAT виртуальной сети, Azure DNS, служба пиринга Azure и Azure бастиона.


### <a name="virtual-network"></a><a name="vnet"></a>Виртуальная сеть

Виртуальная сеть (VNet) Azure — это стандартный строительный блок для вашей частной сети в Azure. Виртуальных сетей можно использовать для:
- **Обмен данными между ресурсами Azure**. виртуальные машины и другие типы ресурсов Azure можно развернуть в виртуальной сети, например в средах службы приложений Azure, в службе Azure Kubernetes Service (AKS) и в масштабируемых наборах виртуальных машин Azure. Полный список ресурсов Azure, которые можно развернуть в виртуальной сети, см. в статье [Интеграция виртуальной сети для служб Azure](../virtual-network/virtual-network-for-azure-services.md).
- **Обмен данными между** собой. виртуальные сети можно подключать друг к другу, что позволяет ресурсам в виртуальной сети взаимодействовать друг с другом, используя пиринг виртуальных сетей. Виртуальные сети, которые вы подключаете, могут находиться в одном или в разных регионах Azure. Дополнительные сведения см. в статье [Пиринг между виртуальными сетями](../virtual-network/virtual-network-peering-overview.md).
- **Подключение к Интернету**. по умолчанию все ресурсы в виртуальной сети могут обмениваться данными по сети. Можно также установить входящее подключение к ресурсу, присвоив ему общедоступный IP-адрес, или общедоступный экземпляр Load Balancer. Для управления исходящими подключениями можно также использовать [общедоступные IP-адреса](../virtual-network/virtual-network-public-ip-address.md) или общедоступные [Load Balancer](../load-balancer/load-balancer-overview.md) .
- **Взаимодействие с локальными сетями**. Вы можете подключить локальные компьютеры и сети к виртуальной сети с помощью [VPN-шлюза](../vpn-gateway/vpn-gateway-about-vpngateways.md) или [ExpressRoute](../expressroute/expressroute-introduction.md).

Дополнительные сведения см. в статье [что такое виртуальная сеть Azure?](../virtual-network/virtual-networks-overview.md).

### <a name="expressroute"></a><a name="expressroute"></a>ExpressRoute
ExpressRoute позволяет расширять локальные сети в облако Майкрософт по частному соединению, которое обеспечивается поставщиком услуг подключения. Это подключение является закрытым. Трафик не проходит через Интернет. ExpressRoute позволяет устанавливать подключения к облачным службам Майкрософт, таким как Microsoft Azure, Microsoft 365 и Dynamics 365.  Дополнительные сведения см. в статье [что такое ExpressRoute?](../expressroute/expressroute-introduction.md).

:::image type="content" source="./media/networking-overview/expressroute-connection-overview.png" alt-text="Azure ExpressRoute" border="false":::

### <a name="vpn-gateway"></a><a name="vpngateway"></a>VPN-шлюз
VPN-шлюз помогает создавать зашифрованные распределенные подключения к виртуальной сети из локальных расположений или создавать зашифрованные подключения между виртуальных сетей. Существуют различные конфигурации для подключений VPN-шлюза, например, "сеть — сеть", "точка — сеть" или "Виртуальная сеть — виртуальная сети".
На следующей схеме показано несколько VPN-подключений типа "сеть — сеть" к одной виртуальной сети.

:::image type="content" source="./media/networking-overview/vpngateway-multisite-connection-diagram.png" alt-text="Подключения VPN-шлюза Azure типа &quot;сеть — сеть&quot;":::

Дополнительные сведения о различных типах VPN-подключений см. в разделе [VPN-шлюз](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="virtual-wan"></a><a name="virtualwan"></a>Виртуальная глобальная сеть
Виртуальная глобальная сеть Azure — это сетевая служба, которая обеспечивает оптимизированное и автоматизированное подключение типа "ветвь — ветвь" через Azure. Регионы Azure служат в качестве концентраторов, к которым вы можете подключить ветви. Вы можете использовать магистральную сеть Azure также для подключения ветвей и воспользоваться преимуществами подключения ветвей к виртуальной сети. Виртуальная глобальная сеть Azure объединяет многие службы облачного подключения Azure, такие как VPN типа "сеть — сеть", ExpressRoute, VPN-подключение "точка — сеть", в один рабочий интерфейс. Подключение к виртуальным сетям Azure устанавливается с помощью подключения по виртуальной сети. Дополнительные сведения см. в статье [что такое Виртуальная глобальная сеть Azure?](../virtual-wan/virtual-wan-about.md).

:::image type="content" source="./media/networking-overview/virtualwan1.png" alt-text="Схема Виртуальной глобальной сети":::

### <a name="azure-dns"></a><a name="dns"></a>Azure DNS
Azure DNS является службой размещения доменов DNS, осуществляющей разрешение имен на базе инфраструктуры Microsoft Azure. Размещая домены в Azure, вы можете управлять своими записями DNS с помощью тех же учетных данных, API и инструментов и оплачивать использование, как и другие службы Azure. Дополнительные сведения см. в разделе [что такое Azure DNS?](../dns/dns-overview.md).

### <a name="azure-bastion"></a><a name="bastion"></a>Бастион Azure
Служба Бастион Azure — это новая полностью управляемая платформой PaaS-служба, которая подготавливается в виртуальной сети. Она обеспечивает безопасное и бесперебойное подключение RDP или SSH к виртуальным машинам непосредственно на портале Azure по протоколу TLS. При подключении с помощью Бастиона Azure виртуальным машинам не требуются общедоступные IP-адреса. Дополнительные сведения см. в статье [что такое Azure бастиона?](../bastion/bastion-overview.md).

:::image type="content" source="./media/networking-overview/architecture.png" alt-text="Архитектура Azure бастиона":::

### <a name="virtual-network-nat-gateway"></a><a name="nat"></a>Шлюз NAT виртуальной сети
NAT (преобразование сетевых адресов) виртуальной сети упрощает возможность исходящего интернет-подключения для виртуальных сетей. При настройке NAT в подсети все исходящие подключения используют указанные статические общедоступные IP-адреса. Исходящее подключение возможно без подсистемы балансировки нагрузки или общедоступных IP-адресов путем прямого подключения к виртуальным машинам. Дополнительные сведения см. в статье [что такое шлюз NAT виртуальной сети?](../virtual-network/nat-overview.md).

:::image type="content" source="./media/networking-overview/flow-map.png" alt-text="Шлюз NAT виртуальной сети":::

### <a name="azure-peering-service"></a><a name="azurepeeringservice"></a> Служба пиринга Azure
Служба пиринга Azure расширяет возможности подключения клиентов к облачным службам Майкрософт, таким как службы Microsoft 365, Dynamics 365, программного обеспечения как услуга (SaaS), Azure или любые службы Майкрософт, доступные через общедоступный Интернет. Дополнительные сведения см. в статье [что такое служба пиринга Azure?](../peering-service/about.md).

### <a name="azure-edge-zones"></a><a name="edge-zones"></a>Пограничные зоны Azure

Пограничная зона Azure — это семейство предложений от Microsoft Azure, которое обеспечивает близкое к пользователю обработку данных. Вы можете развернуть виртуальные машины, контейнеры и другие выбранные службы Azure в пограничных зонах, чтобы устранить низкую задержку и требования к высокой пропускной способности приложений. Дополнительные сведения см. в статье [что такое зоны пограничных зон Azure?](edge-zones-overview.md).

### <a name="azure-orbital"></a><a name="orbital"></a>Azure Орбитал

Azure Orbital — это полностью управляемая облачная наземная станция, которая позволяет обмениваться данными между космическими аппаратами или группой спутников, передавать данные на борт и из него, обрабатывать данные в облаке, объединять службы в цепочки со службами Azure в уникальных сценариях и создавать продукты для ваших клиентов. Эта система создана на основе глобальной инфраструктуры Azure и глобальной волоконной сети с низкой задержкой. Дополнительные сведения см. в статье [что такое Azure Орбитал?](azure-orbital-overview.md).

:::image type="content" source="./media/azure-orbital-overview/orbital-communications-use-flow.png" alt-text="Схема обмена данными Azure Орбитал":::

## <a name="application-protection-services"></a><a name="protect"></a>Службы защиты приложений

В этом разделе описываются сетевые службы в Azure, помогающие защитить ваши сетевые ресурсы. Защитите свои приложения с помощью любой или комбинации этих сетевых служб в Azure-от атак DDoS Protection, частной ссылке, брандмауэре, брандмауэре веб-приложения, группах безопасности сети и конечных точках службы виртуальной сети.

### <a name="ddos-protection"></a><a name="ddosprotection"></a>Защита от атак DDoS 
[Защита Azure от атак DDoS](../virtual-network/manage-ddos-protection.md) обеспечивает контрмеры для самых сложных угроз от атак DDoS. Служба предоставляет расширенные возможности по устранению рисков от атак DDoS для приложения и ресурсов, развернутых в виртуальных сетях. Кроме того, клиенты, использующие службу защиты Azure от атак DDoS, имеют доступ к от атак DDoS быстрой реакции, чтобы привлечь экспертов от атак DDoS во время активной атаки.

:::image type="content" source="./media/networking-overview/ddos-protection.png" alt-text="Защита от атак DDoS":::

### <a name="azure-private-link"></a><a name="privatelink"></a>Приватный канал Azure
[Частная ссылка Azure](../private-link/private-link-overview.md) позволяет получить доступ к службам Azure PaaS (например, службе хранилища Azure и базе данных SQL), а также службам, принадлежащим заказчику, или партнерскую службу, размещенную в частной конечной точке в виртуальной сети.
Трафик между виртуальной сетью и службой проходит через магистральную сеть Майкрософт. Предоставление доступа к службе через общедоступный Интернет больше не требуется. Вы можете создать собственную службу приватного канала в виртуальной сети и предоставлять ее клиентам.

:::image type="content" source="./media/networking-overview/private-endpoint.png" alt-text="Обзор закрытой конечной точки":::

### <a name="azure-firewall"></a><a name="firewall"></a>Брандмауэр Azure
Брандмауэр Azure — это управляемая облачная служба сетевой безопасности, которая защищает ресурсы виртуальной сети Azure. С помощью брандмауэра Azure можно централизованно создавать, применять и регистрировать политики приложений и сетевых подключений в подписках и виртуальных сетях. Брандмауэр Azure использует статический общедоступный IP-адрес для виртуальных сетевых ресурсов, позволяя внешним брандмауэрам идентифицировать трафик, исходящий из виртуальной сети. 

Дополнительные сведения о брандмауэре Azure см. в [документации по брандмауэру Azure](../firewall/overview.md).

:::image type="content" source="./media/networking-overview/firewall-threat.png" alt-text="Общие сведения о брандмауэре":::

### <a name="web-application-firewall"></a><a name="waf"></a>Брандмауэр веб-приложения
[Брандмауэр веб-приложения Azure](../web-application-firewall/overview.md) (WAF) обеспечивает защиту веб-приложений от распространенных веб-эксплойтов и уязвимостей, таких как внедрение кода SQL и межсайтовые сценарии. Azure WAF обеспечивает стандартную защиту от OWASP первых 10 уязвимостей через управляемые правила. Кроме того, клиенты могут настраивать настраиваемые правила, которые являются управляемыми правилами клиентов для обеспечения дополнительной защиты на основе диапазона IP-адресов источника, и такие атрибуты запросов, как заголовки, файлы cookie, поля данных формы или параметры строки запроса.

Клиенты могут выбрать развертывание [Azure WAF с помощью шлюза приложений](../application-gateway/waf-overview.md) , который обеспечивает региональную защиту сущностей в общедоступном и частном адресном пространстве. Клиенты также могут развернуть [WAF Azure с помощью передней дверцы](../frontdoor/waf-overview.md) , которая обеспечивает защиту на границе сети до общедоступных конечных точек.

:::image type="content" source="./media/networking-overview/waf-overview.png" alt-text="Брандмауэр веб-приложения":::

### <a name="network-security-groups"></a><a name="nsg"></a>Группы безопасности сети
Отфильтровать трафик, поступающий из ресурсов Azure и обратно, в виртуальной сети можно с помощью группы безопасности сети. Дополнительные сведения см. в разделе [Группы безопасности сети](../virtual-network/network-security-groups-overview.md).

### <a name="service-endpoints"></a><a name="serviceendpoints"></a>Конечные точки службы
Конечные точки служб виртуальной сети расширяют пространство частных адресов и возможности идентификации вашей виртуальной сети в службах Azure благодаря прямому соединению. Конечные точки позволяют защищать критически важные ресурсы служб Azure в пределах отдельных виртуальных сетей. Трафик, поступающий из виртуальной сети в службу Azure, всегда остается в магистральной сети Microsoft Azure. Дополнительные сведения см. в статье [Конечные точки служб для виртуальной сети](../virtual-network/virtual-network-service-endpoints-overview.md).

:::image type="content" source="./media/networking-overview/vnet-service-endpoints-overview.png" alt-text="Конечные точки службы для виртуальной сети":::

## <a name="application-delivery-services"></a><a name="deliver"></a>Службы доставки приложений

В этом разделе описываются сетевые службы в Azure, которые помогают доставлять приложения — сеть доставки содержимого, служба "Передняя дверца Azure", диспетчер трафика, Load Balancer и шлюз приложений.

### <a name="content-delivery-network"></a><a name="cdn"></a>Сеть доставки содержимого
Azure CDN предлагает разработчикам глобальное решение для быстрой доставки больших объемов содержимого пользователям путем кэширования содержимого на стратегически расположенных физических узлах по всему миру. Дополнительные сведения о Azure CDN см. в статье [сеть доставки содержимого Azure](../cdn/cdn-overview.md).

:::image type="content" source="./media/networking-overview/cdn-overview.png" alt-text="Azure CDN":::

### <a name="azure-front-door-service"></a><a name="frontdoor"></a>Azure Front Door Service
Azure Front Door Service позволяет определять, управлять и отслеживать глобальную маршрутизацию для вашего веб-трафика посредством оптимизации для обеспечения наилучшей производительности и мгновенной глобальной отработки отказа для обеспечения высокой доступности. С помощью службы Front Door вы можете преобразовать глобальные (с поддержкой нескольких регионов) пользовательские и корпоративные приложения в современные, надежные, высокопроизводительные и персонализированные приложения, интерфейсы API и содержимое, которые охватывают глобальную аудиторию с помощью Azure. Дополнительные сведения см. в статье [Передняя дверца Azure](../frontdoor/front-door-overview.md).

:::image type="content" source="./media/networking-overview/front-door-visual-diagram.png" alt-text="Обзор службы &quot;Передняя дверь&quot;":::

### <a name="traffic-manager"></a><a name="trafficmanager"></a>Диспетчер трафика

Диспетчер трафика Azure — это подсистема балансировки нагрузки трафика на основе DNS, которая позволяет оптимально распределять трафик между службами во всех регионах Azure, обеспечивая высокий уровень доступности и скорости реагирования. Диспетчер трафика предоставляет ряд методов маршрутизации трафика для распределения трафика, такого как приоритет, взвешенный, производительность, географическая, многозначная или подсеть. Дополнительные сведения о методах маршрутизации трафика см. в разделе [методы маршрутизации диспетчера трафика](../traffic-manager/traffic-manager-routing-methods.md).

На следующей схеме показана маршрутизация на основе приоритета конечных точек с помощью диспетчера трафика.

:::image type="content" source="./media/networking-overview/priority.png" alt-text="Диспетчер трафика Azure: маршрутизация трафика по приоритету":::

Дополнительные сведения о диспетчере трафика см. в статье [что такое диспетчер трафика Azure?](../traffic-manager/traffic-manager-overview.md)

### <a name="load-balancer"></a><a name="loadbalancer"></a>Load Balancer
Azure Load Balancer обеспечивает высокую производительность и балансировку нагрузки уровня 4 с низкой задержкой для всех протоколов UDP и TCP. Она управляет и входящими, и исходящими подключениями. Вы можете настроить общедоступные и внутренние конечные точки с балансировкой нагрузки, а также определить правила для сопоставления входящих подключений к внутреннему пулу, используя параметры проверки состояний TCP и HTTP, чтобы управлять доступностью служб. Дополнительные сведения о Load Balancer см. в [этой статье](../load-balancer/load-balancer-overview.md).

На схеме ниже показано многоуровневое приложение с доступом к Интернету, использующее внешние и внутренние балансировщики нагрузки:

:::image type="content" source="./media/networking-overview/load-balancer.png" alt-text="Пример Azure Load Balancer":::

### <a name="application-gateway"></a><a name="applicationgateway"></a>Шлюз приложений
Шлюз приложений Azure — это подсистема балансировки нагрузки веб-трафика, предназначенная для управления трафиком веб-приложений. Это контроллер доставки приложений (ADC) в качестве службы, предлагающий различные возможности балансировки нагрузки уровня 7 для ваших приложений. Дополнительные сведения см. в статье [что такое шлюз приложений Azure?](../application-gateway/overview.md).

На следующей схеме показана маршрутизация на основе URL-пути с помощью шлюза приложений.

:::image type="content" source="./media/networking-overview/figure1-720.png" alt-text="Пример шлюза приложений":::

## <a name="network-monitoring-services"></a><a name="monitor"></a>Службы мониторинга сети
В этом разделе описываются сетевые службы в Azure, помогающие отслеживать сетевые ресурсы — наблюдатель за сетями, Azure Monitor для сетей, мониторов ExpressRoute, Azure Monitor и TAP для виртуальной сети.

### <a name="network-watcher"></a><a name="networkwatcher"></a>Наблюдатель за сетями
Наблюдатель за сетями Azure предоставляет инструменты для мониторинга, диагностики, просмотра метрик и включения или отключения журналов для ресурсов в виртуальной сети Azure. Дополнительные сведения см. в статье [что такое наблюдатель за сетями](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json).

### <a name="azure-monitor-for-networks-preview"></a>Предварительная версия Azure Monitor для сетей
Azure Monitor для сетей предоставляет исчерпывающее представление о работоспособности и метриках для всех развернутых сетевых ресурсов, не требуя настройки. Он также предоставляет доступ к возможностям мониторинга сети, таким как [монитор подключения](../network-watcher/connection-monitor-preview.md), [ведение журнала потоков для групп безопасности сети](../network-watcher/network-watcher-nsg-flow-logging-overview.md)и [аналитика трафика](../network-watcher/traffic-analytics.md). Дополнительные сведения см. в разделе [Azure Monitor for Networks Preview](../azure-monitor/insights/network-insights-overview.md?toc=%2fazure%2fnetworking%2ftoc.json).

### <a name="expressroute-monitor"></a><a name="expressroutemonitor"></a>Монитор ExpressRoute
Сведения о том, как просматривать метрики канала ExpressRoute, журналы ресурсов и оповещения, см. в разделе [мониторинг expressroute, метрики и оповещения](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="azure-monitor"></a><a name="azuremonitor"></a>Azure Monitor
Служба Azure Monitor обеспечивает максимальную доступность и производительность приложений, предоставляя полноценное решение для сбора, анализа и обработки данных телеметрии из облачных и локальных сред. Она поможет вам понять, как выполняются приложения, а также заранее определить проблемы, влияющие на них, и ресурсы, от которых они зависят. Дополнительные сведения см. в разделе [Azure Monitor обзор](../azure-monitor/overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="virtual-network-tap"></a><a name="vnettap"></a>КАСАНИе виртуальной сети
TAP (точка доступа к терминалу) виртуальной сети Azure позволяет непрерывно передавать сетевой трафик виртуальной машины в сборщик сетевых пакетов или средство аналитики. Средство сбора данных или анализа предоставляется партнером по [сетевым виртуальным устройствам](https://azure.microsoft.com/solutions/network-appliances/) .

На следующем рисунке показано, как работает КАСАНИе виртуальной сети.

:::image type="content" source="./media/networking-overview/virtual-network-tap-architecture.png" alt-text="Как работает TAP виртуальной сети":::

Дополнительные сведения см. в разделе [что такое касание виртуальной сети](../virtual-network/virtual-network-tap-overview.md).

## <a name="next-steps"></a>Дальнейшие действия

- Создайте первую виртуальную сеть и подключите к ней несколько виртуальных машин, выполнив действия, описанные в статье [Создание первой виртуальной сети](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) .
- Подключите компьютер к виртуальной сети, выполнив действия, описанные в [статье Настройка подключения типа "точка — сеть](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)".
- Реализуйте балансировку интернет-трафика на общедоступные серверы, выполнив действия, описанные в статье [Создание подсистемы балансировки нагрузки для Интернета на портале Azure](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json).
