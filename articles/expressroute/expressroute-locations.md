---
title: Поставщики услуг подключения и расположения для Azure ExpressRoute | Документация Майкрософт
description: В этой статье приведена подробная информация о расположениях, где предлагаются услуги, и способах подключения к регионам Azure. Эта таблица отсортирована по поставщику услуг подключения.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/28/2019
ms.author: mialdrid
ms.openlocfilehash: 671fdafeb4bba871256968c6eebca73130a229b5
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252863"
---
# <a name="expressroute-partners-and-peering-locations"></a>Партнеры и одноранговые расположения ExpressRoute

> [!div class="op_single_selector"]
> * [Расположения по поставщикам](expressroute-locations.md)
> * [Поставщики по расположению](expressroute-locations-providers.md)


В данной статье приведены таблицы со сведениями о поставщиках услуг подключения ExpressRoute, географическом покрытии ExpressRoute, облачных службах Майкрософт, поддерживаемых через ExpressRoute, и системных интеграторах ExpressRoute.

## <a name="partners"></a>Поставщики услуг подключения ExpressRoute
ExpressRoute поддерживается во всех регионах и расположениях Azure. На следующей карте обозначены регионы Azure и расположения ExpressRoute. Расположения ExpressRoute соответствуют тем территориям, где Майкрософт взаимодействует с несколькими одноранговыми поставщиками услуг.

![Карта расположения][0]

Вы сможете получить доступ ко всем службам Azure во всех регионах соответствующего геополитического региона, если вы подключены по меньшей мере к одному расположению ExpressRoute в этом регионе.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Регионы Azure с расположениями ExpressRoute в пределах геополитических регионов
В следующей таблице сопоставлены регионы Azure с расположениями ExpressRoute в пределах геополитических регионов.

| **Геополитический регион** | **Зона** | **Регионы Azure** | **Расположения ExpressRoute** |
| --- | --- | --- | --- |
| **Государственные организации Австралии** | 1 | Центральная Австралия, Центральная Австралия 2 |Канберра, Канберра 2 |
| **Европа** | 1 |Центральная Франция, Южная Франция, Северная Европа, Западная Европа, западная часть Соединенного Королевства, южная часть Соединенного Королевства |Амстердам, Amsterdam2, Дублин, Франкфурт, Лондон, London2, Марселе, ньюпорт (Уэльс), Париж, Цюрих |
| **Северная Америка** | 1 |Восточная часть США, западная часть США, восточная часть США 2, западная часть США 2, центральная часть США, центрально-южная часть США, центрально-северная часть США, центрально-западная часть США, Центральная Канада, Восточная Канада |Атланта, Чикаго, Даллас, Денвер, Лас-Вегас, Лос-Анджелес, Майами, Нью-Йорк, Сан-Антонио, Сиэтл, кремниевая Долина, кремниевая Valley2, DC2 Вашингтон Округ Колумбия, штат Вашингтон, Монреаль, Квебек, Торонто |
| **Азия** | 2 |Восточная Азия, Юго-Восточная Азия |Гонконг (САР), Куала-Лумпур, Сингапур, Сингапур 2, Тайбэй |
| **Австралия** | 2 |Восточная Австралия, Юго-Восточная Австралия |Мельбурн, Перт, Сидней | 
| **Индия** | 2 |Западная Индия, Центральная Индия, Южная Индия |Ченнаи, Ченнаи 2, Мумбаи, Мумбаи 2 |
| **Япония** | 2 |Западная Япония, Восточная Япония |Осака, Токио |
| **Южная Корея** | 2 |Центральная Корея, Южная Корея |Пусан, Сеул|
| **ОБЪЕДИНЕННЫЕ АРАБСКИЕ ЭМИРАТЫ** | 3 | Центральная часть ОАЭ, Северная часть ОАЭ | Дубаи, Dubai2 |
| **ЮАР** | 3 |Запад ЮАР, Север ЮАР |Кейптаун, Йоханнесбург |
| **Южная Америка** | 3 |Южная часть Бразилии |Сан-Паулу |


### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Регионы и геополитические границы для национальных облаков
В таблице ниже содержатся сведения о регионах и геополитических границах для национальных облаков.

