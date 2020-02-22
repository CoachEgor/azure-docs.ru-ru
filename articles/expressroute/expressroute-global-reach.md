---
title: 'Azure ExpressRoute: подключение к Microsoft Cloud с помощью Global Reach'
description: В этой статье даются общие сведения о службе Global Reach в ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: cherylmc
ms.openlocfilehash: f574576044b7e4ddd34289b5cc45fe5ca353f180
ms.sourcegitcommit: 163be411e7cd9c79da3a3b38ac3e0af48d551182
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2020
ms.locfileid: "77538510"
---
# <a name="expressroute-global-reach"></a>Global Reach ExpressRoute
ExpressRoute — частный и устойчивый способ подключения локальных сетей к Microsoft Cloud. Вы можете получить доступ ко многим облачным службам Майкрософт, таким как Azure, и Office 365 из частного центра обработки данных или корпоративной сети. Например, у вас есть филиал в Сан-Франциско с каналом ExpressRoute в Кремниевой долине и другой филиал в Лондоне с каналом ExpressRoute в том же городе. Оба филиала могут иметь высокоскоростное подключение к ресурсам Azure в западной части США и южной части Соединенного Королевства. Тем не менее филиалы не могут обмениваться данными напрямую друг с другом. Другими словами, 10.0.1.0/24 может отправлять данные в 10.0.3.0/24 и 10.0.4.0/24, но не в 10.0.2.0/24.

![без][1]

С помощью **ExpressRoute Global Reach** можно связать каналы ExpressRoute, чтобы настроить частную сеть между локальными сетями. В приведенном выше примере с добавлением ExpressRoute Global Reach офис в Сан-Франциско (10.0.1.0/24) может напрямую обмениваться данными с офисом в Лондоне (10.0.2.0/24) через имеющиеся каналы ExpressRoute и через глобальную сеть Майкрософт. 

![с][2]

## <a name="use-case"></a>Вариант использования
ExpressRoute Global Reach предназначен для дополнения реализации поставщика услуг глобальной сети и подключения ваших филиалов по всему миру. Например, если ваш поставщик услуг в основном работает в США и связал все филиалы в этой стране, но он не работает в Японии и Гонконге, то с помощью ExpressRoute Global Reach осуществляется работа с локальным поставщиком службы, а корпорация Майкрософт подключит филиалы к представленным в США, с помощью ExpressRoute и всемирной сети.

![вариант использования][3]

## <a name="availability"></a>Доступность 
В настоящее время ExpressRoute Global Reach поддерживается в следующих странах:

* Австралия
* Канада
* France
* Germany
* Гонконг (САР)
* Ирландия
* Япония
* Korea
* Нидерланды
* Новая Зеландия
* Сингапур
* Швейцария
* Соединенное Королевство
* Соединенные Штаты

Каналы ExpressRoute необходимо создавать в [расположениях пиринга ExpressRoute](expressroute-locations.md) в странах или регионах, указанных выше. Чтобы включить ExpressRoute Global Reach между [разными геополитическими регионами](expressroute-locations.md), у каналов должен быть номер SKU уровня "Премиум".

## <a name="next-steps"></a>Следующие шаги
1. [Получение дополнительных сведений об ExpressRoute Global Reach](expressroute-faqs.md).
2. [Сведения о настройке ExpressRoute Global Reach](expressroute-howto-set-global-reach.md)
3. [Связывание виртуальной сети Azure с каналом ExpressRoute](expressroute-howto-linkvnet-arm.md).


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "схема без Global Reach"
[2]: ./media/expressroute-global-reach/2.png "схема с Global Reach"
[3]: ./media/expressroute-global-reach/3.png "вариант использования глобального доступа"
