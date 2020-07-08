---
title: Перенос профиля Azure CDN из Verizon Standard в Verizon Premium
description: Сведения о переносе профиля из Verizon категории "Стандартный" в Verizon категории "Премиум".
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 06/21/2018
ms.author: allensu
ms.custom: ''
ms.openlocfilehash: c1302822715a37ef1b85007130067ac02a8fd17a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "84887594"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Перенос профиля Azure CDN из Verizon категории "Стандартный" в Verizon категории "Премиум"

При создании профиля сети доставки содержимого (CDN) Azure для управления конечными точками Azure CDN предоставляет четыре доступных продукта. Сведения о различных продуктах и их функциях см. в разделе [Сравнение функций продуктов Azure CDN](cdn-features.md).

Если вы создали профиль **Azure CDN категории "Стандартный" из Verizon** и используете его для управления конечными точками, вы можете обновить этот профиль до **Azure CDN категории "Премиум" из Verizon**. При обновлении конечные точки CDN и все данные будут сохранены. 

> [!IMPORTANT]
> После обновления до профиля **Azure CDN категории "Премиум" из Verizon** вы не сможете преобразовать его в профиль **Azure CDN категории "Стандартный" из Verizon**.
> 

Чтобы обновить профиль до **Azure CDN категории "Стандартный" из Verizon**, обратитесь в [службу поддержки корпорации Майкрософт](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Сравнение профилей
Профили **Azure CDN категории "Премиум" из Verizon** имеют следующие основные отличия от профилей **Azure CDN категории "Стандартный" из Verizon**:
- Для некоторых функций Azure CDN, таких как [сжатие](cdn-improve-performance.md), [правила кэширования](cdn-caching-rules.md) и [геофильтрация](cdn-restrict-access-by-country.md) вы не можете использовать интерфейс Azure CDN и должны использовать портал Verizon, нажав кнопку **Управление**.
- API: в отличие от Verizon категории "Стандартный" вы не можете использовать API для управления функциями, доступ к которым осуществляется через портал Verizon категории "Премиум". Однако вы можете использовать API для управления другими распространенными функциями, такими как создание или удаление конечной точки, очистка или загрузка кэшированных ресурсов и включение или отключение пользовательского домена.
- Цены: структура ценообразования для передачи данных Verizon категории "Премиум" отличается от Verizon категории "Стандартный". Дополнительные сведения см. на странице [Цены на сеть доставки содержимого](https://azure.microsoft.com/pricing/details/cdn/).

Профили **Azure CDN категории "Премиум" из Verizon** включают следующие дополнительные возможности:
- [Проверка подлинности маркера](cdn-token-auth.md): позволяет пользователям получить и использовать маркер для доступа к защищенным ресурсам.
- [Обработчик правил](cdn-rules-engine.md): позволяет настраивать процесс обработки запросов HTTP.
- Средства расширенной аналитики:
   - [Подробный анализ запросов HTTP](cdn-advanced-http-reports.md)
   - [Анализ производительности пограничного узла](cdn-edge-performance.md)
   - [Анализ в реальном времени](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Дальнейшие шаги
Дополнительные сведения об обработчике правил см. в [Справке по обработчику правил Azure CDN](cdn-rules-engine-reference.md).