| **Геополитический регион** | **Регионы Azure** | **Расположения ExpressRoute** |
| --- | --- | --- |
| **Облако правительства США** |US Gov (Аризона), US Gov (Айова), US Gov (Техас), US Gov (Вирджиния), центральный регион US DoD, восточный регион US DoD  |Чикаго, Даллас, Нью-Йорк, Феникс, Сан-Антонио, Сиэтл, Кремниевая долина, Вашингтон (округ Колумбия) |
| **Восточный Китай** |"Восточный Китай", "Восточный Китай 2" |Шанхай, Шанхай 2 |
| **Северный Китай** |"Северный Китай", "Северный Китай 2" |Пекин, Пекин 2 |
| **Германия** |Центральная Германия, восточная Германия |Берлин, Франкфурт |

В стандартном номере SKU ExpressRoute подключение между геополитическими регионами не поддерживается. Для поддержки глобальных подключений необходимо включить надстройку ExpressRoute класса "Премиум". Подключение к национальным облачным средам не поддерживается. При необходимости вы можете работать с поставщиками услуг подключения.

## <a name="locations"></a>Расположения поставщиков услуг подключения

В следующей таблице показаны расположения по поставщикам услуг. См. дополнительные сведения о [службах доступных поставщиков по расположению](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Рабочая среда Azure

| **Поставщик услуг** | **Microsoft Azure** | **Office 365 и Dynamics 365** | **Расположения** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Поддерживаются |Поддерживаются |Мельбурн, Сидней |
| **[Airtel](https://www.airtel.in/creatingsmiles/)** | Поддерживаются | Поддерживаются | Ченнаи 2, Мумбаи 2 |
| **[Aryaka Networks](https://www.aryaka.com/)** |Поддерживаются |Поддерживаются |Амстердам, Чикаго, Даллас, Гонконг, САР, Сан-Паулу, Сиэтл, кремниевая Долина, Сингапур, Токио, Вашингтон Округ Колумбия |
| **[Ascenty Data Centers](https://ascenty.com/servicos/cloud-connect/microsoft-expressroute/)** |Поддерживаются |Поддерживаются |Сан-Паулу |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Поддерживаются |Поддерживаются |Амстердам, Вашингтон (округ Колумбия), Даллас, Кремниевая долина, Лондон, Сидней, Сингапур, Токио, Торонто, Чикаго |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Поддерживаются |Поддерживаются |Квебек, Монреаль, Торонто |
| **[British Telecom](https://www.globalservices.bt.com/en/solutions/products/bt-compute-for-microsoft-azure)** |Поддерживаются |Поддерживаются |Амстердам, Гонконг, Лондон, ньюпорт (Уэльс), Сан-Паулу, кремниевая Долина, Сингапур, Сидней, Токио, Вашингтон Округ Колумбия |
| **C3ntro** |Скоро |Скоро |Майами |
| **CDC** | Поддерживаются | Поддерживаются | Канберра, Канберра 2 |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Поддерживаются |Поддерживаются |Кремниевая долина, Лас-Вегас, Нью-Йорк, Сан-Антонио, Токио, Торонто |
| **Chief Telecom** |Поддерживаются |Поддерживаются |Тайбэй |
| **China Telecom Global** |Поддерживаются |Не поддерживается |Гонконг, САР |
| **[Cologix](https://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Поддерживаются |Поддерживаются |Чикаго, Даллас, Монреаль, Торонто, Вашингтон (округ Колумбия) |
| **[Colt](https://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Поддерживаются |Поддерживаются |Амстердам, Дублин, Лондон, Париж, Сингапур 2, Токио |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Поддерживаются |Поддерживаются |Чикаго, Кремниевая долина, Вашингтон, округ Колумбия |
| **[CoreSite](https://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Поддерживаются |Поддерживаются |Чикаго, Денвер, Лос-Анджелес, Нью-Йорк, кремниевая Долина, Silicon Valley2 Вашингтон Округ Колумбия, штат Вашингтон DC2 |
| **[DE-CIX](https://www.de-cix.net/en/de-cix-service-world/directcloud/find-a-cloud-service/detail/microsoft-azure)** | Поддерживаются |Поддерживаются |Amsterdam2, Франкфурт|
| **du datamena** |Поддерживаются |Поддерживаются | Dubai2 |
| **eir** |Поддерживаются |Поддерживаются |Дублин|
| **[Эпсилон глобальных каналов связи](https://www.epsilontel.com/solutions/direct-cloud-connect)** |Поддерживаются |Поддерживаются |Сингапур, Сингапур 2 |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Поддерживаются |Поддерживаются |Амстердам, Атланта, Чикаго, Даллас, Дублин, Гонконг, Лондон, London2, Лос-Анджелес, Мельбурн, Майами, Нью-Йорк, Осака, Париж, Сан-Паулу, Сиэтл, кремниевая Долина, Сингапур, Сидней, Токио, Торонто, Вашингтон Округ Колумбия |
| **Etisalat ОАЭ** |Поддерживаются |Поддерживаются |Дубай|
| **[euNetworks](https://eunetworks.com/services/solutions/cloud-connect/microsoft-azure-expressroute/)** |Поддерживаются |Поддерживаются |Амстердам, Дублин, Лондон |
| **FarEasTone** |Поддерживаются |Поддерживаются |Тайбэй|
| **GÉANT** |Поддерживаются |Поддерживаются |Амстердам |
| **[Global Cloud Xchange (GCX)](https://globalcloudxchange.com/cloud-platform/cloud-x-fusion/)** | Поддерживаются| Поддерживаются | Ченнаи, Мумбаи |
| **[InterCloud](https://www.intercloud.com/)** |Поддерживаются |Поддерживаются |Амстердам, Чикаго, Лондон, Нью-Йорк, Париж, кремниевая Долина, Сингапур, Вашингтон Округ Колумбия |
| **Internet2** |Поддерживаются |Поддерживаются |Чикаго, Даллас, Вашингтон (округ Колумбия) |
| **[Internet Initiative Japan Inc. - IIJ](https://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Поддерживаются |Поддерживаются |Осака, Токио |
| **[Internet Solutions - Cloud Connect](https://www.is.co.za/solution/cloud-connect/)** |Поддерживаются |Поддерживаются |Кейптаун, Йоханнесбург, Лондон |
| **[Interxion](https://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Поддерживаются |Поддерживаются |Амстердам, Amsterdam2, Дублин, Франкфурт, Лондон, Марселе, Париж, Цюрих |
| **[IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)**|Поддерживаются |Поддерживаются | Амстердам, London2, кремниевая Долина, Торонто |
| **[Jisc +](https://www.jisc.ac.uk/microsoft-azure-expressroute)** |Поддерживаются |Поддерживаются |Лондон |
| **[KINX](https://www.kinx.net/service/network/cloudhub/ms-expressroute/?lang=en)** |Поддерживаются |Поддерживаются |Сеул |
| **[Kordia](https://www.kordia.co.nz/cloudconnect)** | Поддерживаются |Поддерживаются |Сидней |
| **[KPN](https://www.kpn.com/zakelijk/cloud/connect.htm)** | Поддерживаются | Поддерживаются | Амстердам | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Поддерживаются |Поддерживаются |Амстердам, Вашингтон (округ Колумбия), Даллас, Кремниевая долина, Лондон, Ньюпорт (Уэльс), Сан-Паулу, Сингапур, Сиэтл, Чикаго |
| **LG CNS** |Поддерживаются |Поддерживаются |Пусан, Сеул |
| **[Liquid Telecom](https://www.liquidtelecom.com/products-and-services/cloud.html)** |Поддерживаются |Поддерживаются |Кейптаун, Йоханнесбург |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Поддерживаются |Поддерживаются |Амстердам, Атланта, Чикаго, Даллас, Денвер, Дублин, Гонконг, Лас-Вегас, Лондон, Лос-Анджелес, Мельбурн, Майами, Нью-Йорк, ПЕРТ, Квебек, Сан-Антонио, Сиэтл, кремниевая Долина, Сингапур, Сингапур 2, Сидней, Торонто, Вашингтон Округ Колумбия |
| **[MTN](https://www.mtnbusiness.com/en/enterprise/Pages/microsoft-express-route.aspx)** |Поддерживаются |Поддерживаются |Лондон |
| **[Neutrona Networks](http://www.neutrona.com/index.php/azure-expressroute/)** |Поддерживаются |Поддерживаются |Даллас, Лос-Анджелес, Майами, Сан-Паулу, Вашингтон Округ Колумбия |
| **[Next Generation Data](https://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Поддерживаются |Поддерживаются |Ньюпорт (Уэльс) |
| **[NEXTDC](https://www.nextdc.com/services/axon-ethernet/microsoft-expressroute)** |Поддерживаются |Поддерживаются |Мельбурн, Перт, Сидней |
| **[NTT Communications](https://www.ntt.com/en/services/network/virtual-private-network.html)** |Поддерживаются |Поддерживаются |Амстердам, Гонконг, Лондон, Лос-Анджелес, Осака, Сингапур, Сидней, Токио, Вашингтон Округ Колумбия |
| **[NTT EAST](https://flets.com/cloudgateway/crossconnect/)** |Поддерживаются |Поддерживаются |Токио |
| **[NTT SmartConnect](https://cloud.nttsmc.com/cxc/azure.html)** |Поддерживаются |Поддерживаются |Осака |
| **[Optus](https://www.optus.com.au/enterprise/networking/network-connectivity/express-link)** |Поддерживаются |Поддерживаются |Мельбурн, Сидней |
| **[Orange](https://www.orange-business.com/en/products/business-vpn-galerie)** |Поддерживаются |Поддерживаются |Амстердам, Гонконг, Лондон, Париж, Сан-Паулу, кремниевая Долина, Сингапур, Сидней, Токио, Вашингтон Округ Колумбия |
| **[PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)** |Поддерживаются |Поддерживаются |Чикаго, Кремниевая долина, Вашингтон, округ Колумбия |
| **[PCCW Global Limited](https://consoleconnect.com/clouds/#azureRegions)** |Поддерживаются |Поддерживаются |Чикаго, Гонконг, Лондон |
| **[Sejong Telecom](https://www.sejongtelecom.net/en/pages/service/cloud_ms)** |Поддерживаются |Поддерживаются |Сеул |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Поддерживаются |Поддерживаются |Ченнаи, Мумбаи 2 |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Поддерживаются |Поддерживаются |Сингапур, Сингапур 2 |
| **[Softbank](https://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Поддерживаются |Поддерживаются |Осака, Токио |
| **[Спринт](https://business.sprint.com/solutions/cloud-networking/)** |Поддерживаются |Поддерживаются |Чикаго, Кремниевая долина, Вашингтон, округ Колумбия |
| **[Tata Communications](https://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Поддерживаются |Поддерживаются |Амстердам, Ченнаи, Гонконг, Лондон, Мумбаи, Сан-Паулу, кремниевая Долина, Сингапур, Вашингтон Округ Колумбия |
| **TeleCity Group** |Поддерживаются |Поддерживаются |Амстердам |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Поддерживаются |Поддерживаются |Амстердам, Сан-Паулу |
| **[Telehouse - KDDI](https://www.telehouse.net/solutions/cloud-services/cloud-link)**  |Поддерживаются |Поддерживаются |Лондон |
| **Telenor** |Поддерживаются |Поддерживаются |Амстердам, Лондон |
| **[Telia Carrier](https://teliacarrier.com/our-services/connectivity/cloud-connect.html?title=Cloud%20Connect)** | Поддерживаются | Поддерживаются |Амстердам, Чикаго, Даллас, Лондон, Париж, Вашингтон Округ Колумбия |
| **Telmex Uninet**| Поддерживаются | Поддерживаются | Даллас |
| **[Telstra Corporation](https://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Поддерживаются |Поддерживаются |Мельбурн, Сингапур, Сидней |
| **[Telus](https://www.telus.com)** |Поддерживаются |Поддерживаются |Монреаль, Сиэтл, Торонто |
| **[Teraco](https://www.teraco.co.za/services/africa-cloud-exchange/)** |Поддерживаются |Поддерживаются |Кейптаун, Йоханнесбург |
| **[TIME dotCom](https://www.time.com.my/enterprise/connectivity/cloud-interconnect)** | Поддерживаются | Поддерживаются | Куала-Лумпур |
| **[UOLDIVEO](https://www.uoldiveo.com.br/)** |Поддерживаются |Поддерживаются |Сан-Паулу |
| **[Verizon](https://enterprise.verizon.com/products/network/application-enablement/secure-cloud-interconnect/)** |Поддерживаются |Поддерживаются |Амстердам, Чикаго, Даллас, Гонконг, Лондон, Мумбаи, кремниевая Долина, Сингапур, Сидней, Токио, Торонто, Вашингтон Округ Колумбия |
| **[Vodafone](https://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Поддерживаются |Поддерживаются |Лондон, Сингапур |
| **Vodafone Idea** | Поддерживаются | Поддерживаются | Мумбаи, Мумбаи 2 |
| **[Zayo](https://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Поддерживаются |Поддерживаются |Амстердам, Чикаго, Даллас, Денвер, Лондон, Лос-Анджелес, Монреаль, Нью-Йорк, Париж, Сиэтл, кремниевая Долина, Торонто, Вашингтон Округ Колумбия |

 **+** означает "скоро"

### <a name="national-cloud-environment"></a>Национальная облачная среда

### <a name="us-government-cloud"></a>Облако правительства США

| **Поставщик услуг** | **Microsoft Azure** | **Office 365** | **Расположения** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Поддерживаются |Поддерживаются |Чикаго, Phoenix, Вашингтон Округ Колумбия |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Поддерживаются |Поддерживаются |Нью-Йорк, Phoenix, Сан-Антонио |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Поддерживаются |Поддерживаются |Чикаго, Даллас, Нью-Йорк, Сиэтл, Кремниевая долина, Вашингтон (округ Колумбия) |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Поддерживаются |Поддерживаются |Чикаго, Кремниевая долина, Вашингтон, округ Колумбия |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Поддерживаются | Поддерживаются | Чикаго, Даллас, Сан-Антонио, Сиэтл, Вашингтон Округ Колумбия |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Поддерживаются |Поддерживаются |Чикаго, Даллас, Нью-Йорк, Кремниевая долина, Вашингтон (округ Колумбия) |

### <a name="china"></a>Китай

| **Поставщик услуг** | **Microsoft Azure** | **Office 365** | **Расположения** |
| --- | --- | --- | --- |
| **China Telecom** |Поддерживаются |Не поддерживается |Пекин, Shanghai2 Beijing2, Шанхай, |
| **[GDS](http://en.gds-services.com/news_detail/newsId=21.html)** |Поддерживаются |Не поддерживается |Пекин 2, Шанхай 2 |

Дополнительные сведения см. на странице [ExpressRoute в Китае](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Германия

| **Поставщик услуг** | **Microsoft Azure** | **Office 365** | **Расположения** |
| --- | --- | --- | --- |
| **[Colt](https://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Поддерживаются |Не поддерживается |Франкфурт |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Поддерживаются |Не поддерживается |Франкфурт |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Поддерживаются |Не поддерживается |Берлин |
| **Interxion** |Поддерживаются |Не поддерживается |Франкфурт |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Поддерживаются  | Не поддерживается | Берлин |
| **T-Systems** |Поддерживаются |Не поддерживается |Берлин |

## <a name="connectivity-through-exchange-providers"></a>Подключение через поставщиков Exchange

Вы можете создать подключение, даже если ваш поставщик услуг подключения не указан в предыдущих разделах.

* Узнайте у своего поставщика услуг подключения, подключен ли он к какому-либо Exchange, указанному в таблице выше. Дополнительные сведения об услугах, предлагаемых поставщиками Exchange, см. по ссылкам ниже. Несколько поставщиков услуг подключения уже подключены к серверам Ethernet Exchange.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equnix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](https://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](https://www.nextdc.com/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/) 
* Обратитесь к своему поставщику услуг подключения, чтобы он расширил вашу сеть, добавив необходимое пиринговое расположение.
  * Убедитесь, что поставщик услуг подключения расширяет границы вашего подключения, сохраняя высокую доступность во избежание влияния единых точек отказа.
* Закажите канал ExpressRoute с Exchange у вашего поставщика услуг подключения для связи с Майкрософт.
  * Для настройки подключения выполните действия, описанные в статье [Создание и изменение канала ExpressRoute с помощью PowerShell](expressroute-howto-circuit-classic.md) .

## <a name="connectivity-through-additional-service-providers"></a>Подключение через дополнительных поставщиков услуг

| **Поставщик услуг подключения** | **Exchange** | **Расположения** |
| --- | --- | --- |
| **[1CLOUDSTAR](https://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Сингапур |
| **[Airgate Technologies, Inc.](https://www.airgate.ca/expressroute)** | Equinix, Cologix | Торонто, Монреаль |
| **[Alaska Communications](https://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Сиэтл; |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |Нью-Йорк, Вашингтон (округ Колумбия) |
| **[Arteria Networks Corporation](https://www.arteria-net.com/business/service/cloud/sca/)** |Equinix |Токио |
| **[Axtel](https://alestra.mx/landing/expressrouteazure/)** |Equinix |Даллас|
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Лондон |
| **[BICS](https://bics.com/bics-solutions-suite/cloud-connect/)** | Equinix | Амстердам, Франкфурт, Лондон, Сингапур, Вашингтон (округ Колумбия) |
| **[BroadBand Tower, Inc.](https://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Токио |
| **[C3ntro Telecom](https://www.c3ntro.com/data/express-route)** | Equinix, Megaport | Даллас |
| **[Chief](https://www.chief.com.tw/dispPageBox/ct.aspx?ddsPageID=ENCLOUDSERVICE&dbid=4852937342)** | Equinix | Гонконг, САР |
| **[Cinia](https://www.cinia.fi/en/services/connectivity-services/direct-public-cloud-connection.html)** | Equinix, Megaport | Франкфурт, Германия |
| **[CloudXpress](https://www2.telenet.be/fr/business/produits-services/internet/cloudxpress/)** | Equinix | Амстердам | 
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Монреаль, Торонто |
| **[CoreAzure](http://coreazure.com/expressroute)**| Equinix | Лондон |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Даллас, Кремниевая долина, Вашингтон, округ Колумбия | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Даллас |
| **[Epsilon Telecommunications Limited](https://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | Лондон, Сингапур, Вашингтон, округ Колумбия |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Амстердам |
| **[Exponential E](https://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Лондон |
| **[Fastweb S.p.A](https://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Амстердам |
| **[Fibrenoire](https://www.fibrenoire.ca/en/cloudextn)** | Megaport | Город Квебек |
| **[Gtt Communications Inc](https://www.gtt.net)** |Equinix | Вашингтон, округ Колумбия |
| **[Gulf Bridge International](https://www.gbiinc.com/microsoft-azure-expressroute/)** | Equinix | Амстердам |
| **[HSO](https://www.hso.co.uk/products/cloud-direct)** |Equinix | Лондон, Слау |
| **[IVedha Inc](http://www.ivedha.com/cloud/manage-azure-cloud/express-route-4/)**| Equinix | Торонто |
| **LGA Telecom** |Equinix |Сингапур|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Вашингтон (округ Колумбия), Нью-Йорк, Чикаго |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Гонконг, САР |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Сидней |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Амстердам |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Вашингтон, округ Колумбия |
| **[MTN](https://www.mtnbusiness.com/en/enterprise/Pages/microsoft-express-route.aspx)** | Teraco | Кейптаун, Йоханнесбург |
| **[NexGen Networks](https://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Лондон |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Амстердам, Франкфурт |
| **[Post](https://www.teralinksolutions.com/cloud-connectivity/cloudbridge-to-azure-expressroute/)**|Equinix | Амстердам |
| **[Proximus](https://www.proximus.be/en/id_b_cl_proximus_external_cloud_connect/companies-and-public-sector/discover/magazines/expert-blog/proximus-external-cloud-connect.html)**|Equinix | Амстердам, Дублин, Лондон, Париж |
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Франкфурт |  
| **Rogers** | Cologix, Equinix | Монреаль, Торонто |
| **[Spectrum Enterprise](https://enterprise.spectrum.com/services/cloud.html)** | Equinix | Чикаго, Даллас, Лос-Анджелес, Нью-Йорк, кремниевая Долина | 
| **[Tamares Telecom](https://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | Лондон | 
| **[TDC Erhverv](https://tdc.dk/Produkter/cloudaccessplus)** | Equinix | Амстердам | 
| **[Telecom Italia Sparkle](https://www.tisparkle.com/our-platform/corporate-platform/sparkle-cloud-connect#catalogue)**| Equinix | Амстердам |
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Амстердам |
| **[ThinkTel](https://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Торонто | 
| **[Transtelco](https://www.transtelco.net/tcloud/microsoft)** |Equinix | Даллас, Лос-Анджелес |
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Сингапур |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Equinix | Сан-Паулу |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | Нью-Йорк |
| **[Windstream](https://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Чикаго, Кремниевая долина, Вашингтон, округ Колумбия |
| **Zain** |Equinix |Лондон|
| **[Zertia](https://www.zertia.es)**| Level 3 | Мадрид |
| **[Zirro](https://zirro.com/services/)**| Cologix, Equinix | Монреаль, Торонто |

## <a name="connectivity-through-datacenter-providers"></a>Подключение с помощью поставщиков центров обработки данных

| **Поставщик** | **Exchange** |
| --- | --- |
| **[CyrusOne](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport, PacketFabric |
| **[Cyxtera](https://www.cyxtera.com/data-center-services/interconnection)** | Megaport, PacketFabric |
| **[Банк данных](https://www.databank.com/platforms/connectivity/cloud-direct-connect/)** | Megaport |
| **[DataFoundry](https://www.datafoundry.com/services/cloud-connect/)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | IX Reach, Megaport PacketFabric |
| **[EdgeConnex](https://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport, PacketFabric |
| **[Flexential](https://www.flexential.com/connectivity/cloud-connect-microsoft-azure-expressroute)** | IX Reach, Megaport, PacketFabric |
| **[QTS центров обработки данных](https://www.qtsdatacenters.com/hybrid-solutions/connectivity/azure-cloud )** | Megaport, PacketFabric |
| **[Stream центров обработки данных]( https://www.streamdatacenters.com/products-services/network-cloud/ )** | Megaport |
| **[RagingWire Data Centers](https://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach, Megaport, PacketFabric |
| **[vXchnge](https://www.vxchnge.com/colocation-services/interconnection)** | IX Reach, Megaport |
| **[T5 Datacenters](https://t5datacenters.com/network-cloud-connect/)** | IX Reach |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Подключение через национальные исследовательские и образовательные сети (NREN)

| **Поставщик**|
| --- |
| **AARNET**| 
| **DeIC, через GÉANT**|
| **GARR, через GÉANT**|
| **GÉANT**|
| **HEAnet, через GÉANT**|
| **Internet2**|
| **JISC**|
| **RedIRIS, через GÉANT**|
| **SINET**|
| **Surfnet, через GÉANT**|

* Если вашего поставщика услуг подключения нет в списке, проверьте, подключены ли они к любому из перечисленных выше партнеров ExpressRoute Exchange.

## <a name="expressroute-system-integrators"></a>Системные интеграторы ExpressRoute
Возможность частного подключения, соответствующего вашим потребностям, будет зависеть от масштаба сети. Чтобы упростить переход на ExpressRoute, вы можете обратиться к одному из системных интеграторов, указанных в таблице ниже.

| **Системный интегратор** | **Континент** |
| --- | --- |
| **[Altogee](https://altogee.be/diensten/express-route/)** | Европа |
| **[Avanade Inc.](https://www.avanade.com/)** | Азия, Европа, Северная Америка, Южная Америка |
| **[Bright Skies GmbH](https://bskies.io/expressroute)** | Европа
| **[Ensyst](https://www.ensyst.com.au)** | Азия
| **[Equinix Professional Services](https://www.equinix.com/services/consulting/)** | Северная Америка |
| **[FlexManage](https://www.flexmanage.com/cloud)** | Северная Америка |
| **[Lightstream](https://www.lightstream.tech/partners/microsoft-azure/)** | Северная Америка |
| **[The IT Consultancy Group](https://itconsult.com.au/)** | Австралия |
| **[MOQdigital](https://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Австралия |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Европа (Германия) |
| **[Nelite](https://www.nelite.com/offres-services/)** | Европа |
| **[New Signature](https://newsignature.com/technologies/express-route/)** | Европа |
| **[OneAs1a](https://www.oneas1a.com/connectivity.html)** | Азия |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Европа |
| **[Perficient](https://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Северная Америка |
| **[Presidio](https://info.presidio.com/microsoft-azure-expressroute)** | Северная Америка |
| **[sol-tec](https://www.sol-tec.com/services)** | Европа |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Северная Америка |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Австралия |

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения об ExpressRoute см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).
* Убедитесь, что выполнены все необходимые условия. Ознакомьтесь с разделом [Предварительные требования и контрольный список для ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Карта расположения"
